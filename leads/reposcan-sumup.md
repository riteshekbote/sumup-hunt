## REPOSCAN 2026-09-03 16:33:22 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-03 19:25:11 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-03 21:55:24 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-03 23:43:42 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 02:14:52 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 07:16:02 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 12:09:22 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 16:29:46 UTC
class: SECRET
asset: `sumup-mcp/wrangler.jsonc` (lines 24, 49, 74)
confidence: 85
reasoning: Cloudflare account ID `2037fc18a2fb8175c20d20776cac65c5` hardcoded in all three environment blocks (dev/stage/live). Account IDs are used in Cloudflare API calls and dashboard URLs. Not a full credential but enables targeted enumeration of Cloudflare resources.
impact: Low-Medium
verify_steps: 1) Navigate to `https://dash.cloudflare.com/2037fc18a2fb8175c20d20776cac65c5` to confirm the account exists. 2) Use Cloudflare API `GET /client/v4/accounts/2037fc18a2fb8175c20d20776cac65c5` to enumerate available services (requires API token - passive verification).
class: MISCONFIG
asset: `sumup-mcp/wrangler.jsonc`, `sumup-mcp/src/auth.test.ts`
confidence: 90
reasoning: Full internal staging/development infrastructure naming scheme exposed: `sam-app.ro` domain with subdomains `mcp-theta.sam-app.ro` (dev), `mcp.sam-app.ro` (stage), `api-theta.sam-app.ro`, `api.sam-app.ro`, `auth-theta.sam-app.ro`, `auth.sam-app.ro`, `mcp-beta.sam-app.ro`. Reveals infrastructure naming convention, environment topology, and staging API hosts. An attacker can enumerate and probe these non-production environments.
impact: Medium
verify_steps: 1) DNS lookup on `sam-app.ro`, `api.sam-app.ro`, `auth.sam-app.ro`, `mcp-theta.sam-app.ro` to confirm they resolve. 2) HTTP HEAD/GET to these hosts to identify running services and software versions. 3) Check if staging environments have weaker security controls than production.
class: MISCONFIG
asset: `sumup-mcp/src/config.ts:8`, `sumup-developer/public/_headers:2`
confidence: 80
reasoning: `Access-Control-Allow-Origin: *` configured on the production MCP server (`mcp.sumup.com`) and developer portal. For the MCP server handling OAuth-authenticated requests with merchant payment data, wildcard CORS allows any website to make credentialed cross-origin requests. Combined with the Bearer token authentication, this could enable CSRF-style token exfiltration if a user visits a malicious site while authenticated.
impact: Medium
verify_steps: 1) `curl -I -X OPTIONS https://mcp.sumup.com/mcp -H "Origin: https://evil.com" -H "Access-Control-Request-Method: POST"` to confirm wildcard CORS is active. 2) Verify that credentials (cookies/Authorization headers) are actually sent cross-origin in browser context. 3) Test if the MCP server respects the Origin header for any state-changing operations.
class: MISCONFIG
asset: `sumup-mcp/.dev.vars`
confidence: 70
reasoning: The `.dev.vars` file (Cloudflare Workers local secrets file) is committed to the public repo and NOT listed in `.gitignore`. Currently contains only `OPENAI_APPS_CHALLENGE=` (empty value), but this pattern is dangerous - any developer adding real secrets to this file will automatically commit them. The file's purpose is specifically for local development secrets.
impact: Low
verify_steps: 1) Verify the file exists at `https://github.com/sumup/sumup-mcp/blob/main/.dev.vars`. 2) Check git history to confirm it was never modified with actual secrets. 3) Confirm `.dev.vars` is missing from `.gitignore`.
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 19:12:31 UTC
class: SECRET
asset: sumup-plugin-medusa/examples/docker/medusa/medusa-config.ts:12-13
confidence: 75
reasoning: JWT_SECRET, COOKIE_SECRET, MEDUSA_ADMIN_PASSWORD, SUPERADMIN_PASSWORD all default to "supersecret" when env vars are unset. These are real fallback values in production-adjacent config (not test mocks). The entrypoint.sh seeds an admin user with this password on first boot. If a developer deploys via docker-compose without overriding env vars, the admin dashboard and JWT signing are protected by this trivially guessable value.
impact: medium — requires deploying the example Docker stack unchanged; affects demo/starter setups shipped by SumUp
verify_steps: (1) Check if any SumUp-managed demo/staging deployments use these defaults; (2) Search internal deployment configs for MEDUSA_ADMIN_PASSWORD or SUPERADMIN_PASSWORD defaults; (3) Confirm the example.env is the only file providing these defaults
class: SECRET
asset: sumup-plugin-vendure/examples/docker/example.env:8-9
confidence: 70
reasoning: POSTGRES_USER=vendure, POSTGRES_PASSWORD=vendure are hardcoded defaults used as env var fallbacks. Combined with HYP-001, a docker-compose up exposes Postgres with trivial credentials. The docker-compose.yml uses these as variable defaults too.
impact: low — only affects local/example Docker deployments; Postgres is not directly internet-facing in this config
verify_steps: (1) Confirm no SumUp-managed instances use these defaults; (2) Check if the Postgres port is exposed externally in any deployment
class: MISCONFIG
asset: sumup-android-tap-to-pay/build.gradle.kts:18
confidence: 90
reasoning: The URL `https://tap-to-pay-sdk.fleet.live.sumup.net/` reveals an internal Maven artifact server hostname and its infrastructure naming convention (Fleet = likely a continuous delivery platform). This endpoint serves the proprietary Tap-to-Pay SDK binary. While the server requires Maven credentials (env-var based), the hostname itself leaks internal infrastructure details that could aid targeted attacks against SumUp's build/deploy infrastructure.
impact: low — the endpoint requires credentials; the URL itself is not exploitable but aids recon
verify_steps: (1) Confirm this is the only public reference; (2) Check if the Fleet server has additional exposed endpoints; (3) Verify auth requirements on this Maven repo
class: MISCONFIG
asset: sumup-rs/sdk/tests/client.rs:59-60
confidence: 85
reasoning: The string `https://mock.sumup.internal` appears in test code, revealing an internal DNS naming convention for mock services. This is test-only code but leaks infrastructure naming patterns.
impact: very low — purely informational; test code only
verify_steps: (1) Verify the internal domain doesn't resolve from outside; (2) Check if more internal hostnames appear in other repos
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 21:31:25 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-04 23:15:18 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 01:00:41 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 05:27:11 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 09:19:29 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 12:47:34 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 15:43:48 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 17:50:17 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 19:45:21 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 21:51:09 UTC
[HYP] Cloudflare Account ID Hardcoded in wrangler.jsonc
class: SECRET
asset: sumup-mcp/wrangler.jsonc (lines 24, 49, 74)
confidence: 85
reasoning: Cloudflare account ID 2037fc18a2fb8175c20d20776cac65c5 hardcoded in all three env blocks (dev/stage/live). Enables targeted enumeration of Cloudflare resources.
impact: Low-Medium
verify_steps: 1) Navigate to https://dash.cloudflare.com/2037fc18a2fb8175c20d20776cac65c5 2) GET /client/v4/accounts/2037fc18a2fb8175c20d20776cac65c5 (passive)
[HYP] Internal Infrastructure Naming Scheme Exposed
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc, sumup-mcp/src/auth.test.ts
confidence: 90
reasoning: Full internal naming convention exposed: sam-app.ro domain with subdomains mcp-theta.sam-app.ro (dev), mcp.sam-app.ro (stage), api-theta.sam-app.ro, api.sam-app.ro, auth-theta.sam-app.ro, auth.sam-app.ro, mcp-beta.sam-app.ro.
impact: Medium
verify_steps: 1) DNS lookup on sam-app.ro, api.sam-app.ro, auth.sam-app.ro, mcp-theta.sam-app.ro 2) HTTP HEAD to identify running services
[HYP] Wildcard CORS on Production MCP Server
class: MISCONFIG
asset: sumup-mcp/src/config.ts:8, sumup-developer/public/_headers:2
confidence: 80
reasoning: Access-Control-Allow-Origin: * on mcp.sumup.com (OAuth-authenticated MCP server with merchant payment data). Any website can make credentialed cross-origin requests; could enable CSRF token exfiltration.
impact: Medium
verify_steps: 1) curl -I -X OPTIONS https://mcp.sumup.com/mcp -H "Origin: https://evil.com" -H "Access-Control-Request-Method: POST" 2) Verify credentials sent cross-origin
[HYP] Committed Secrets File (.dev.vars)
class: MISCONFIG
asset: sumup-mcp/.dev.vars
confidence: 70
reasoning: Cloudflare Workers local secrets file committed to public repo, not in .gitignore. Currently empty (OPENAI_APPS_CHALLENGE=) but dangerous pattern — any developer adding real secrets auto-commits them.
impact: Low
verify_steps: 1) Verify file at https://github.com/sumup/sumup-mcp/blob/main/.dev.vars 2) Check git history for actual secrets
[HYP] Hardcoded Default Secrets in Docker Example
class: SECRET
asset: sumup-plugin-medusa/examples/docker/medusa/medusa-config.ts:12-13
confidence: 75
reasoning: JWT_SECRET, COOKIE_SECRET, MEDUSA_ADMIN_PASSWORD, SUPERADMIN_PASSWORD all default to "supersecret" when env vars unset. entrypoint.sh seeds admin with this password. Deploying docker-compose unchanged gives trivially guessable JWT signing key.
impact: Medium
verify_steps: 1) Check if any SumUp demo/staging uses these defaults 2) Search internal configs for MEDUSA_ADMIN_PASSWORD
[HYP] Hardcoded Postgres Credentials in Docker Example
class: SECRET
asset: sumup-plugin-vendure/examples/docker/example.env:8-9
confidence: 70
reasoning: POSTGRES_USER=vendure, POSTGRES_PASSWORD=vendure hardcoded defaults. Combined with above, docker-compose up exposes Postgres with trivial credentials.
impact: Low
verify_steps: 1) Confirm no SumUp instances use these defaults 2) Check if Postgres port is externally exposed
[HYP] Internal Maven Server URL Leaked
class: MISCONFIG
asset: sumup-android-tap-to-pay/build.gradle.kts:18
confidence: 90
reasoning: URL https://tap-to-pay-sdk.fleet.live.sumup.net/ reveals internal Maven artifact server hostname and Fleet CD platform naming convention.
impact: Low
verify_steps: 1) Confirm this is the only public reference 2) Check Fleet server for additional exposed endpoints
[HYP] Internal Mock Service DNS Name in Tests
class: MISCONFIG
asset: sumup-rs/sdk/tests/client.rs:59-60
confidence: 85
reasoning: String https://mock.sumup.internal in test code reveals internal DNS naming convention for mock services.
impact: Very Low
verify_steps: 1) Verify domain doesn't resolve externally 2) Check other repos for internal hostnames
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-05 23:43:17 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 01:18:28 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 06:02:15 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 11:06:47 UTC
[HYP] Cloudflare Account ID Exposed in Public Repository
class: MISCONFIG
asset: sumup/sumup-mcp/wrangler.jsonc
confidence: 85
reasoning: Cloudflare account ID "2037fc18a2fb8175c20d20776cac65c5" is hardcoded in wrangler.jsonc for all environments (dev, stage, live). This is a Cloudflare-specific identifier that should not be public.
impact: Medium - Allows attackers to enumerate Cloudflare resources, potentially discover other workers, or craft targeted attacks against the account.
verify_steps: 1. Visit https://github.com/sumup/sumup-mcp/blob/main/wrangler.jsonc 2. Verify account_id matches 2037fc18a2fb8175c20d20776cac65c5 3. Check Cloudflare dashboard if accessible with this ID
[HYP] Internal Staging/Development Domains Exposed
class: MISCONFIG
asset: sumup/sumup-mcp/wrangler.jsonc
confidence: 90
reasoning: Internal staging domains revealed: mcp-theta.sam-app.ro (dev), mcp.sam-app.ro (stage), api-theta.sam-app.ro, auth-theta.sam-app.ro, api.sam-app.ro, auth.sam-app.ro. These are non-public SumUp infrastructure.
impact: High - Exposes internal attack surface for reconnaissance. Attackers can probe these domains for vulnerabilities, misconfigurations, or weaker security controls.
verify_steps: 1. DNS lookup on sam-app.ro domains 2. Check if these are reachable from public internet 3. Probe for exposed services or admin panels
[HYP] CORS Wildcard on Auth-Protected MCP Endpoint
class: MISCONFIG
asset: sumup/sumup-mcp/src/config.ts
confidence: 75
reasoning: MCP server uses Access-Control-Allow-Origin: * which allows any origin to make requests to the endpoint, even though it requires Bearer token authentication.
impact: Low-Medium - While the endpoint requires JWT authentication, the wildcard CORS could facilitate CSRF-like attacks or allow malicious websites to interact with the MCP server if user's browser has valid tokens.
verify_steps: 1. Visit https://mcp.sumup.com 2. Send a cross-origin request from a different domain 3. Verify CORS headers allow all origins
[HYP] OpenAI Apps Challenge Endpoint Exposed
class: OTHER
asset: sumup/sumup-mcp/wrangler.jsonc + src/worker.ts
confidence: 60
reasoning: Dedicated route /.well-known/openai-apps-challenge with environment variable OPENAI_APPS_CHALLENGE. This appears to be a feature flag for OpenAI integration that could be enabled.
impact: Low - Potential additional attack surface if the OpenAI challenge feature is enabled or contains sensitive configuration.
verify_steps: 1. Visit https://mcp.sumup.com/.well-known/openai-apps-challenge 2. Check if endpoint returns content or 404
[HYP] Cloudflare Durable Object Session State Storage
class: MISCONFIG
asset: sumup/sumup-mcp/wrangler.jsonc
confidence: 70
reasoning: MCP server uses Cloudflare Durable Objects (SumUpMcpAgent) to persist session state across requests within the same Worker deployment.
impact: Low-Medium - If Durable Object storage is not properly secured or if there are race conditions, session data could be accessed or manipulated.
verify_steps: 1. Check if Durable Object bindings are properly scoped 2. Review sumup-agent.ts for session handling logic
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 14:13:19 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 17:06:21 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 19:20:02 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 21:23:52 UTC
[HYP] (none - no candidates)
class: OTHER
asset: N/A
confidence: 0
reasoning: Candidate list is empty ("no org candidates"). scope.yml github_orgs is "none-configured". reposcan-raw/summary.txt confirms "TARGET_ORG not configured for sumup; skipping public-org deep scan."
impact: N/A
verify_steps: N/A - no repos to clone or grep
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-06 23:06:01 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 01:03:23 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 06:12:26 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 12:39:12 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 17:57:53 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 21:02:16 UTC
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc (lines 24, 49, 74)
confidence: 95
reasoning: The wrangler.jsonc file contains the Cloudflare account_id "2037fc18a2fb8175c20d20776cac65c5" in clear text for all three environments (dev, stage, live). While CF account IDs are not full secrets on their own, they are non-public identifiers that reveal the owning account and can be used as a prerequisite for targeted Cloudflare API abuse (e.g., account-level enumeration, Workers KV/Durable Object interaction) if combined with any leaked API token.
impact: LOW-MEDIUM — Aids reconnaissance; insufficient alone for compromise.
verify_steps: 1. Confirm the account_id matches the live SumUp Cloudflare account via passive DNS or certificate transparency logs for mcp.sumup.com. 2. Check whether any other SumUp repos or config files reference this account_id.
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc, sumup-mcp/src/auth.test.ts
confidence: 95
reasoning: The wrangler.jsonc and test files reveal internal staging hostnames: mcp-theta.sam-app.ro, api-theta.sam-app.ro, auth-theta.sam-app.ro (dev), and mcp.sam-app.ro, api.sam-app.ro, auth.sam-app.ro (stage). These expose the naming convention of SumUp's internal infrastructure and reveal that development/staging environments sit behind the sam-app.ro domain. If these hosts are not access-controlled, they could be targeted for weaker security posture than production.
impact: LOW-MEDIUM — Information disclosure of internal infrastructure; potential attack surface expansion.
verify_steps: 1. Passively check DNS resolution for the exposed hostnames (e.g., via dig/nslookup). 2. Confirm they resolve to internal/private IPs or that public DNS does not exist.
class: MISCONFIG
asset: sumup-mcp/wrangler.jsonc (line: upload_source_maps: true), sumup-developer/wrangler.jsonc (line: upload_source_maps: true)
confidence: 90
reasoning: Both the MCP server and developer portal Cloudflare Worker configs have upload_source_maps set to true. When deployed, this uploads source maps to Cloudflare, making the original TypeScript source decompilable by anyone who can access the Workers runtime or debug endpoints. This exposes internal logic, comments, and potentially internal-only code paths.
impact: LOW — Source code exposure via source maps; requires additional access to Cloudflare debugging surface.
verify_steps: 1. Confirm source maps are actually served in production by inspecting the Worker response for source map references. 2. Check if Cloudflare's source map access controls are properly configured.
class: MISCONFIG
asset: sumup-plugin-vendure/examples/docker/vendure/vendure-config.ts (lines 25-38), sumup-plugin-vendure/examples/docker/example.env
confidence: 85
reasoning: The example Docker configuration hardcodes default credentials: SUPERADMIN_PASSWORD="supersecret", COOKIE_SECRET="supersecret", SESSION_SECRET="supersecret", POSTGRES_PASSWORD="vendure", POSTGRES_USER="vendure". While these are explicitly in an examples/ directory, developers frequently deploy without changing defaults. The example.env file ships identical values. The cookie secret "supersecret" is especially weak for session signing.
impact: LOW — Example-only defaults; risk only if deployed without modification by integrators.
verify_steps: 1. Confirm these are only in the examples/ directory and not in the plugin source itself. 2. Check if the README warns against using defaults in production.
class: MISCONFIG
asset: sumup-plugin-vendure/src/sumup.controller.ts (lines 9-17)
confidence: 80
reasoning: The SumUpController.webhook() method at POST /payments/sumup/webhook accepts a checkout_id from the raw request body and calls syncPaymentFromCheckout() without verifying a webhook signature or HMAC. In contrast, the ACP repo (sumup-audit/acp/signature/signature.go) implements proper HMAC-SHA256 verification. The lack of webhook verification means any party who can reach this endpoint can trigger payment state transitions by supplying arbitrary checkout IDs.
impact: MEDIUM — Potential for payment status manipulation if the Vendure webhook endpoint is network-accessible without additional middleware.
verify_steps: 1. Check if the Vendure plugin documentation requires users to add webhook signature verification middleware. 2. Confirm whether SumUp sends a signature header on webhooks that the plugin expects users to validate externally.
class: OTHER
asset: sumup-developer/astro.config.ts (line 151)
confidence: 100
reasoning: The Google Search Console verification token "0mA7KPaajXK9CtZgu7A9lLDHeTEZ_SiHdmXz2vDej7Y" is hardcoded in the Astro config. While Google site verification tokens are inherently semi-public (they are used for DNS/HTML verification), embedding them in a public source repo confirms the ownership claim and could be used for targeted phishing against the developer.sumup.com property.
impact: LOW — Semi-public by design; minor information disclosure.
verify_steps: 1. Confirm this matches the live verification on developer.sumup.com via a meta tag or DNS TXT record.
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-07 23:16:36 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 01:29:18 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 05:59:45 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 10:55:57 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 14:50:42 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 18:13:01 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 21:12:01 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-08 23:25:19 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 01:29:12 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 06:38:07 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 11:49:27 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 15:40:30 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 18:51:15 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 21:25:39 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-09 23:29:28 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 01:25:03 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 06:37:20 UTC
[HYP] <no findings>
class: N/A
asset: N/A
confidence: 0
reasoning: The scope.yml shows github_orgs: - none-configured and cands.txt states "no org candidates". No public repositories from SumUp's GitHub organization are available to audit via the GitHub API.
impact: N/A
verify_steps: N/A - no candidate repos to scan
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 11:47:17 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 15:36:25 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 18:40:20 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 21:12:12 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
## REPOSCAN 2026-09-10 23:12:34 UTC
TARGET_ORG not configured for sumup; skipping public-org deep scan.
