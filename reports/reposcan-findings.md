# REPOSCAN FINDINGS — SumUp Public Repositories
**Scan date:** 2026-09-07
**Org:** github.com/sumup
**Repos scanned:** sumup-mcp, sumup-developer, sumup-plugin-medusa, sumup-plugin-vendure, sumup-postman, sumup-openapi, sumup-ai, sumup-android-tap-to-pay, sumup-rs, sumup-cli, sumup-checkout-examples

---

## [HYP] Cloudflare Account ID Hardcoded in wrangler.jsonc
- **class:** SECRET
- **asset:** sumup-mcp/wrangler.jsonc (lines 24, 49, 74)
- **confidence:** 85
- **reasoning:** Cloudflare account ID `2037fc18a2fb8175c20d20776cac65c5` is hardcoded in all three environment blocks (dev/stage/live). This 32-char hex value is the Cloudflare account identifier used in API calls and dashboard URLs. Not a full credential but enables targeted enumeration of Cloudflare resources (Workers, KV, D1, etc.).
- **impact:** Low-Medium — enables Cloudflare account enumeration; combined with other info could assist targeted attacks
- **verify_steps:** 1) Navigate to `https://dash.cloudflare.com/2037fc18a2fb8175c20d20776cac65c5` to confirm account exists. 2) Cloudflare API `GET /client/v4/accounts/2037fc18a2fb8175c20d20776cac65c5` (requires API token — passive verification only).

---

## [HYP] Internal Staging/Development Infrastructure Naming Exposed
- **class:** MISCONFIG
- **asset:** sumup-mcp/wrangler.jsonc, sumup-mcp/src/auth.test.ts, sumup-mcp/src/worker.test.ts, sumup-mcp/src/sumup-agent.test.ts, sumup-mcp/.github/workflows/deploy.yaml
- **confidence:** 90
- **reasoning:** Full internal infrastructure naming scheme exposed in public repo: `sam-app.ro` domain with subdomains `mcp-theta.sam-app.ro` (dev), `mcp.sam-app.ro` (stage), `api-theta.sam-app.ro` (dev API), `api.sam-app.ro` (stage API), `auth-theta.sam-app.ro` (dev auth), `auth.sam-app.ro` (stage auth), `mcp-beta.sam-app.ro` (beta). Reveals environment topology, naming convention, and staging API hosts. These are live, reachable domains confirmed by prior recon.
- **impact:** Medium — exposes internal attack surface; attackers can probe staging environments for weaker security controls
- **verify_steps:** 1) DNS lookup on `sam-app.ro`, `api.sam-app.ro`, `auth.sam-app.ro`, `mcp-theta.sam-app.ro` to confirm resolution. 2) HTTP HEAD/GET to identify running services and software versions. 3) Check if staging environments have weaker auth/debug modes enabled.

---

## [HYP] Wildcard CORS on Production MCP Server
- **class:** MISCONFIG
- **asset:** sumup-mcp/src/config.ts:8
- **confidence:** 80
- **reasoning:** `Access-Control-Allow-Origin: *` configured on the production MCP server (`mcp.sumup.com`) which handles OAuth-authenticated requests with merchant payment data. The wildcard CORS header allows any website to make credentialed cross-origin requests. Combined with Bearer token authentication, this could enable CSRF-style token exfiltration if a user visits a malicious site while authenticated.
- **impact:** Medium — any origin can interact with the MCP endpoint; could facilitate token theft via malicious page if browser sends credentials
- **verify_steps:** 1) `curl -I -X OPTIONS https://mcp.sumup.com/mcp -H "Origin: https://evil.com" -H "Access-Control-Request-Method: POST"` to confirm wildcard CORS active. 2) Verify credentials (cookies/Authorization headers) are actually sent cross-origin in browser context. 3) Test if MCP server respects Origin header for state-changing operations.

---

## [HYP] Wildcard CORS on Developer Portal
- **class:** MISCONFIG
- **asset:** sumup-developer/public/_headers:2
- **confidence:** 70
- **reasoning:** `Access-Control-Allow-Origin: *` set on all paths (`/*`) of the production developer portal (developer.sumup.com). While the developer portal is primarily documentation, the wildcard CORS could allow cross-origin reading of any non-public API responses if the portal serves authenticated content.
- **impact:** Low-Medium — documentation site; lower risk than MCP server but still violates CORS best practices for a production SumUp property
- **verify_steps:** 1) `curl -I https://developer.sumup.com/ -H "Origin: https://evil.com"` to confirm CORS header. 2) Check if any authenticated routes exist on the portal.

---

