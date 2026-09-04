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
