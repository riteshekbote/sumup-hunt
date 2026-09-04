## 2026-09-03 17:25:41 UTC [target] (model nemotron3)
[NEW] Dedicated deep scan (2026-09-03) found **0 genuinely dedicated hosts** — all subdomains resolve to shared/CDN/wildcard IPs (Cloudflare, AWS ELB, iriscrm.com). Attack surface is wildcard-dominated; enumeration must focus on path/parameter-level on known hosts.
[NEW] `portal.sumup.com` CNAME → `sumup.iriscrm.com` (third-party CRM). This introduces supply-chain/SSRF surface via webhook/callback endpoints on a non-SumUp domain.
[CHANGED] `api.sumup.com` returns 404 on root — suggests versioned API paths (/v1, /v2, /beta, /internal) are the real surface, not yet mapped.
[PRIO] api.sumup.com,8.0,a=9,b=10,t=8,g=5,c=8,f=5  
[PRIO] auth.sumup.com,7.4,a=8,b=9,t=7,g=6,c=7,f=5  
[PRIO] portal.sumup.com,6.7,a=7,b=8,t=6,g=6,c=6,f=5  
[PRIO] admin.sumup.com,5.8,a=6,b=7,t=5,g=3,c=8,f=5  
[PRIO] www.sumup.com,5.0,a=4,b=4,t=4,g=8,c=7,f=5  
[HYP] API versioned endpoint IDOR/BOLA on merchant resources  
class: IDOR  
asset: api.sumup.com  
confidence: 65  
reasoning: Root returns 404; payment APIs typically version at /v1 /v2 /beta /internal. Merchant-scoped resources (transactions, payouts, devices, readers) often use predictable IDs (merchant_id, transaction_id, reader_serial). Cloudflare WAF may not block parameter tampering on authenticated calls.  
evidence_needed: Discovery of versioned API paths + authenticated requests showing cross-merchant access via ID manipulation  
verify_steps:  
testability: PASSIVE  
impact: Cross-merchant transaction/payout/device data access — HIGH (PCI/financial data)
[HYP] OAuth redirect_uri validation bypass on auth.sumup.com flows  
class: OAUTH  
asset: auth.sumup.com  
confidence: 55  
reasoning: auth.sumup.com redirects to /flows/login — indicates OAuth/OIDC authorization server. Common flaws: loose redirect_uri regex (subdomain bypass, path traversal), missing state validation, wildcard allowlist. Cloudflare fronting may mask origin validation logic.  
evidence_needed: Authorization endpoint accepting attacker-controlled redirect_uri with code leakage  
verify_steps:  
testability: PASSIVE  
impact: Authorization code theft → account takeover for merchants — CRITICAL
[HYP] SSRF via webhook/callback parameter on portal.sumup.com (iriscrm.com)  
class: SSRF  
asset: portal.sumup.com  
confidence: 45  
reasoning: CNAME to sumup.iriscrm.com (third-party CRM). Merchant portals often accept webhook_url/callback_url/return_url parameters for integrations. If validation is weak, SSRF to 169.254.169.254 (AWS metadata) or internal services is possible. Varnish caching layer may forward X-Forwarded-For/Host headers unsafely.  
evidence_needed: Parameter accepting URL that triggers outbound request to attacker-controlled or metadata endpoint  
verify_steps:  
testability: PASSIVE  
impact: Cloud metadata credential theft → lateral movement in SumUp AWS — CRITICAL
[PARKED] SSRF via webhook/callback parameter on portal.sumup.com: confidence 45 < threshold; third-party domain (iriscrm.com) ownership unconfirmed in-scope; verify_steps require form interaction (borderline active).  
[FINAL] 1. API versioned endpoint IDOR/BOLA on merchant resources (confidence 65)  
[FINAL] 2. OAuth redirect_uri validation bypass on auth.sumup.com flows (confidence 55)
[NEXT] PROBE: GET https://api.sumup.com/v1/ (HEAD) → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json, /api-docs.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.  
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.  
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[RISK] sumup: 75 — Core payment API (api.sumup.com) and auth server (auth.sumup.com) are high-value targets with confirmed live HTTP, Cloudflare/AWS edge, and standard OAuth/API patterns that historically yield IDOR/OAuth flaws. Wildcard-dominated DNS limits subdomain enum but concentrates attack surface on known hosts. Third-party portal (iriscrm.com) adds supply-chain risk if in-scope.
## 2026-09-03 19:58:39 UTC [target] (model nemotron3)
[PRIO] api.sumup.com,8.0,a=9,b=10,t=8,g=5,c=8,f=5
[PRIO] auth.sumup.com,7.4,a=8,b=9,t=7,g=6,c=7,f=5
[PRIO] portal.sumup.com,6.7,a=7,b=8,t=6,g=6,c=6,f=5
[PRIO] admin.sumup.com,5.8,a=6,b=7,t=5,g=3,c=8,f=5
[PRIO] www.sumup.com,5.0,a=4,b=4,t=4,g=8,c=7,f=5
[HYP] API versioned endpoint IDOR/BOLA on merchant resources
class: IDOR
asset: api.sumup.com
confidence: 65
reasoning: Root returns 404; payment APIs typically version at /v1 /v2 /beta /internal. Merchant-scoped resources (transactions, payouts, devices, readers) often use predictable IDs (merchant_id, transaction_id, reader_serial). Cloudflare WAF may not block parameter tampering on authenticated calls.
evidence_needed: Discovery of versioned API paths + authenticated requests showing cross-merchant access via ID manipulation
verify_steps: HEAD https://api.sumup.com/v1/ → observe status, Server, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json. Then GET discovered versioned endpoints to map resource paths.
impact: Cross-merchant transaction/payout/device data access — HIGH (PCI/financial data)
testability: PASSIVE
[HYP] OAuth redirect_uri validation bypass on auth.sumup.com flows
class: OAUTH
asset: auth.sumup.com
confidence: 55
reasoning: auth.sumup.com redirects to /flows/login — indicates OAuth/OIDC authorization server. Common flaws: loose redirect_uri regex (subdomain bypass, path traversal), missing state validation, wildcard allowlist. Cloudflare fronting may mask origin validation logic.
evidence_needed: Authorization endpoint accepting attacker-controlled redirect_uri with code leakage
verify_steps: HEAD https://auth.sumup.com/flows/login → capture redirect chain, Location header, Set-Cookie. Then GET with test redirect_uri parameters (subdomain, path traversal, wildcard) to observe validation behavior passively.
impact: Authorization code theft → account takeover for merchants — CRITICAL
testability: PASSIVE
[HYP] Admin panel authentication bypass or info leak on admin.sumup.com
class: AUTH
asset: admin.sumup.com
confidence: 45
reasoning: Returns 403 on nginx/1.26.1 behind AWS ELB — consistent with auth-gated internal tooling. 403 vs 302/401 may indicate IP allowlist, client cert, or header-based auth that could be misconfigured. ELB + nginx stack may leak internal headers or support X-Forwarded-For spoofing.
evidence_needed: Response headers showing auth mechanism (WWW-Authenticate, Set-Cookie, X-Frame-Options), or status change with modified Host/X-Forwarded-For headers
verify_steps: HEAD https://admin.sumup.com/ → capture Server, WWW-Authenticate, Set-Cookie, X-Frame-Options. Repeat with Host: localhost, X-Forwarded-For: 127.0.0.1, X-Original-URL: / to probe auth bypass vectors passively.
impact: Internal admin panel access → merchant data manipulation, payout tampering, device management — CRITICAL
testability: PASSIVE
[PARKED] SSRF via webhook/callback parameter on portal.sumup.com: confidence 45 < threshold for FINAL; third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; verify_steps require form interaction (borderline active).
[FINAL] 1. API versioned endpoint IDOR/BOLA on merchant resources (confidence 65)
[FINAL] 2. OAuth redirect_uri validation bypass on auth.sumup.com flows (confidence 55)
[FINAL] 3. Admin panel authentication bypass or info leak on admin.sumup.com (confidence 45)
[NEXT] PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json, /api-docs.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passively.
[RISK] sumup: 75 — Core payment API (api.sumup.com) and auth server (auth.sumup.com) are high-value targets with confirmed live HTTP, Cloudflare/AWS edge, and standard OAuth/API patterns that historically yield IDOR/OAuth flaws. Wildcard-dominated DNS limits subdomain enum but concentrates attack surface on known hosts. Third-party portal (iriscrm.com) adds supply-chain risk if in-scope. Admin panel (admin.sumup.com) exposes internal tooling surface behind ELB/nginx.
## 2026-09-03 22:32:00 UTC [target] (model nemotron3)
[PRIO] api.sumup.com,8.0,a=9,b=10,t=8,g=5,c=8,f=5
[PRIO] auth.sumup.com,7.4,a=8,b=9,t=7,g=6,c=7,f=5
[PRIO] portal.sumup.com,6.7,a=7,b=8,t=6,g=6,c=6,f=5
[PRIO] admin.sumup.com,5.8,a=6,b=7,t=5,g=3,c=8,f=5
[PRIO] www.sumup.com,5.0,a=4,b=4,t=4,g=8,c=7,f=5
[HYP] API versioned endpoint IDOR/BOLA on merchant resources
class: IDOR
asset: api.sumup.com
confidence: 65
reasoning: Root returns 404; payment APIs typically version at /v1 /v2 /beta /internal. Merchant-scoped resources (transactions, payouts, devices, readers) often use predictable IDs (merchant_id, transaction_id, reader_serial). Cloudflare WAF may not block parameter tampering on authenticated calls.
evidence_needed: Discovery of versioned API paths + authenticated requests showing cross-merchant access via ID manipulation
verify_steps: HEAD https://api.sumup.com/v1/ → observe status, Server, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json. Then GET discovered versioned endpoints to map resource paths.
impact: Cross-merchant transaction/payout/device data access — HIGH (PCI/financial data)
testability: PASSIVE
[HYP] OAuth redirect_uri validation bypass on auth.sumup.com flows
class: OAUTH
asset: auth.sumup.com
confidence: 55
reasoning: auth.sumup.com redirects to /flows/login — indicates OAuth/OIDC authorization server. Common flaws: loose redirect_uri regex (subdomain bypass, path traversal), missing state validation, wildcard allowlist. Cloudflare fronting may mask origin validation logic.
evidence_needed: Authorization endpoint accepting attacker-controlled redirect_uri with code leakage
verify_steps: HEAD https://auth.sumup.com/flows/login → capture redirect chain, Location header, Set-Cookie. Then GET with test redirect_uri parameters (subdomain, path traversal, wildcard) to observe validation behavior passively.
impact: Authorization code theft → account takeover for merchants — CRITICAL
testability: PASSIVE
[HYP] Admin panel authentication bypass or info leak on admin.sumup.com
class: AUTH
asset: admin.sumup.com
confidence: 45
reasoning: Returns 403 on nginx/1.26.1 behind AWS ELB — consistent with auth-gated internal tooling. 403 vs 302/401 may indicate IP allowlist, client cert, or header-based auth that could be misconfigured. ELB + nginx stack may leak internal headers or support X-Forwarded-For spoofing.
evidence_needed: Response headers showing auth mechanism (WWW-Authenticate, Set-Cookie, X-Frame-Options), or status change with modified Host/X-Forwarded-For headers
verify_steps: HEAD https://admin.sumup.com/ → capture Server, WWW-Authenticate, Set-Cookie, X-Frame-Options. Repeat with Host: localhost, X-Forwarded-For: 127.0.0.1, X-Original-URL: / to probe auth bypass vectors passively.
impact: Internal admin panel access → merchant data manipulation, payout tampering, device management — CRITICAL
testability: PASSIVE
[PARKED] SSRF via webhook/callback parameter on portal.sumup.com: confidence 45 < threshold; third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; verify_steps require form interaction (borderline active).
[FINAL] 1. API versioned endpoint IDOR/BOLA on merchant resources (confidence 65)
[FINAL] 2. OAuth redirect_uri validation bypass on auth.sumup.com flows (confidence 55)
[FINAL] 3. Admin panel authentication bypass or info leak on admin.sumup.com (confidence 45)
[NEXT] PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json, /api-docs.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passively.
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[RISK] sumup: 75 — Core payment API (api.sumup.com) and auth server (auth.sumup.com) are high-value targets with confirmed live HTTP, Cloudflare/AWS edge, and standard OAuth/API patterns that historically yield IDOR/OAuth flaws. Wildcard-dominated DNS limits subdomain enum but concentrates attack surface on known hosts. Third-party portal (iriscrm.com) adds supply-chain risk if in-scope. Admin panel (admin.sumup.com) exposes internal tooling surface behind ELB/nginx.
## 2026-09-04 00:31:41 UTC [target] (model nemotron3)
[PRIO] auth.sumup.com,8.2,a=9,b=10,t=9,g=7,c=8,f=6
[PRIO] api.sumup.com,7.1,a=8,b=10,t=6,g=3,c=7,f=5
[PRIO] admin.sumup.com,5.5,a=6,b=8,t=5,g=3,c=7,f=5
[PRIO] portal.sumup.com,6.0,a=7,b=7,t=6,g=5,c=6,f=5
[HYP] OAuth token_endpoint_auth_methods=none enables public client impersonation
class: AUTH
asset: auth.sumup.com
confidence: 70
reasoning: Both discovery docs list "none" in token_endpoint_auth_methods_supported — allows token endpoint calls without client authentication. Combined with PAR (pushed_authorization_request_endpoint) and request_object_signing_alg_values_supported including "none", a malicious client could register or reuse a public client_id and obtain tokens via PAR + token endpoint without client_secret.
evidence_needed: Successful token request to /oauth2/token with client_id only (no client_secret) using authorization_code from PAR flow
verify_steps: POST https://auth.sumup.com/oauth2/par with minimal request (client_id, redirect_uri, response_type=code, code_challenge) → observe if request_uri returned. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code, code=<from PAR>, client_id=<same>, no client_secret.
impact: Client impersonation → merchant account takeover via OAuth flow — CRITICAL
testability: AUTH_HELPED
[HYP] OAuth redirect_uri validation bypass via loose regex on auth.sumup.com
class: OAUTH
asset: auth.sumup.com
confidence: 60
reasoning: Discovery docs show redirect_uri registration required (require_request_uri_registration=true) but validation logic untested. Cloudflare fronts the auth server; common bypasses: subdomain takeover (evil.sumup.com), path traversal (/../), wildcard match (https://app.sumup.com.attacker.com), open redirect on registered domain. Scope catalog shows high-value merchant scopes.
evidence_needed: Authorization request to /oauth2/auth with attacker-controlled redirect_uri returning authorization code to attacker domain
verify_steps: GET https://auth.sumup.com/oauth2/auth?client_id=test&redirect_uri=https://evil.com&response_type=code&scope=merchants.read → observe redirect behavior. Test variants: https://sumup.com.evil.com, https://app.sumup.com/../evil.com, https://sub.sumup.com (if subdomain takeover possible).
impact: Authorization code theft → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] API versioned endpoint IDOR/BOLA on merchant resources via authenticated calls
class: IDOR
asset: api.sumup.com
confidence: 55
reasoning: Auth discovery reveals complete merchant resource model (merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/receipts/sales/roles + read/write). All unauthenticated paths 404. BOLA requires merchant OAuth token. x-envoy-decorator-operation leaks internal k8s routing (apigateway2-headless.identity.svc.cluster.local) — may indicate internal API structure.
evidence_needed: Authenticated cross-merchant access via ID manipulation (merchant_id, transaction_id, reader_serial) on versioned endpoints
verify_steps: Obtain valid merchant OAuth token (AUTH_HELPED). GET https://api.sumup.com/v1/merchants/{other_merchant_id}/transactions → observe 403 vs 200. Repeat for /v1/transactions/{txn_id}, /v1/readers/{serial}, /v1/payouts/{id}.
impact: Cross-merchant financial data access (transactions, payouts, readers) — HIGH (PCI/financial)
testability: AUTH_HELPED
[PARKED] Admin panel authentication bypass on admin.sumup.com: confidence 35 < 40; all header spoofing attempts return identical 403 with nginx/1.26.1 headers — no auth mechanism leak, no status change.
[PARKED] SSRF via portal.sumup.com webhook: confidence 40; third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; requires form interaction (active).
[PARKED] k8s service name leak via x-envoy-decorator-operation on api.sumup.com: REJECTED class (MISCONFIG — header/banner leak explicit out-of-scope per program note).
[FINAL] 1. OAuth token_endpoint_auth_methods=none enables public client impersonation (confidence 70)
[FINAL] 2. OAuth redirect_uri validation bypass on auth.sumup.com (confidence 60)
[FINAL] 3. API versioned endpoint IDOR/BOLA on merchant resources (confidence 55)
[NEXT] PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` → capture request_uri response; then POST https://auth.sumup.com/oauth2/token with `grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier` (no client_secret) to test "none" auth method.
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
[LEARN] REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token.
[LEARN] REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
[RISK] sumup: 80 — Auth server (auth.sumup.com) exposes critical OAuth misconfigurations (token_endpoint_auth_methods=none, request_object alg=none, PAR, device flow) with full scope catalog mapping to payment API resources. API gateway (api.sumup.com) fully gated but resource model known. Admin panel hardened. Third-party portal adds supply-chain risk. High-value financial attack surface concentrated on known hosts.
## 2026-09-04 05:12:47 UTC [target] (model nemotron3)
[NEW] me.sumup.com confirmed as distinct Vercel-served merchant self-service asset (non-Cloudflare origin) with OAuth2 application registry at /settings/oauth2-applications behind dashboard client_id
[NEW] Real public OAuth client `dashboard` exposed with production scope catalog broader than OIDC discovery: accounting.read/write invoices.read/write api_keys:write readers.read/write lending.read/write receivables.read/write unified_customer_directory:read/write
[NEW] OAuth `state` parameter is HS256-signed JWT carrying appState{flow,pathname,queryParams}; server enforces state≥8 chars
[CHANGED] auth.sumup.com redirect_uri validation CONFIRMED strict allowlist for client_id=dashboard — attacker host, subdomain-confusion, path-traversal all rejected (invalid_request on server flow page)
[CHANGED] /oauth2/par and /oauth2/device return 404 on OPTIONS (documented but unrouted) while /oauth2/token and /oauth2/revoke return 200 — PAR/device grants not deployed at routing level
[CHANGED] api.sumup.com all versioned paths return 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
[PRIO] auth.sumup.com,8.4,a=9,b=10,t=9,g=4,c=8,f=8
[PRIO] me.sumup.com,7.8,a=7,b=9,t=8,g=6,c=8,f=8
[PRIO] api.sumup.com,7.2,a=8,b=10,t=7,g=3,c=8,f=5
[PRIO] admin.sumup.com,5.9,a=6,b=8,t=5,g=3,c=7,f=5
[PRIO] portal.sumup.com,5.5,a=6,b=6,t=5,g=5,c=6,f=5
[HYP] OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow
class: AUTH
asset: auth.sumup.com
confidence: 70
reasoning: Discovery docs list "none" in token_endpoint_auth_methods_supported; PAR endpoint documented (/oauth2/par); request_object_signing_alg_values_supported includes "none". A public client (no secret) could obtain tokens via PAR + token endpoint without client_secret.
evidence_needed: Successful token request to /oauth2/token with client_id only (no client_secret) using authorization_code from PAR flow
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 → capture request_uri. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier (no client_secret).
impact: Client impersonation → merchant account takeover via OAuth flow — CRITICAL
testability: AUTH_HELPED
[HYP] me.sumup.com Vercel origin exposes serverless API routes or permissive CORS on SSO callback
class: BUSLOGIC
asset: me.sumup.com
confidence: 55
reasoning: me.sumup.com is a fresh Vercel-served asset (non-Cloudflare/Envoy edge) with merchant self-service OAuth2 app registry. Vercel serverless functions at /api/* may expose debug endpoints, permissive CORS, or logic flaws in the SSO callback (/api/sso/callback) that handles the dashboard OAuth flow.
evidence_needed: Anonymous GET/OPTIONS to /api/* routes returning non-401; CORS headers allowing credentialed cross-origin; error taxonomy differences revealing stack traces
verify_steps: GET https://me.sumup.com/api/sso/callback (anonymous) → observe status, error body, CORS headers. OPTIONS https://me.sumup.com/api/sso/callback → capture Access-Control-Allow-Origin, Access-Control-Allow-Credentials. GET https://me.sumup.com/_vercel/insights and /api/* enumeration for non-401 responses.
impact: Merchant self-service misconfig → OAuth app registration abuse, broad-scope token issuance, or data exposure — HIGH
testability: PASSIVE
[HYP] API scope-mapped BOLA on merchant resources via dashboard-client scopes
class: IDOR
asset: api.sumup.com
confidence: 60
reasoning: Dashboard client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps 1:1 to hidden api.sumup.com resources. All unauthenticated paths 404. Cross-tenant BOLA requires valid merchant OAuth token (from dashboard scopes) then ID manipulation on resource identifiers (merchant_id, transaction_id, reader_serial, payout_id).
evidence_needed: Authenticated cross-merchant access via ID manipulation on versioned endpoints returning 200 vs 403
verify_steps: Obtain valid merchant OAuth token with dashboard scopes (AUTH_HELPED). GET https://api.sumup.com/v1/merchants/{other_merchant_id}/transactions → observe 403 vs 200. Repeat for /v1/transactions/{txn_id}, /v1/readers/{serial}, /v1/payouts/{id}, /v1/invoices/{id}.
impact: Cross-merchant financial/PII data exposure (transactions, payouts, readers, invoices) — CRITICAL (PCI/financial)
testability: AUTH_HELPED
[PARKED] OAuth redirect_uri validation bypass on auth.sumup.com: REFUTED for naive vectors — strict allowlist confirmed for client_id=dashboard (attacker-host/subdomain/path-traversal all rejected with invalid_request on server page). No viable passive redirect_uri bypass; confidence dropped below threshold.
[PARKED] OAuth PAR/device flow token theft: PAR and device endpoints return 404 on OPTIONS (unrouted at gateway); redirect_uri strictly allowlisted; requires registered attacker client + key — not passive, low current yield.
[PARKED] SSRF via portal.sumup.com webhook: third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; requires form interaction (active).
[PARKED] k8s service name leak via x-envoy-decorator-operation on api.sumup.com: REJECTED class (MISCONFIG — header/banner leak explicit out-of-scope per program note).
[PARKED] Admin panel auth bypass on admin.sumup.com: header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth mechanism leak, no status change; confidence 35 < 40.
[FINAL] 1. OAuth token_endpoint_auth_methods=none enables public client impersonation (confidence 70)
[FINAL] 2. API scope-mapped BOLA on merchant resources (confidence 60)
[FINAL] 3. me.sumup.com Vercel surface / OAuth2-app-registration logic (confidence 55)
[NEXT] PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` → capture request_uri response; then POST https://auth.sumup.com/oauth2/token with `grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier` (no client_secret) to test "none" auth method.
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
[LEARN] ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal bypass refuted; errors surfaced on server flow page.
[LEARN] ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudflare attack surface.
[LEARN] ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 200 — PAR/device grants likely not deployed at routing level.
[LEARN] ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broader hidden api.sumup.com resource model than OIDC discovery.
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token.
[LEARN] REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
[LEARN] REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
[RISK] sumup: 78 — Auth server (auth.sumup.com) exposes critical OAuth misconfigurations (token_endpoint_auth_methods=none, request_object alg=none, PAR documented) with full scope catalog mapping to payment API resources. API gateway (api.sumup.com) fully gated but resource model known via dashboard scopes. New Vercel-served me.sumup.com adds fresh non-Cloudflare attack surface with OAuth2 app registration UX. Admin panel hardened. Third-party portal adds supply-chain risk if in-scope confirmed. High-value financial attack surface concentrated on known hosts.
## 2026-09-04 09:49:01 UTC [target] (model nemotron3)
[NEW] auth.sumup.com: OIDC discovery fully exposed with PAR, device flow, `token_endpoint_auth_methods=["none"]`, `request_object_signing_alg_values_supported=["none"]` — all documented but PAR/device return 404 on OPTIONS (unrouted)
[NEW] me.sumup.com: Distinct Vercel-served merchant self-service asset (non-Cloudflare origin) with OAuth2 application registry at `/settings/oauth2-applications` behind `client_id=dashboard`
[NEW] Real public OAuth client `dashboard` exposed with production scope catalog: `accounting.read/write invoices.read/write api_keys:write readers.read/write lending.read/write receivables.read/write unified_customer_directory:read/write` — broader than OIDC discovery
[CHANGED] auth.sumup.com redirect_uri validation: Strict allowlist confirmed for `client_id=dashboard` — attacker host, subdomain-confusion, path-traversal all rejected with `invalid_request` on server flow page
[CHANGED] api.sumup.com: All versioned paths return 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
[CHANGED] admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation
[PRIO] auth.sumup.com,8.4,a=9,b=10,t=9,g=4,c=8,f=8
[PRIO] me.sumup.com,7.8,a=7,b=9,t=8,g=6,c=8,f=8
[PRIO] api.sumup.com,7.2,a=8,b=10,t=7,g=3,c=8,f=5
[PRIO] admin.sumup.com,5.9,a=6,b=8,t=5,g=3,c=7,f=5
[PRIO] portal.sumup.com,5.5,a=6,b=6,t=5,g=5,c=6,f=5
[HYP] OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow
class: AUTH
asset: auth.sumup.com
confidence: 70
reasoning: Discovery docs list "none" in token_endpoint_auth_methods_supported; PAR endpoint documented (/oauth2/par); request_object_signing_alg_values_supported includes "none". A public client (no secret) could obtain tokens via PAR + token endpoint without client_secret.
evidence_needed: Successful token request to /oauth2/token with client_id only (no client_secret) using authorization_code from PAR flow
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 → capture request_uri. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier (no client_secret).
impact: Client impersonation → merchant account takeover via OAuth flow — CRITICAL
testability: AUTH_HELPED
[HYP] API scope-mapped BOLA on merchant resources via dashboard-client scopes
class: IDOR
asset: api.sumup.com
confidence: 60
reasoning: Dashboard client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps 1:1 to hidden api.sumup.com resources. All unauthenticated paths 404. Cross-tenant BOLA requires valid merchant OAuth token (from dashboard scopes) then ID manipulation on resource identifiers (merchant_id, transaction_id, reader_serial, payout_id).
evidence_needed: Authenticated cross-merchant access via ID manipulation on versioned endpoints returning 200 vs 403
verify_steps: Obtain valid merchant OAuth token with dashboard scopes (AUTH_HELPED). GET https://api.sumup.com/v1/merchants/{other_merchant_id}/transactions → observe 403 vs 200. Repeat for /v1/transactions/{txn_id}, /v1/readers/{serial}, /v1/payouts/{id}, /v1/invoices/{id}.
impact: Cross-merchant financial/PII data exposure (transactions, payouts, readers, invoices) — CRITICAL (PCI/financial)
testability: AUTH_HELPED
[HYP] me.sumup.com Vercel origin exposes serverless API routes or permissive CORS on SSO callback
class: BUSLOGIC
asset: me.sumup.com
confidence: 55
reasoning: me.sumup.com is a fresh Vercel-served asset (non-Cloudflare/Envoy edge) with merchant self-service OAuth2 app registry. Vercel serverless functions at /api/* may expose debug endpoints, permissive CORS, or logic flaws in the SSO callback (/api/sso/callback) that handles the dashboard OAuth flow.
evidence_needed: Anonymous GET/OPTIONS to /api/* routes returning non-401; CORS headers allowing credentialed cross-origin; error taxonomy differences revealing stack traces
verify_steps: GET https://me.sumup.com/api/sso/callback (anonymous) → observe status, error body, CORS headers. OPTIONS https://me.sumup.com/api/sso/callback → capture Access-Control-Allow-Origin, Access-Control-Allow-Credentials. GET https://me.sumup.com/_vercel/insights and /api/* enumeration for non-401 responses.
impact: Merchant self-service misconfig → OAuth app registration abuse, broad-scope token issuance, or data exposure — HIGH
testability: PASSIVE
[PARKED] OAuth redirect_uri validation bypass on auth.sumup.com: REFUTED for naive vectors — strict allowlist confirmed for client_id=dashboard (attacker-host/subdomain/path-traversal all rejected with invalid_request on server page). No viable passive redirect_uri bypass; confidence dropped below threshold.
[PARKED] OAuth PAR/device flow token theft: PAR and device endpoints return 404 on OPTIONS (unrouted at gateway); redirect_uri strictly allowlisted; requires registered attacker client + key — not passive, low current yield.
[PARKED] SSRF via portal.sumup.com webhook: third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; requires form interaction (active).
[PARKED] k8s service name leak via x-envoy-decorator-operation on api.sumup.com: REJECTED class (MISCONFIG — header/banner leak explicit out-of-scope per program note).
[PARKED] Admin panel auth bypass on admin.sumup.com: header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth mechanism leak, no status change; confidence 35 < 40.
[FINAL] 1. OAuth token_endpoint_auth_methods=none enables public client impersonation (confidence 70)
[FINAL] 2. API scope-mapped BOLA on merchant resources (confidence 60)
[FINAL] 3. me.sumup.com Vercel surface / OAuth2-app-registration logic (confidence 55)
[NEXT] PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` → capture request_uri response; then POST https://auth.sumup.com/oauth2/token with `grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier` (no client_secret) to test "none" auth method.
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
[LEARN] ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal bypass refuted; errors surfaced on server flow page.
[LEARN] ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudflare attack surface.
[LEARN] ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 200 — PAR/device grants likely not deployed at routing level.
[LEARN] ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broader hidden api.sumup.com resource model than OIDC discovery.
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token.
[LEARN] REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
[LEARN] REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
[RISK] sumup: 78 — Auth server (auth.sumup.com) exposes critical OAuth misconfigurations (token_endpoint_auth_methods=none, request_object alg=none, PAR documented) with full scope catalog mapping to payment API resources. API gateway (api.sumup.com) fully gated but resource model known via dashboard scopes. New Vercel-served me.sumup.com adds fresh non-Cloudflare attack surface with OAuth2 app registration UX. Admin panel hardened. Third-party portal adds supply-chain risk if in-scope confirmed. High-value financial attack surface concentrated on known hosts.
## 2026-09-04 14:14:22 UTC [target] (model nemotron3)
[NEW] me.sumup.com/api/sso/callback returns 403 (not 404) on anonymous GET — Vercel serverless function exists and enforces auth at edge
[NEW] auth.sumup.com/oauth2/token returns 405 on GET (method not allowed) — confirms POST-only token endpoint, consistent with OAuth2 spec
[CHANGED] auth.sumup.com/oauth2/par returns 404 on GET — PAR endpoint documented but not accessible via GET (POST-only per spec), routing unconfirmed
[CHANGED] api.sumup.com/v1/merchants/{other_merchant_id} returns 404 unauthenticated — versioned resource paths fully gated, no info leak on ID format
[PRIO] auth.sumup.com,8.5,a=9,b=10,t=9,g=4,c=9,f=8
[PRIO] me.sumup.com,7.9,a=7,b=9,t=8,g=6,c=8,f=8
[PRIO] api.sumup.com,7.3,a=8,b=10,t=7,g=3,c=8,f=5
[PRIO] admin.sumup.com,5.9,a=6,b=8,t=5,g=3,c=7,f=5
[PRIO] portal.sumup.com,5.5,a=6,b=6,t=5,g=5,c=6,f=5
[HYP] OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow
class: AUTH
asset: auth.sumup.com
confidence: 70
reasoning: Discovery docs list "none" in token_endpoint_auth_methods_supported; PAR endpoint documented (/oauth2/par); request_object_signing_alg_values_supported includes "none". A public client (no secret) could obtain tokens via PAR + token endpoint without client_secret.
evidence_needed: Successful token request to /oauth2/token with client_id only (no client_secret) using authorization_code from PAR flow
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 → capture request_uri. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier (no client_secret).
impact: Client impersonation → merchant account takeover via OAuth flow — CRITICAL
testability: AUTH_HELPED
[HYP] API scope-mapped BOLA on merchant resources via dashboard-client scopes
class: IDOR
asset: api.sumup.com
confidence: 60
reasoning: Dashboard client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps 1:1 to hidden api.sumup.com resources. All unauthenticated paths 404. Cross-tenant BOLA requires valid merchant OAuth token (from dashboard scopes) then ID manipulation on resource identifiers (merchant_id, transaction_id, reader_serial, payout_id).
evidence_needed: Authenticated cross-merchant access via ID manipulation on versioned endpoints returning 200 vs 403
verify_steps: Obtain valid merchant OAuth token with dashboard scopes (AUTH_HELPED). GET https://api.sumup.com/v1/merchants/{other_merchant_id}/transactions → observe 403 vs 200. Repeat for /v1/transactions/{txn_id}, /v1/readers/{serial}, /v1/payouts/{id}, /v1/invoices/{id}.
impact: Cross-merchant financial/PII data exposure (transactions, payouts, readers, invoices) — CRITICAL (PCI/financial)
testability: AUTH_HELPED
[HYP] me.sumup.com Vercel origin exposes serverless API routes or permissive CORS on SSO callback
class: BUSLOGIC
asset: me.sumup.com
confidence: 55
reasoning: me.sumup.com is a fresh Vercel-served asset (non-Cloudflare/Envoy edge) with merchant self-service OAuth2 app registry. Vercel serverless functions at /api/* may expose debug endpoints, permissive CORS, or logic flaws in the SSO callback (/api/sso/callback) that handles the dashboard OAuth flow.
evidence_needed: Anonymous GET/OPTIONS to /api/* routes returning non-401; CORS headers allowing credentialed cross-origin; error taxonomy differences revealing stack traces
verify_steps: GET https://me.sumup.com/api/sso/callback (anonymous) → observe status, error body, CORS headers. OPTIONS https://me.sumup.com/api/sso/callback → capture Access-Control-Allow-Origin, Access-Control-Allow-Credentials. GET https://me.sumup.com/_vercel/insights and /api/* enumeration for non-401 responses.
impact: Merchant self-service misconfig → OAuth app registration abuse, broad-scope token issuance, or data exposure — HIGH
testability: PASSIVE
[PARKED] OAuth PAR/device flow token theft: PAR and device endpoints return 404 on OPTIONS (unrouted at gateway); redirect_uri strictly allowlisted; requires registered attacker client + key — not passive, low current yield.
[PARKED] SSRF via portal.sumup.com webhook: third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; requires form interaction (active).
[PARKED] k8s service name leak via x-envoy-decorator-operation on api.sumup.com: REJECTED class (MISCONFIG — header/banner leak explicit out-of-scope per program note).
[PARKED] Admin panel auth bypass on admin.sumup.com: header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth mechanism leak, no status change; confidence 35 < 40.
[FINAL] 1. OAuth token_endpoint_auth_methods=none enables public client impersonation (confidence 70)
[FINAL] 2. API scope-mapped BOLA on merchant resources (confidence 60)
[FINAL] 3. me.sumup.com Vercel surface / OAuth2-app-registration logic (confidence 55)
[NEXT] PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` → capture request_uri response; then POST https://auth.sumup.com/oauth2/token with `grant_type=authorization_code&code=<from_par>&client_id=test&code_verifier=test_verifier` (no client_secret) to test "none" auth method.
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
[LEARN] ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal bypass refuted; errors surfaced on server flow page.
[LEARN] ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudflare attack surface.
[LEARN] ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 200 — PAR/device grants likely not deployed at routing level.
[LEARN] ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broader hidden api.sumup.com resource model than OIDC discovery.
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token.
[LEARN] REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
[LEARN] REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
[RISK] sumup: 78 — Auth server (auth.sumup.com) exposes critical OAuth misconfigurations (token_endpoint_auth_methods=none, request_object alg=none, PAR documented) with full scope catalog mapping to payment API resources. API gateway (api.sumup.com) fully gated but resource model known via dashboard scopes. New Vercel-served me.sumup.com adds fresh non-Cloudflare attack surface with OAuth2 app registration UX. Admin panel hardened. Third-party portal adds supply-chain risk if in-scope confirmed. High-value financial attack surface concentrated on known hosts.
