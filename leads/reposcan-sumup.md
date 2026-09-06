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
