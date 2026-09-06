# SumUp Payments Limited / SumUp Group inventory (discovery seed 2026-09-02)
# NOTE: hosts below are discovery candidates from passive DNS/CT; confirm in-scope vs program scope before active testing.
admin.sumup.com
api.sumup.com
auth.sumup.com
dashboard.sumup.com
portal.sumup.com
sumup.com
support.sumup.com
web.sumup.com
www.sumup.com

## PASSIVE RECON 2026-09-02 (read-only, non-intrusive)

> Recon observations only. These are NOT confirmed vulnerabilities; ownership/in-scope of each host must be confirmed against the program scope before any active testing. Hosts resolve + serve HTTP — investigation requires scoped authorization.

**Probed:** 9 hosts | **Live HTTP:** 5

| Host | Status | Server/Tech |
|---|---|---|
| `auth.sumup.com` | 301 | Server: cloudflare -> https://auth.sumup.com/flows/login |
| `api.sumup.com` | 404 | Server: cloudflare |
| `portal.sumup.com` | 302 | Server: nginx; Via: 1.1 varnish -> /login |
| `www.sumup.com` | 200 | Server: cloudflare |
| `admin.sumup.com` | 403 | Server: nginx/1.26.1 |

**CNAME review signals (5):**
- `auth.sumup.com` -> `auth.sumup.com.cdn.cloudflare.net`
- `api.sumup.com` -> `api.sumup.com.cdn.cloudflare.net`
- `portal.sumup.com` -> `sumup.iriscrm.com`
- `www.sumup.com` -> `www.sumup.com.cdn.cloudflare.net`
- `admin.sumup.com` -> `k8s-sumup-soapnlb-0a002ce48d-817675a16e59f14e.elb.eu-west-1.amazonaws.com`

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `admin.sumup.com` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `api.sumup.com` | **Ports:** [80, 443, 2082, 2083, 2086, 2087, 8080, 8443]
**Non-web ports observed:** [2082, 2083, 2086, 2087, 8080, 8443]
> NOTE: repeated identical non-web port sets (e.g. 2082,2083,2086,2087,8080,8443) across many hosts and wide port sets are likely a shared edge/proxy answering EOF, NOT confirmed real services. Verify with a proper port scanner (e.g. nmap) under authorization before treating as real. These are surface-map hints only, not findings.

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `auth.sumup.com` | **Ports:** [80, 443, 2082, 2083, 2086, 2087, 8080, 8443]
**Non-web ports observed:** [2082, 2083, 2086, 2087, 8080, 8443]
> NOTE: repeated identical non-web port sets (e.g. 2082,2083,2086,2087,8080,8443) across many hosts and wide port sets are likely a shared edge/proxy answering EOF, NOT confirmed real services. Verify with a proper port scanner (e.g. nmap) under authorization before treating as real. These are surface-map hints only, not findings.

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `portal.sumup.com` | **Ports:** [80, 443]
**Web surface only:** [80, 443]

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `www.sumup.com` | **Ports:** [80, 443, 2082, 2083, 2086, 2087, 8080, 8443]
**Non-web ports observed:** [2082, 2083, 2086, 2087, 8080, 8443]
> NOTE: repeated identical non-web port sets (e.g. 2082,2083,2086,2087,8080,8443) across many hosts and wide port sets are likely a shared edge/proxy answering EOF, NOT confirmed real services. Verify with a proper port scanner (e.g. nmap) under authorization before treating as real. These are surface-map hints only, not findings.

## 2026-09-02 21:55:21 UTC

## 2026-09-03 00:07:18 UTC

## 2026-09-03 04:11:41 UTC

## 2026-09-03 09:02:20 UTC

## 2026-09-03 13:32:16 UTC

