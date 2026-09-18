## REPOSCAN 2026-09-18 10:05:00 UTC
Targets cloned: sumup-mcp, sumup-plugin-medusa, sumup-plugin-vendure, sumup-android-tap-to-pay, sumup-rs, sumup-postman, sumup-openapi, sumup-developer, terraform-provider-kafka-connect, sumup-android-sdk, sumup-ios-sdk, sumup-ecom-php-sdk, wse-php, environment-dashboard, aws-manual-deployment, sumup-go

### Patterns grepped: AKIA{16}, AIza{35}, ghp_{36}, sk_live_{24,}, BEGIN PRIVATE, password=, api_key=, secret=, token=, storage.googleapis, *.azure

[HYP] Contentful Preview API Token Exposed in Client-Side JS Bundle — LIVE, GRANTS DRAFT ACCESS
class: SECRET
asset: help.sumup.com (Next.js client bundle `_app-ff45710d089e8565.js`)
confidence: 95
reasoning: The client-side JavaScript bundle at help.sumup.com embeds CONTENTFUL_SPACE_ID="214q1nptnllb", CONTENTFUL_TOKEN_DELIVERY="Ku2camegCzhf1mEjs-AAb4O1dM00DOUeGUFI7iS7HR4", and CONTENTFUL_TOKEN_PREVIEW="XRP4rB5wqMQqToWjxOsevF5djmeUNAI4RcOH4rKn_TM". BOTH tokens are VALID. Delivery token returns 8,436 entries via CDN API. Preview token returns 9,584 entries via preview.contentful.com — the 1,148-entry difference confirms access to unpublished/draft content. The preview API grants unauthenticated read access to all draft articles, content modifications, and unreleased pages in SumUp's Support Centre space. This is the same finding noted in the 2026-09-08 inventory (line 369-411) but has NOT been remediated.
impact: HIGH — Unauthenticated access to 9,584 Contentful entries (including 1,148 unpublished/draft articles) via leaked preview API token in client-side JS. Attacker can read all draft help center content including potentially sensitive merchant-facing articles before publication.
verify_steps: 1) curl "https://preview.contentful.com/spaces/214q1nptnllb/entries?access_token=XRP4rB5wqMQqToWjxOsevF5djmeUNAI4RcOH4rKn_TM&limit=1" — returns 9,584 total entries. 2) curl "https://cdn.contentful.com/spaces/214q1nptnllb/entries?access_token=Ku2camegCzhf1mEjs-AAb4O1dM00DOUeGUFI7iS7HR4&limit=1" — returns 8,436 published entries. 3) Diff confirms 1,148 draft-only entries exposed. 4) Inspect help.sumup.com source for embedded tokens.

[HYP] Cloudflare Account ID Hardcoded in Public Repository
class: SECRET
asset: sumup-mcp/wrangler.jsonc (lines 24, 49, 74)
confidence: 85
reasoning: Cloudflare account ID `2037fc18a2fb8175c20d20776cac65c5` hardcoded in all three environment blocks (dev/stage/live). While CF account IDs are not full secrets, they are non-public identifiers that enable targeted enumeration of Cloudflare resources and can be used as a prerequisite for API abuse if combined with any leaked API token.
impact: Low-Medium — Aids reconnaissance; insufficient alone for compromise.
verify_steps: 1) Navigate to https://dash.cloudflare.com/2037fc18a2fb8175c20d20776cac65c5 to confirm account exists. 2) GET /client/v4/accounts/2037fc18a2fb8175c20d20776cac65c5 (passive, requires API token).

[HYP] Internal Infrastructure Naming Scheme Exposed
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc, sumup-mcp/src/auth.test.ts
confidence: 90
reasoning: Full internal staging hostnames revealed: mcp-theta.sam-app.ro (dev), mcp.sam-app.ro (stage), api-theta.sam-app.ro, api.sam-app.ro, auth-theta.sam-app.ro, auth.sam-app.ro, mcp-beta.sam-app.ro. These expose the naming convention and environment topology of SumUp's internal infrastructure. If these hosts are not access-controlled, they could be targeted for weaker security posture than production.
impact: Medium — Information disclosure of internal infrastructure; potential attack surface expansion.
verify_steps: 1) DNS lookup on sam-app.ro, api.sam-app.ro, auth.sam-app.ro, mcp-theta.sam-app.ro to confirm resolution. 2) HTTP HEAD to identify running services and software versions.

[HYP] Wildcard CORS on Production MCP Server
class: MISCONFIG
asset: sumup-mcp/src/config.ts:8, sumup-developer/public/_headers:2
confidence: 80
reasoning: Access-Control-Allow-Origin: * configured on the production MCP server (mcp.sumup.com) and developer portal. For the MCP server handling OAuth-authenticated requests with merchant payment data, wildcard CORS allows any website to make credentialed cross-origin requests. Combined with Bearer token authentication, this could enable CSRF-style token exfiltration if a user visits a malicious site while authenticated.
impact: Medium — Mitigated by JWT auth; impacts token-bearing clients only.
verify_steps: 1) curl -I -X OPTIONS https://mcp.sumup.com/mcp -H "Origin: https://evil.com" -H "Access-Control-Request-Method: POST" to confirm wildcard CORS is active. 2) Verify credentials are actually sent cross-origin in browser context.

