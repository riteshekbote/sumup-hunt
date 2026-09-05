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