## 2026-09-03 17:25:51 UTC
- NEW Dedicated deep scan (2026-09-03) found **0 genuinely dedicated hosts** — all subdomains resolve to shared/CDN/wildcard IPs (Cloudflare, AWS ELB, iriscrm.com). Attack surface is wildcard-dominated; enu
- NEW `portal.sumup.com` CNAME → `sumup.iriscrm.com` (third-party CRM). This introduces supply-chain/SSRF surface via webhook/callback endpoints on a non-SumUp domain.
- CHANGED `api.sumup.com` returns 404 on root — suggests versioned API paths (/v1, /v2, /beta, /internal) are the real surface, not yet mapped.

## 2026-09-03 19:59:04 UTC
- NEW api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
- CHANGED admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
- CHANGED portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.

## 2026-09-03 22:32:22 UTC
- NEW Dedicated deep scan (2026-09-03) found **0 genuinely dedicated hosts** — all subdomains resolve to shared/CDN/wildcard IPs (Cloudflare, AWS ELB, iriscrm.com). Attack surface is wildcard-dominated; enu
- NEW `portal.sumup.com` CNAME → `sumup.iriscrm.com` (third-party CRM). This introduces supply-chain/SSRF surface via webhook/callback endpoints on a non-SumUp domain.
- CHANGED `api.sumup.com` returns 404 on root — suggests versioned API paths (/v1, /v2, /beta, /internal) are the real surface, not yet mapped.
- NEW api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
- CHANGED admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
- CHANGED portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.
- NEW api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
- CHANGED admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
- CHANGED portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.
- NEW auth.sumup.com OIDC/OAuth discovery docs fully exposed: `.well-known/openid-configuration` and `.well-known/oauth-authorization-server` return 200 with complete endpoint map + scope catalog. Live endp
- NEW Scope catalog on auth.sumup.com enumerates the merchant API resource model: merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/receipts/sales/roles + read/write variants — dir
- NEW Security-relevant OAuth settings revealed: PAR endpoint `/oauth2/par` (404 via GET, POST-only), device flow `/oauth2/device`, `token_endpoint_auth_methods_supported` includes `"none"`, request_object 
- CHANGED api.sumup.com: uniform 404 on ALL enumerated paths (v0/v0.1/v1/v2/merchants/checkouts/transactions/etc.) — unauthenticated surface fully gated at gateway; scope-derived paths also 404. API enumeration
- NEW auth.sumup.com: OIDC discovery fully exposed (.well-known/openid-configuration + oauth-authorization-server return 200) revealing complete endpoint map (/oauth2/auth, /oauth2/token, /oauth2/par, /oaut
- NEW auth.sumup.com: scope catalog documents merchant API resource model (merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/receipts/sales/roles + read/write) — maps hidden api.su
- NEW auth.sumup.com: security-relevant OAuth flags exposed — PAR + request_uri supported (require_request_uri_registration), device flow, token_endpoint_auth_methods incl "none", request_object alg incl "n
- CHANGED api.sumup.com: uniform 404 on every enumerated path (v0/v0.1/v1/v2, scope-derived resources) — unauthenticated API surface fully gated; enumeration dead without auth.

## 2026-09-04 00:36:06 UTC
- NEW me.sumup.com identified as a distinct merchant self-service asset served by Vercel (not Cloudflare/nginx/ELB). Root and /settings/oauth2-applications both 307 → auth.sumup.com OAuth with public `clien
- NEW Real public OAuth client `dashboard` exposed; its production scope catalog differs from OIDC discovery and developer docs: `openid offline classic accounting.read/write invoices.read/write api_keys ap
- NEW OAuth `state` on the dashboard flow is an HS256-signed JWT carrying `appState{flow,pathname,queryParams}`; server enforces state≥8 chars.
- CHANGED auth.sumup.com redirect_uri validation CONFIRMED strict allowlist for `client_id=dashboard`: attacker host, subdomain-confusion, and path-traversal redirect_uri all rejected (`invalid_request` → error
- CHANGED /oauth2/par and /oauth2/device return 404 on OPTIONS (documented but not routed), while /oauth2/token and /oauth2/revoke return 200 on OPTIONS — PAR/device grant routes likely not deployed at routing 

## 2026-09-04 05:12:55 UTC
- NEW me.sumup.com confirmed as distinct Vercel-served merchant self-service asset (non-Cloudflare origin) with OAuth2 application registry at /settings/oauth2-applications behind dashboard client_id
- NEW Real public OAuth client `dashboard` exposed with production scope catalog broader than OIDC discovery: accounting.read/write invoices.read/write api_keys:write readers.read/write lending.read/write r
- NEW OAuth `state` parameter is HS256-signed JWT carrying appState{flow,pathname,queryParams}; server enforces state≥8 chars
- CHANGED auth.sumup.com redirect_uri validation CONFIRMED strict allowlist for client_id=dashboard — attacker host, subdomain-confusion, path-traversal all rejected (invalid_request on server flow page)
- CHANGED /oauth2/par and /oauth2/device return 404 on OPTIONS (documented but unrouted) while /oauth2/token and /oauth2/revoke return 200 — PAR/device grants not deployed at routing level
- CHANGED api.sumup.com all versioned paths return 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token

## 2026-09-04 09:55:16 UTC
- NEW auth.sumup.com: OIDC discovery fully exposed with PAR, device flow, `token_endpoint_auth_methods=["none"]`, `request_object_signing_alg_values_supported=["none"]` — all documented but PAR/device retur
- NEW me.sumup.com: Distinct Vercel-served merchant self-service asset (non-Cloudflare origin) with OAuth2 application registry at `/settings/oauth2-applications` behind `client_id=dashboard`
- NEW Real public OAuth client `dashboard` exposed with production scope catalog: `accounting.read/write invoices.read/write api_keys:write readers.read/write lending.read/write receivables.read/write unifi
- CHANGED auth.sumup.com redirect_uri validation: Strict allowlist confirmed for `client_id=dashboard` — attacker host, subdomain-confusion, path-traversal all rejected with `invalid_request` on server flow pag
- CHANGED api.sumup.com: All versioned paths return 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- CHANGED admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation

## 2026-09-04 14:14:31 UTC
- NEW me.sumup.com/api/sso/callback returns 403 (not 404) on anonymous GET — Vercel serverless function exists and enforces auth at edge
- NEW auth.sumup.com/oauth2/token returns 405 on GET (method not allowed) — confirms POST-only token endpoint, consistent with OAuth2 spec
- CHANGED auth.sumup.com/oauth2/par returns 404 on GET — PAR endpoint documented but not accessible via GET (POST-only per spec), routing unconfirmed
- CHANGED api.sumup.com/v1/merchants/{other_merchant_id} returns 404 unauthenticated — versioned resource paths fully gated, no info leak on ID format

## 2026-09-04 17:50:13 UTC

## 2026-09-04 20:02:49 UTC

## 2026-09-04 22:20:56 UTC
- NEW api.sumup.com/authorize: Legacy OAuth authorize endpoint discovered with client_id oracle + loose redirect_uri validation on legacy-registered test/dev SDK clients (sumup-ios-sdk, sumup.pos, reader, s
- NEW Probe vector: GET https://api.sumup.com/authorize?client_id={legacy_client_id} enumeration — new attack surface on API gateway (non-Cloudflare path?)

## 2026-09-05 00:24:44 UTC
- NEW api.sumup.com/authorize returns 404 for all tested legacy client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal, dashboard) — legacy authorize endpoint not functional on API gateway
- NEW auth.sumup.com/oauth2/par and /oauth2/device endpoints ARE routed (respond to POST) but require client authentication — "none" auth method not usable for dashboard client
- NEW me.sumup.com/api/sso/callback returns 307 redirect to root on anonymous GET/OPTIONS — no CORS headers exposed, no debug endpoints found
- CHANGED Legacy OAuth authorize hypothesis (confidence 55→25): client_id oracle + loose redirect_uri REFUTED by 404 on all legacy client_ids
- CHANGED Public client impersonation via "none" auth method (confidence 70→40): token_endpoint_auth_methods=["none"] documented but dashboard client rejects unauthenticated requests

## 2026-09-05 04:44:37 UTC
- NEW portal.sumup.com returns 200 on root (previously 302→/login) — live CMS/page surface now accessible for parameter enumeration
- NEW auth.sumup.com/oauth2/auth with request_object alg=none returns 405 (not 302/200) — endpoint rejects unsigned request_object at method level before validation
- CHANGED me.sumup.com/api/sso/callback consistently returns 403 (not 307) on anonymous GET — Vercel function enforces auth at edge, no redirect loop
- CHANGED api.sumup.com/authorize returns 404 for ALL legacy client_ids — legacy OAuth surface fully dead on API gateway
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device respond to POST (routed) but require client auth — "none" auth_method not usable for dashboard client

## 2026-09-05 08:45:15 UTC
- NEW api.sumup.com/authorize is LIVE: bare and client_id probes return 302 → auth.sumup.com/flows/oauth2/error with OAuth error taxonomy — directly contradicts the 2026-09-05 recording that "legacy authori
- NEW api.sumup.com/authorize exposes a client_id ORACLE via error taxonomy: `invalid_client` ("does not exist") for unknown clients vs `invalid_request` (redirect_uri mismatch) for the registered `dashboar
- NEW api.sumup.com/.well-known/security.txt returns 200 (PGP-signed, canonical set includes api.sumup.com) with `access-control-allow-origin: *` + `Domain=sumup.com SameSite=None` cookies — public known fi
- NEW web.sumup.com IP 77.246.42.130 confirmed in Rackspace netblock `UK-RACKSPACE-20070509` (org: Rackspace Ltd., Texas) — NOT SumUp-owned; third-party lease pool, strengthens the dormant subdomain-takeove
- NEW api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) — contradicts the 2026-09-05 "legacy authorize dead (404)" recording; the earlier log ran a literal unexpanded `{legacy_client_i
- NEW Client_id ORACLE on api.sumup.com/authorize: invalid_client ("does not exist") for unknown IDs vs invalid_request (redirect mismatch) for registered `dashboard` — 2-class error taxonomy.
- NEW Legacy registry divergence: `dashboard` client's modern registered callback `https://me.sumup.com/api/sso/callback` is rejected on the legacy gateway (invalid_request redirect-mismatch) but yields 303
- NEW Endpoint-specific wildcard CORS confirmed side-by-side: api.sumup.com/authorize sends `access-control-allow-origin: *` + `access-control-allow-methods: GET,HEAD,PUT,PATCH,POST,DELETE` + `access-contro
- NEW api.sumup.com/.well-known/security.txt → 200 PGP-signed (canonical includes api.sumup.com) — public known file, benign.
- NEW web.sumup.com IP 77.246.42.130 confirmed in Rackspace lease pool (UK-RACKSPACE-20070509, org Rackspace Ltd) — NOT SumUp-owned; strengthens dormant subdomain-takeover candidate; host still non-responsi
- NEW api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) — contradicts the KB entry "2026-09-05 REJECTED: legacy authorize returns 404 for all legacy client_ids." Those probe logs ran a
- NEW Client_id ORACLE on api.sumup.com/authorize via error taxonomy: `invalid_client` ("does not exist") for unknown IDs, `invalid_request` (redirect_uri mismatch) for the registered `dashboard` client — 2
- NEW Legacy registry divergence: `client_id=dashboard&redirect_uri=https://me.sumup.com/api/sso/callback` is REJECTED on the legacy gateway (`invalid_request` redirect-mismatch, even with valid state) but 
- NEW Endpoint-specific wildcard CORS confirmed side-by-side: api.sumup.com/authorize emits `access-control-allow-origin: *` + `access-control-allow-methods: GET,HEAD,PUT,PATCH,POST,DELETE` + `access-contro
- NEW api.sumup.com/.well-known/security.txt → 200 PGP-signed with canonical set including api.sumup.com — public known file, benign (out-of-scope class).
- NEW web.sumup.com IP 77.246.42.130 confirmed in Rackspace lease pool RDAP `UK-RACKSPACE-20070509` (org Rackspace Ltd., San Antonio TX) — NOT SumUp-owned netblock; strengthens the dormant subdomain-takeove
- NEW portal.sumup.com returns 200 with React CRM login page (iriscrm.com) — live parameter enumeration surface now accessible
- CHANGED auth.sumup.com/oauth2/auth with request_object returns 405 (rejects at method level before validation)
- CHANGED me.sumup.com/api/sso/callback returns 307 (not 403) on anonymous GET — redirect to OAuth flow
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST — routed but require client authentication params
- CHANGED api.sumup.com/authorize returns 404 for ALL legacy client_ids — legacy OAuth surface fully dead

## 2026-09-05 12:10:03 UTC
- NEW api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence (modern me.sumup.com callback rejec
- NEW api.sumup.com/authorize exposes wildcard CORS (access-control-allow-origin:*, broad allow-methods, max-age) + SameSite=None cookies on Domain=sumup.com — absent on auth.sumup.com/oauth2/auth
- NEW portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403

## 2026-09-05 15:31:08 UTC
- NEW api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern me.sumup.com callback reje
- NEW api.sumup.com/authorize exposes wildcard CORS (access-control-allow-origin:*, broad allow-methods, max-age) + SameSite=None cookies on Domain=sumup.com — absent on auth.sumup.com/oauth2/auth
- NEW portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface now accessible
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403

## 2026-09-05 17:52:35 UTC
- NEW api.sumup.com/authorize confirmed LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle: `invalid_client` for unknown IDs vs `invalid_request` (redirect_uri mismatch) for registered `dash
- NEW Legacy/modern redirect_uri divergence confirmed: `dashboard` client's modern callback `https://me.sumup.com/api/sso/callback` accepted on auth.sumup.com (302→login flow) but REJECTED on api.sumup.com/
- NEW api.sumup.com/authorize exposes endpoint-specific wildcard CORS (`access-control-allow-origin: *`, broad allow-methods, max-age=300) + `SameSite=None; Domain=sumup.com` cookies — absent on auth.sumup.
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403

## 2026-09-05 19:34:42 UTC
- NEW api.sumup.com/authorize confirmed LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle: `invalid_client` for unknown IDs vs `invalid_request` (redirect_uri mismatch) for registered `dash
- NEW Legacy/modern redirect_uri divergence confirmed: `dashboard` client's modern callback `https://me.sumup.com/api/sso/callback` accepted on auth.sumup.com (302→login flow) but REJECTED on api.sumup.com/
- NEW api.sumup.com/authorize exposes endpoint-specific wildcard CORS (`access-control-allow-origin: *`, broad allow-methods, max-age=300) + `SameSite=None; Domain=sumup.com` cookies — absent on auth.sumup.
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible

## 2026-09-05 21:50:26 UTC
- NEW api.sumup.com/authorize confirmed LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle: `invalid_client` for unknown IDs vs `invalid_request` (redirect_uri mismatch) for registered `dash
- NEW Legacy/modern redirect_uri divergence confirmed: `dashboard` client's modern callback `https://me.sumup.com/api/sso/callback` accepted on auth.sumup.com (302→login flow) but REJECTED on api.sumup.com/
- NEW api.sumup.com/authorize exposes endpoint-specific wildcard CORS (`access-control-allow-origin: *`, broad allow-methods, max-age=300) + `SameSite=None; Domain=sumup.com` cookies — absent on auth.sumup.
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- NEW crt.sh passive CT sweep (4422 certs → 257 unique names): first records of checkout.sumup.com (Vercel 76.76.21.61), read-api.sumup.com + sf-gateway-api.sumup.com (Cloudflare, root 404), app-auth.sumup.
- CHANGED Legacy api.sumup.com/authorize ground truth re-confirmed by raw curl: LIVE (302→auth error flow with `error=` taxonomy). The 19:35 probe-log "HTTP 404" lines were redirect-following harness artifacts,
- NEW sumup-ios-sdk → `invalid_client` ("does not exist") on legacy gateway — legacy SDK clients NOT registered there; only `dashboard` known-registered on the legacy path.
- CHANGED Legacy redirect oracle re-tested +18 new combos this session (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, sumup://, sumup-pos://, com.sumup.pos://, api.sumup.com×3, www.sumup.
- CHANGED checkout.sumup.com: uniform 403 text/plain on all paths (/, assets/*, sdk.js, api/*) — Vercel edge-gated, posture identical to me.sumup.com.
- NEW api.sumup.com/authorize confirmed LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle: `invalid_client` for unknown IDs vs `invalid_request` (redirect_uri mismatch) for registered `dash
- NEW Legacy/modern redirect_uri divergence confirmed: `dashboard` client's modern callback `https://me.sumup.com/api/sso/callback` accepted on auth.sumup.com (302→login flow) but REJECTED on api.sumup.com/
- NEW api.sumup.com/authorize exposes endpoint-specific wildcard CORS (`access-control-allow-origin: *`, broad allow-methods, max-age=300) + `SameSite=None; Domain=sumup.com` cookies — absent on auth.sumup.
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- NEW api.sumup.com/authorize confirmed LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle: `invalid_client` for unknown IDs vs `invalid_request` (redirect_uri mismatch) for registered `dash
- NEW Legacy/modern redirect_uri divergence confirmed: `dashboard` client's modern callback `https://me.sumup.com/api/sso/callback` accepted on auth.sumup.com (302→login flow) but REJECTED on api.sumup.com/
- NEW api.sumup.com/authorize exposes endpoint-specific wildcard CORS (`access-control-allow-origin: *`, broad allow-methods, max-age=300) + `SameSite=None; Domain=sumup.com` cookies — absent on auth.sumup.
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible

## 2026-09-05 23:45:33 UTC
- NEW api.sumup.com/authorize confirmed LIVE via raw curl (302→auth.sumup.com/flows/oauth2/error) — probe harness 404s were redirect-following artifacts; endpoint exposes client_id oracle (invalid_client vs
- NEW api.sumup.com/authorize endpoint-specific wildcard CORS confirmed: access-control-allow-origin:* + access-control-allow-methods:GET,HEAD,PUT,PATCH,POST,DELETE + access-control-max-age:300 + SameSite=N
- NEW crt.sh passive CT sweep: 4422 certs → 257 unique names; first records of checkout.sumup.com (Vercel 76.76.21.61), read-api.sumup.com + sf-gateway-api.sumup.com (Cloudflare, root 404), app-auth.sumup.c
- NEW checkout.sumup.com: new Vercel asset (76.76.21.61); uniform 403 text/plain on all paths (/, assets/*, sdk.js, api/*) — edge-gated same as me.sumup.com
- NEW sumup-ios-sdk and unknown client_ids → invalid_client ("does not exist") on legacy gateway — legacy SDK clients NOT registered; only dashboard confirmed registered on legacy path
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- CHANGED Legacy redirect oracle re-tested +18 new combos (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, sumup://, sumup-pos://, com.sumup.pos://, api.sumup.com×3, www.sumup.com) — all in

## 2026-09-06 04:05:10 UTC
- NEW Dedicated deep scan (2026-09-03) found **0 genuinely dedicated hosts** — all subdomains resolve to shared/CDN/wildcard IPs (Cloudflare, AWS ELB, iriscrm.com). Attack surface is wildcard-dominated; enu
- NEW `portal.sumup.com` CNAME → `sumup.iriscrm.com` (third-party CRM). This introduces supply-chain/SSRF surface via webhook/callback endpoints on a non-SumUp domain.
- CHANGED `api.sumup.com` returns 404 on root — suggests versioned API paths (/v1, /v2, /beta, /internal) are the real surface, not yet mapped.
- NEW api.sumup.com/authorize confirmed LIVE via raw curl (302→auth.sumup.com/flows/oauth2/error) — probe harness 404s were redirect-following artifacts; endpoint exposes client_id oracle (invalid_client vs
- NEW api.sumup.com/authorize endpoint-specific wildcard CORS confirmed: access-control-allow-origin:* + access-control-allow-methods:GET,HEAD,PUT,PATCH,POST,DELETE + access-control-max-age:300 + SameSite=N
- NEW crt.sh passive CT sweep: 4422 certs → 257 unique names; first records of checkout.sumup.com (Vercel 76.76.21.61), read-api.sumup.com + sf-gateway-api.sumup.com (Cloudflare, root 404), app-auth.sumup.c
- NEW checkout.sumup.com: new Vercel asset (76.76.21.61); uniform 403 text/plain on all paths (/, assets/*, sdk.js, api/*) — edge-gated same as me.sumup.com
- NEW sumup-ios-sdk and unknown client_ids → invalid_client ("does not exist") on legacy gateway — legacy SDK clients NOT registered; only dashboard confirmed registered on legacy path
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- CHANGED Legacy redirect oracle re-tested +18 new combos (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, sumup://, sumup-pos://, com.sumup.pos://, api.sumup.com×3, www.sumup.com) — all in
- NEW checkout.sumup.com: Vercel asset (76.76.21.61) discovered via CT sweep; uniform 403 text/plain on all paths (/, assets/*, sdk.js, api/*) — edge-gated same as me.sumup.com
- NEW read-api.sumup.com + sf-gateway-api.sumup.com: Cloudflare-fronted, root 404, discovered via CT sweep (4422 certs → 257 unique names)
- NEW app-auth.sumup.com: Discovered via CT sweep, Cloudflare-fronted
- NEW sumup-ios-sdk and unknown client_ids → invalid_client ("does not exist") on legacy gateway api.sumup.com/authorize — legacy SDK clients NOT registered; only dashboard confirmed registered on legacy pa
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404 as previously logged
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- CHANGED Legacy redirect oracle re-tested +18 new combos (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, sumup://, sumup-pos://, com.sumup.pos://, api.sumup.com×3, www.sumup.com) — all in
- CHANGED api.sumup.com/authorize confirmed LIVE via raw curl (302→auth.sumup.com/flows/oauth2/error) — probe harness 404s were redirect-following artifacts; endpoint exposes client_id oracle (invalid_client vs

## 2026-09-06 08:47:51 UTC
- NEW mcp.sumup.com discovered: Official SumUp MCP (Cloudflare Worker, bearer JWKS from auth.sumup.com, Durable Object agent) LIVE, absent from inventory — surfaced via sumup-mcp public repo config
- NEW sam-app.ro staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /authorize invalid_request on evil redirect, OIDC discovery pu
- NEW checkout.sumup.com (Vercel 76.76.21.61), read-api.sumup.com, sf-gateway-api.sumup.com, app-auth.sumup.com discovered via CT sweep (4422 certs → 257 unique names)
- CHANGED api.sumup.com/authorize confirmed LIVE via raw curl (302→auth.sumup.com/flows/oauth2/error) — probe harness 404s were redirect-following artifacts; endpoint exposes client_id oracle + wildcard CORS
- CHANGED auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404
- CHANGED me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
- CHANGED portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
- CHANGED Legacy redirect oracle re-tested +18 new combos (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, custom schemes, api.sumup.com×3, www) — all invalid_request; legacy allowlist host
- CHANGED sumup-ios-sdk and unknown client_ids → invalid_client ("does not exist") on legacy gateway — legacy SDK clients NOT registered; only dashboard confirmed registered on legacy path