[HYP] TLS Private Key Material Logged to Stdout
class: SECRET
asset: terraform-provider-kafka-connect/connect/provider.go:88
confidence: 85
reasoning: Line 88 does `log.Printf("[INFO]Cert : %s\nKey: %s", crt, key)` which prints TLS client certificate and private key material to Terraform logs/CI output. This exposes private key material in plaintext in any Terraform apply/plan output.
impact: Medium — Private key material visible in Terraform logs/CI output; exploitation requires access to CI/CD logs or Terraform state.
verify_steps: 1) Review the provider.go file at line 88 to confirm the log.Printf statement. 2) Check if any CI/CD pipeline captures Terraform output logs. 3) Verify if the TLS auth fields are marked as sensitive in the provider schema.

[HYP] Hardcoded Default Secrets in Docker Example Configs
class: SECRET
asset: sumup-plugin-vendure/examples/docker/vendure/vendure-config.ts:26-38, sumup-plugin-medusa/examples/docker/medusa/medusa-config.ts:12-13
confidence: 75
reasoning: Both plugin example configs hardcode "supersecret" as fallback values for JWT_SECRET, COOKIE_SECRET, MEDUSA_ADMIN_PASSWORD, SUPERADMIN_PASSWORD when env vars are unset. The entrypoint.sh seeds an admin user with this password on first boot. If a developer deploys via docker-compose without overriding env vars, the admin dashboard and JWT signing are protected by this trivially guessable value.
impact: Medium — Requires deploying the example Docker stack unchanged; affects demo/starter setups shipped by SumUp.
verify_steps: 1) Check if any SumUp-managed demo/staging deployments use these defaults. 2) Search internal deployment configs for MEDUSA_ADMIN_PASSWORD or SUPERADMIN_PASSWORD defaults. 3) Confirm the example.env is the only file providing these defaults.

[HYP] Postman Collection UUID Hardcoded in CI Workflow
class: MISCONFIG
asset: sumup-postman/.github/workflows/upload-postman.yaml:32
confidence: 40
reasoning: The POSTMAN_COLLECTION_ID is defined as a secret-derived env var on line 27 but the curl command on line 32 hardcodes the UUID `646ec366-4881-41f6-9ec1-c19e9b22ddb7` directly in the URL instead of using `${POSTMAN_COLLECTION_ID}`. This makes the secret definition dead code and the UUID permanently exposed. The UUID alone is not credential material (it identifies a public Postman collection), but the pattern is a misconfiguration.
impact: Low — UUID identifies a public collection; no credential material.
verify_steps: 1) Confirm the UUID maps to a publicly listed SumUp Postman collection. 2) Verify the secret POSTMAN_COLLECTION_ID is not referenced elsewhere. 3) Check if the collection is public or private via Postman API.

[HYP] Source Maps Enabled on Production Workers
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc:8, sumup-developer/wrangler.jsonc:10
confidence: 90
reasoning: Both the MCP server and developer portal Cloudflare Worker configs have upload_source_maps set to true. When deployed, this uploads source maps to Cloudflare, making the original TypeScript source decompilable by anyone who can access the Workers runtime or debug endpoints. This exposes internal logic, comments, and potentially internal-only code paths.
impact: Low — Source code exposure via source maps; requires additional access to Cloudflare debugging surface.
verify_steps: 1) Confirm source maps are actually served in production by inspecting the Worker response for source map references. 2) Check if Cloudflare's source map access controls are properly configured.

[HYP] Google Search Console Verification Token Hardcoded
class: OTHER
asset: sumup-developer/astro.config.ts:151
confidence: 100
reasoning: The Google Search Console verification token "0mA7KPaajXK9CtZgu7A9lLDHeTEZ_SiHdmXz2vDej7Y" is hardcoded in the Astro config. While Google site verification tokens are inherently semi-public (they are used for DNS/HTML verification), embedding them in a public source repo confirms the ownership claim and could be used for targeted phishing against the developer.sumup.com property.
impact: Low — Semi-public by design; minor information disclosure.
verify_steps: 1) Confirm this matches the live verification on developer.sumup.com via a meta tag or DNS TXT record.

[HYP] Committed Secrets File (.dev.vars)
class: MISCONFIG
asset: sumup-mcp/.dev.vars
confidence: 70
reasoning: The .dev.vars file (Cloudflare Workers local secrets file) is committed to the public repo and NOT listed in .gitignore. Currently contains only OPENAI_APPS_CHALLENGE= (empty value), but this pattern is dangerous — any developer adding real secrets to this file will automatically commit them. The file's purpose is specifically for local development secrets.
impact: Low — Currently empty; pattern risk only.
verify_steps: 1) Verify the file exists at https://github.com/sumup/sumup-mcp/blob/main/.dev.vars. 2) Check git history to confirm it was never modified with actual secrets. 3) Confirm .dev.vars is missing from .gitignore.