## [HYP] Committed Secrets File (.dev.vars) Not in .gitignore
- **class:** MISCONFIG
- **asset:** sumup-mcp/.dev.vars
- **confidence:** 70
- **reasoning:** The `.dev.vars` file (Cloudflare Workers local secrets file) is committed to the public repo. Currently contains only `OPENAI_APPS_CHALLENGE=` (empty value), but this file is specifically designed for local development secrets. It is NOT listed in `.gitignore`, meaning any developer adding real secrets to this file will automatically commit them in future PRs.
- **impact:** Low — currently empty but dangerous anti-pattern; any future developer adding secrets will leak them
- **verify_steps:** 1) Verify file at `https://github.com/sumup/sumup-mcp/blob/main/.dev.vars`. 2) Check `.gitignore` to confirm `.dev.vars` is absent. 3) Check git history for any prior secret values.

---

## [HYP] Hardcoded Default JWT/Session Secrets in Docker Example
- **class:** SECRET
- **asset:** sumup-plugin-medusa/examples/docker/medusa/medusa-config.ts:12-13, sumup-plugin-medusa/examples/docker/example.env:19-20,36
- **confidence:** 75
- **reasoning:** `JWT_SECRET`, `COOKIE_SECRET`, `MEDUSA_ADMIN_PASSWORD` all default to `"supersecret"` when environment variables are unset. The `entrypoint.sh` seeds an admin user with this password on first boot. If a developer deploys the example Docker stack without overriding env vars, the admin dashboard and JWT signing are protected by this trivially guessable value. This is a SumUp-shipped plugin example that could be deployed as-is.
- **impact:** Medium — requires deploying the Docker example unchanged; affects demo/starter setups shipped by SumUp
- **verify_steps:** 1) Check if any SumUp-managed demo/staging deployments use these defaults. 2) Search internal deployment configs for `MEDUSA_ADMIN_PASSWORD` or `SUPERADMIN_PASSWORD`. 3) Confirm the example.env is the only file providing these defaults.

---

## [HYP] Hardcoded Default Secrets in Vendure Docker Example
- **class:** SECRET
- **asset:** sumup-plugin-vendure/examples/docker/vendure/vendure-config.ts:26,29, sumup-plugin-vendure/examples/docker/example.env:8-9,12-13,16
- **confidence:** 75
- **reasoning:** `SUPERADMIN_PASSWORD`, `COOKIE_SECRET`, `SESSION_SECRET` all default to `"supersecret"`. Postgres credentials default to `vendure/vendure`. The `vendure-config.ts` uses these as fallbacks. Combined, deploying the Docker example exposes Postgres with trivial credentials and the admin panel with a guessable password.
- **impact:** Medium — same as Medusa plugin; affects SumUp-shipped example deployments
- **verify_steps:** 1) Confirm no SumUp instances use these defaults. 2) Check if Postgres port is exposed externally in any deployment.

---

## [HYP] Internal Maven Artifact Server URL Leaked
- **class:** MISCONFIG
- **asset:** sumup-android-tap-to-pay/build.gradle.kts:18
- **confidence:** 90
- **reasoning:** URL `https://tap-to-pay-sdk.fleet.live.sumup.net/` reveals an internal Maven artifact server hostname and its infrastructure naming convention (`fleet` = likely a continuous delivery platform). This endpoint serves the proprietary Tap-to-Pay SDK binary. While the server requires Maven credentials (env-var based), the hostname itself leaks internal infrastructure details.
- **impact:** Low — endpoint requires credentials; URL is not directly exploitable but aids reconnaissance of SumUp build/deploy infrastructure
- **verify_steps:** 1) Confirm this is the only public reference to this hostname. 2) Check if the Fleet server has additional exposed endpoints. 3) Verify auth requirements on the Maven repo.

---

## [HYP] Internal Mock Service DNS Name in Test Code
- **class:** MISCONFIG
- **asset:** sumup-rs/sdk/tests/client.rs:59-60
- **confidence:** 85
- **reasoning:** String `https://mock.sumup.internal.test` appears in test code, revealing an internal DNS naming convention for mock services. The `.internal.test` TLD pattern suggests an internal test infrastructure domain.
- **impact:** Very Low — purely informational; test code only, but leaks naming convention
- **verify_steps:** 1) Verify the internal domain doesn't resolve from outside. 2) Check if more internal hostnames appear in other repos.

---

## Summary

| # | Finding | Class | Confidence | Impact |
|---|---------|-------|------------|--------|
| 1 | Cloudflare Account ID in wrangler.jsonc | SECRET | 85 | Low-Medium |
| 2 | Internal Staging Infrastructure Exposed | MISCONFIG | 90 | Medium |
| 3 | Wildcard CORS on MCP Server | MISCONFIG | 80 | Medium |
| 4 | Wildcard CORS on Developer Portal | MISCONFIG | 70 | Low-Medium |
| 5 | Committed .dev.vars File | MISCONFIG | 70 | Low |
| 6 | Default Secrets in Medusa Docker Example | SECRET | 75 | Medium |
| 7 | Default Secrets in Vendure Docker Example | SECRET | 75 | Medium |
| 8 | Internal Maven Server URL Leaked | MISCONFIG | 90 | Low |
| 9 | Internal Mock DNS in Test Code | MISCONFIG | 85 | Very Low |

**Total findings:** 9 (3 SECRET, 6 MISCONFIG)
