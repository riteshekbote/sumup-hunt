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
## 2026-09-04 17:50:05 UTC [target] (model nemotron3)
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
## 2026-09-04 19:57:43 UTC [target] (model nemotron3)
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
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 → capture request_uri. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code&code=<from_par>&client_id=dashboard&code_verifier=test_verifier (no client_secret).
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
[NEXT] PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` → capture request_uri response; then POST https://auth.sumup.com/oauth2/token with `grant_type=authorization_code&code=<from_par>&client_id=dashboard&code_verifier=test_verifier` (no client_secret) to test "none" auth method.
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
## 2026-09-04 22:17:48 UTC [target] (model nemotron3)
[NEW] api.sumup.com/authorize: Legacy OAuth authorize endpoint discovered with client_id oracle + loose redirect_uri validation on legacy-registered test/dev SDK clients (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal) — not present in 19:57 leads
[NEW] Probe vector: GET https://api.sumup.com/authorize?client_id={legacy_client_id} enumeration — new attack surface on API gateway (non-Cloudflare path?)
[PRIO] auth.sumup.com,8.5,a=9,b=10,t=9,g=4,c=9,f=8
[PRIO] api.sumup.com/authorize,7.8,a=8,b=9,t=8,g=7,c=7,f=7
[PRIO] me.sumup.com,7.9,a=7,b=9,t=8,g=6,c=8,f=8
[PRIO] api.sumup.com,7.3,a=8,b=10,t=7,g=3,c=8,f=5
[PRIO] admin.sumup.com,5.9,a=6,b=8,t=5,g=3,c=7,f=5
[PRIO] portal.sumup.com,5.5,a=6,b=6,t=5,g=5,c=6,f=5
[HYP] Legacy OAuth authorize endpoint client_id oracle + loose redirect_uri on api.sumup.com/authorize
class: OATH
asset: api.sumup.com/authorize
confidence: 55
reasoning: Ranked hypotheses 20:02:49 reveals legacy /authorize endpoint on api.sumup.com (distinct from auth.sumup.com) accepting legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal) with potentially relaxed redirect_uri validation vs strict allowlist on auth.sumup.com for dashboard client
evidence_needed: Response differentiation (200/302 vs 400/404) for valid vs invalid legacy client_ids; redirect_uri acceptance for attacker-controlled domains on valid legacy clients
verify_steps: GET https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=https://attacker.com/callback&response_type=code&scope=classic → observe status, Location header, error codes. Repeat for client_id=sumup.pos,reader,sales,virtual-terminal. Test redirect_uri variations (subdomain, path-traversal, localhost).
impact: OAuth code theft via loose redirect_uri on legacy clients → merchant account takeover via authorization_code flow — CRITICAL
testability: PASSIVE
[HYP] OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow
class: AUTH
asset: auth.sumup.com
confidence: 70
reasoning: Discovery docs list "none" in token_endpoint_auth_methods_supported; PAR endpoint documented (/oauth2/par); request_object_signing_alg_values_supported includes "none". A public client (no secret) could obtain tokens via PAR + token endpoint without client_secret.
evidence_needed: Successful token request to /oauth2/token with client_id only (no client_secret) using authorization_code from PAR flow
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 → capture request_uri. Then POST https://auth.sumup.com/oauth2/token with grant_type=authorization_code&code=<from_par>&client_id=dashboard&code_verifier=test_verifier (no client_secret).
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
[PARKED] API scope-mapped BOLA on merchant resources via dashboard-client scopes: confidence 60 but testability AUTH_HELPED (requires merchant token) — no passive verify steps
[PARKED] OAuth PAR/device flow token theft: PAR/device endpoints unrouted (404 OPTIONS), redirect_uri strictly allowlisted, requires registered attacker client — not passive, low yield
[PARKED] SSRF via portal.sumup.com webhook: third-party CNAME (iriscrm.com) ownership unconfirmed in-scope; requires form interaction (active)
[PARKED] k8s service name leak via x-envoy-decorator-operation on api.sumup.com: REJECTED class (MISCONFIG — header/banner leak explicit out-of-scope)
[PARKED] Admin panel auth bypass on admin.sumup.com: header spoofing yields identical 403 — confidence 35 < 40
[FINAL] 1. OAuth token_endpoint_auth_methods=none enables public client impersonation (confidence 70)
[FINAL] 2. Legacy OAuth authorize endpoint client_id oracle + loose redirect_uri on api.sumup.com/authorize (confidence 55)
[FINAL] 3. me.sumup.com Vercel surface / OAuth2-app-registration logic (confidence 55)
[NEXT] PROBE: GET https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=https://example.com/callback&response_type=code&scope=classic → observe status, Location header, error body; repeat for client_id=sumup.pos,reader,sales,virtual-terminal
[LEARN] ACCEPTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint exists on API gateway (distinct from auth.sumup.com) with legacy SDK client_ids and potentially relaxed redirect_uri validation — new attack surface
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources
[LEARN] ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal bypass refuted
[LEARN] ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudflare attack surface
[LEARN] ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 200 — PAR/device grants likely not deployed at routing level
[LEARN] ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog maps broader hidden api.sumup.com resource model than OIDC discovery
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog defines resource model but requires merchant token
[LEARN] REJECTED AUTH @ admin.sumup.com: Header spoofing yields identical 403 — no auth bypass via passive header manipulation
[LEARN] REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks k8s service name — header/banner leak explicit out-of-scope class
[RISK] sumup: 80 — Auth server (auth.sumup.com) exposes critical OAuth misconfigurations (token_endpoint_auth_methods=none, request_object alg=none, PAR documented) with full scope catalog mapping to payment API resources. NEW: Legacy authorize endpoint on api.sumup.com/authorize with client_id oracle and potentially loose redirect_uri on legacy SDK clients adds distinct OAuth attack surface on API gateway. API gateway fully gated but resource model known. Vercel-served me.sumup.com adds fresh non-Cloudflare surface. Admin panel hardened. Third-party portal supply-chain risk if in-scope confirmed. Financial attack surface elevated.
## 2026-09-05 00:24:36 UTC [target] (model nemotron3)
[NEW] api.sumup.com/authorize returns 404 for all tested legacy client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal, dashboard) — legacy authorize endpoint not functional on API gateway
[NEW] auth.sumup.com/oauth2/par and /oauth2/device endpoints ARE routed (respond to POST) but require client authentication — "none" auth method not usable for dashboard client
[NEW] me.sumup.com/api/sso/callback returns 307 redirect to root on anonymous GET/OPTIONS — no CORS headers exposed, no debug endpoints found
[CHANGED] Legacy OAuth authorize hypothesis (confidence 55→25): client_id oracle + loose redirect_uri REFUTED by 404 on all legacy client_ids
[CHANGED] Public client impersonation via "none" auth method (confidence 70→40): token_endpoint_auth_methods=["none"] documented but dashboard client rejects unauthenticated requests
[PRIO] auth.sumup.com,7.8,a=8,b=10,t=9,g=4,c=8,f=7
[PRIO] me.sumup.com,7.2,a=7,b=9,t=7,g=5,c=7,f=7
[PRIO] portal.sumup.com,6.1,a=6,b=7,t=6,g=5,c=6,f=6
[PRIO] api.sumup.com,5.8,a=7,b=10,t=5,g=3,c=7,f=5
[PRIO] admin.sumup.com,5.2,a=5,b=8,t=4,g=3,c=6,f=5
[HYP] OAuth request_object alg=none enables algorithm confusion on auth.sumup.com
class: OATH
asset: auth.sumup.com
confidence: 65
reasoning: Discovery docs show request_object_signing_alg_values_supported=["RS256","none"] and request_parameter_supported=true. A crafted request_object with alg=none could bypass signature verification if JWT library doesn't enforce alg allowlist.
evidence_needed: Successful authorization request with request=eyJhbGciOiJub25lIn0.eyJyZXNwb25zZV90eXBlIjoiY29kZSIsImNsaWVudF9pZCI6ImRhc2hib2FyZCIsInJlZGlyZWN0X3VyaSI6Imh0dHBzOi8vZGFzaGJvYXJkLnN1bXVwLmNvbS9jYWxsYmFjayIsInNjb3BlIjoiY2xhc3NpYyJ9. → observe if server accepts unsigned request_object
verify_steps: GET https://auth.sumup.com/oauth2/auth?client_id=dashboard&request=eyJhbGciOiJub25lIn0.eyJyZXNwb25zZV90eXBlIjoiY29kZSIsImNsaWVudF9pZCI6ImRhc2hib2FyZCIsInJlZGlyZWN0X3VyaSI6Imh0dHBzOi8vZGFzaGJvYXJkLnN1bXVwLmNvbS9jYWxsYmFjayIsInNjb3BlIjoiY2xhc3NpYyJ9.&response_type=code&redirect_uri=https://dashboard.sumup.com/callback → check for 302 to redirect_uri vs error
impact: OAuth code theft via forged request_object → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] me.sumup.com Vercel serverless functions expose debug endpoints or permissive CORS on /api/* routes
class: MISCONFIG
asset: me.sumup.com
confidence: 50
reasoning: me.sumup.com is Vercel-served (non-Cloudflare/Envoy) with OAuth2 app registry at /settings/oauth2-applications. Vercel /api/* routes may expose debug endpoints, stack traces, or permissive CORS on SSO callback handler.
evidence_needed: Anonymous GET/OPTIONS to /api/* routes returning non-307/401; CORS headers allowing credentialed cross-origin; error bodies revealing stack traces
verify_steps: GET https://me.sumup.com/api/sso/callback?error=test → observe status, error body, CORS headers. OPTIONS https://me.sumup.com/api/sso/callback → capture Access-Control-Allow-Origin, Access-Control-Allow-Credentials. GET https://me.sumup.com/_vercel/insights and enumerate /api/* for non-307 responses.
impact: Merchant self-service misconfig → OAuth app registration abuse, broad-scope token issuance, or data exposure — HIGH
testability: PASSIVE
[HYP] portal.sumup.com (iriscrm.com) webhook/callback SSRF via third-party CRM supply chain
class: SSRF
asset: portal.sumup.com
confidence: 45
reasoning: portal.sumup.com CNAME → sumup.iriscrm.com (third-party CRM). CSP allows *.iriscrm.com for connect-src, frame-src, img-src. Webhook/callback endpoints on iriscrm.com could be induced to fetch internal SumUp metadata (169.254.169.254) if parameterized.
evidence_needed: Discovery of webhook/callback parameter on portal.sumup.com or iriscrm.com endpoints accepting URL parameters; SSRF payload execution confirmed
verify_steps: GET https://portal.sumup.com/ → enumerate forms/endpoints with URL/redirect/webhook parameters. Passive crawl for ?url=, ?callback=, ?webhook=, ?redirect= parameters. If found, test with http://169.254.169.254/latest/meta-data/
impact: Cloud metadata exposure → IAM credentials, instance identity → full AWS account compromise — CRITICAL
testability: PASSIVE (enum) → HUMAN_ONLY (exploit requires in-scope confirmation)
[PARKED] OAuth request_object alg=none enables algorithm confusion on auth.sumup.com: Confidence 65 but requires valid client_id + registered redirect_uri (dashboard) — cannot complete flow passively without merchant auth; "none" alg may be rejected at validation layer
[PARKED] me.sumup.com Vercel serverless functions expose debug endpoints: Confidence 50 but all /api/* routes return 307 to auth — no anonymous surface found; requires authenticated session to reach app logic
[PARKED] portal.sumup.com SSRF via iriscrm.com: Confidence 45 but third-party domain (iriscrm.com) — requires explicit in-scope confirmation; webhook parameters not discovered passively
[FINAL] No hypotheses survive filtering — all require AUTH_HELPED or HUMAN_ONLY verification beyond passive capabilities
[NEXT] RAG: Fetch SumUp bug bounty program scope.yml to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
[LEARN] REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal) — no client_id oracle, no loose redirect_uri
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likely restricted to specific client configs not publicly registrable
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authentication
[LEARN] ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector documented but untestable without valid client flow
[LEARN] ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS found passively
[LEARN] REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfirmed in-scope
[RISK] sumup: 72 — Auth server (auth.sumup.com) exposes critical OAuth configuration (alg=none, PAR, device flow, "none" client auth) with full scope catalog mapping to payment APIs. However, all high-impact vectors require merchant OAuth token (AUTH_HELPED) or registered client — no passive exploit path. API gateway fully gated. Vercel me.sumup.com adds non-Cloudflare surface but auth-gated. Portal introduces third-party supply-chain risk (iriscrm.com) if in-scope confirmed. Admin panel hardened. Financial attack surface concentrated but access-controlled.
## 2026-09-05 04:41:39 UTC [target] (model nemotron3)
[NEW] portal.sumup.com returns 200 on root (previously 302→/login) — live CMS/page surface now accessible for parameter enumeration
[NEW] auth.sumup.com/oauth2/auth with request_object alg=none returns 405 (not 302/200) — endpoint rejects unsigned request_object at method level before validation
[CHANGED] me.sumup.com/api/sso/callback consistently returns 403 (not 307) on anonymous GET — Vercel function enforces auth at edge, no redirect loop
[CHANGED] api.sumup.com/authorize returns 404 for ALL legacy client_ids — legacy OAuth surface fully dead on API gateway
[CHANGED] auth.sumup.com/oauth2/par and /oauth2/device respond to POST (routed) but require client auth — "none" auth_method not usable for dashboard client
[PRIO] auth.sumup.com,8.1,a=9,b=10,t=9,g=3,c=8,f=8
[PRIO] portal.sumup.com,6.8,a=7,b=7,t=6,g=5,c=6,f=7
[PRIO] me.sumup.com,6.5,a=7,b=8,t=6,g=5,c=7,f=7
[PRIO] api.sumup.com,5.5,a=7,b=10,t=4,g=2,c=7,f=5
[PRIO] admin.sumup.com,4.8,a=5,b=7,t=3,g=2,c=6,f=5
[HYP] OAuth PAR endpoint accepts unauthenticated request_uri registration bypass
class: OATH
asset: auth.sumup.com
confidence: 55
reasoning: Discovery docs show require_request_uri_registration=true and PAR endpoint (/oauth2/par) routed for POST. If request_uri parameter validation is weak, attacker could register malicious redirect_uri via PAR then use request_uri in authorize call
evidence_needed: Successful PAR POST with attacker-controlled request_uri returning request_uri usable in authorize flow
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic → observe if request_uri accepted and returned request_uri usable in GET /oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri:...
impact: OAuth code theft via malicious request_uri → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] portal.sumup.com (iriscrm.com) webhook/callback SSRF via supply-chain parameter injection
class: SSRF
asset: portal.sumup.com
confidence: 50
reasoning: portal.sumup.com CNAME → sumup.iriscrm.com (third-party CRM). Now returns 200 on root (live page). CRM platforms typically have webhook/callback/config endpoints accepting URLs. CSP allows *.iriscrm.com for connect-src/frame-src/img-src
evidence_needed: Discovery of URL/redirect/webhook/callback parameter on portal.sumup.com or iriscrm.com endpoints; SSRF payload execution against 169.254.169.254
verify_steps: GET https://portal.sumup.com/ → enumerate all forms/links/endpoints for parameters (url, redirect, callback, webhook, next, return_to, target). GET https://sumup.iriscrm.com/ → same enumeration. If parameter found, test with http://169.254.169.254/latest/meta-data/ (passive enum only)
impact: Cloud metadata exposure → IAM credentials, instance identity → full AWS account compromise — CRITICAL
testability: PASSIVE (enum) → HUMAN_ONLY (exploit requires in-scope confirmation)
[HYP] me.sumup.com Vercel serverless function /api/sso/callback state parameter deserialization flaw
class: AUTH
asset: me.sumup.com
confidence: 45
reasoning: /api/sso/callback returns 403 on anonymous GET but is a Vercel serverless function (distinct from Cloudflare stack). OAuth state is HS256-signed JWT carrying appState. If state validation has deserialization flaw or alg confusion, could bypass auth
evidence_needed: Non-403 response (200/302/500) with crafted state parameter revealing deserialization error or bypass
verify_steps: GET https://me.sumup.com/api/sso/callback?state=eyJhbGciOiJub25lIn0.eyJhcHBTdGF0ZSI6e30nfQ. → observe status/error. GET https://me.sumup.com/api/sso/callback?state=malformed → observe error body for stack traces. OPTIONS https://me.sumup.com/api/sso/callback → capture CORS headers
impact: Auth bypass on merchant self-service portal → OAuth app registration abuse, broad-scope token issuance — HIGH
testability: PASSIVE
[PARKED] OAuth PAR endpoint accepts unauthenticated request_uri registration bypass: Confidence 55 but PAR requires client_auth (dashboard client rejects unauthenticated); request_uri registration likely bound to authenticated client context
[PARKED] portal.sumup.com SSRF via iriscrm.com: Confidence 50 but third-party domain (iriscrm.com) requires explicit in-scope confirmation; no webhook parameters discovered passively yet
[PARKED] me.sumup.com state deserialization flaw: Confidence 45 but all anonymous requests return 403 at edge; requires valid session to reach app logic; state JWT uses HS256 (not alg=none)
[FINAL] No hypotheses survive filtering — all require AUTH_HELPED or HUMAN_ONLY verification beyond passive capabilities
[NEXT] RAG: Fetch SumUp bug bounty program scope.yml (or program page) to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
[LEARN] REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal) — no client_id oracle, no loose redirect_uri
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likely restricted to specific client configs not publicly registrable
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authentication
[LEARN] ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector documented but untestable without valid client flow
[LEARN] ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307/403 to OAuth flow — no debug endpoints or permissive CORS found passively
[LEARN] REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (403, no CORS headers)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfirmed in-scope
[RISK] sumup: 68 — Auth server (auth.sumup.com) exposes critical OAuth configuration (alg=none, PAR, device flow, "none" client auth) with full scope catalog mapping to payment APIs. However, all high-impact vectors require merchant OAuth token (AUTH_HELPED) or registered client — no passive exploit path. API gateway fully gated. Vercel me.sumup.com adds non-Cloudflare surface but auth-gated at edge. Portal introduces third-party supply-chain risk (iriscrm.com) if in-scope confirmed — now returns 200 (live page) enabling parameter enum. Admin panel hardened. Financial attack surface concentrated but access-controlled.
## 2026-09-05 08:43:45 UTC [target] (model nemotron3)
[NEW] portal.sumup.com returns 200 with React CRM login page (iriscrm.com) — live parameter enumeration surface now accessible
[CHANGED] auth.sumup.com/oauth2/auth with request_object returns 405 (rejects at method level before validation)
[CHANGED] me.sumup.com/api/sso/callback returns 307 (not 403) on anonymous GET — redirect to OAuth flow
[CHANGED] auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST — routed but require client authentication params
[CHANGED] api.sumup.com/authorize returns 404 for ALL legacy client_ids — legacy OAuth surface fully dead
[PRIO] auth.sumup.com,8.1,a=9,b=10,t=9,g=3,c=8,f=8
[PRIO] portal.sumup.com,6.8,a=7,b=7,t=6,g=5,c=6,f=7
[PRIO] me.sumup.com,6.5,a=7,b=8,t=6,g=5,c=7,f=7
[PRIO] api.sumup.com,5.5,a=7,b=10,t=4,g=2,c=7,f=5
[PRIO] admin.sumup.com,4.8,a=5,b=7,t=3,g=2,c=6,f=5
[HYP] OAuth PAR request_uri registration bypass via unauthenticated client
class: OATH
asset: auth.sumup.com
confidence: 50
reasoning: Discovery shows require_request_uri_registration=true, PAR endpoint routed (400 on POST), request_uri_parameter_supported=true. If PAR accepts request_uri without client_auth binding, attacker could register malicious redirect_uri
evidence_needed: Successful PAR POST returning request_uri usable in authorize flow without client credentials
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic → observe if request_uri accepted; then GET https://auth.sumup.com/oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri:...
impact: OAuth code theft via malicious request_uri → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] portal.sumup.com (iriscrm.com) webhook/callback SSRF via supply-chain parameter injection
class: SSRF
asset: portal.sumup.com
confidence: 45
reasoning: portal.sumup.com CNAME → sumup.iriscrm.com (third-party CRM). Returns 200 with React app. CRM platforms typically have webhook/callback/config endpoints accepting URLs. CSP allows *.iriscrm.com for connect-src/frame-src/img-src
evidence_needed: Discovery of URL/redirect/webhook/callback parameter on portal.sumup.com or iriscrm.com endpoints; SSRF payload execution against 169.254.169.254
verify_steps: GET https://portal.sumup.com/ → enumerate all forms/links/endpoints for parameters (url, redirect, callback, webhook, next, return_to, target). GET https://sumup.iriscrm.com/ → same enumeration. If parameter found, test with http://169.254.169.254/latest/meta-data/ (passive enum only)
impact: Cloud metadata exposure → IAM credentials, instance identity → full AWS account compromise — CRITICAL
testability: PASSIVE (enum) → HUMAN_ONLY (exploit requires in-scope confirmation)
[HYP] me.sumup.com Vercel /api/sso/callback state parameter deserialization via alg=none confusion
class: AUTH
asset: me.sumup.com
confidence: 40
reasoning: /api/sso/callback is Vercel serverless function (non-Cloudflare). OAuth state is HS256-signed JWT carrying appState. OIDC discovery shows request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true. If state validation uses same JWT parser with alg confusion, could bypass auth
evidence_needed: Non-403/307 response (200/500) with crafted state parameter revealing deserialization error or bypass
verify_steps: GET https://me.sumup.com/api/sso/callback?state=eyJhbGciOiJub25lIn0.eyJhcHBTdGF0ZSI6e30nfQ. → observe status/error. GET https://me.sumup.com/api/sso/callback?state=malformed → observe error body for stack traces. OPTIONS https://me.sumup.com/api/sso/callback → capture CORS headers
impact: Auth bypass on merchant self-service portal → OAuth app registration abuse, broad-scope token issuance — HIGH
testability: PASSIVE
[PARKED] OAuth PAR request_uri registration bypass via unauthenticated client: Confidence 50 but PAR requires client_auth (dashboard client rejects unauthenticated); request_uri registration likely bound to authenticated client context
[PARKED] portal.sumup.com SSRF via iriscrm.com: Confidence 45 but third-party domain (iriscrm.com) requires explicit in-scope confirmation; no webhook parameters discovered passively in React bundle
[PARKED] me.sumup.com state deserialization flaw: Confidence 40 but all anonymous requests return 307 at edge; requires valid session to reach app logic; state JWT uses HS256 (not alg=none)
[FINAL] No hypotheses survive filtering — all require AUTH_HELPED or HUMAN_ONLY verification beyond passive capabilities
[NEXT] RAG: Fetch SumUp bug bounty program scope.yml (or program page) to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
[LEARN] REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sales, virtual-terminal) — no client_id oracle, no loose redirect_uri
[LEARN] ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likely restricted to specific client configs not publicly registrable
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authentication
[LEARN] ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector documented but untestable without valid client flow
[LEARN] ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS found passively
[LEARN] REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfirmed in-scope
[RISK] sumup: 68 — Auth server (auth.sumup.com) exposes critical OAuth configuration (alg=none, PAR, device flow, "none" client auth) with full scope catalog mapping to payment APIs. However, all high-impact vectors require merchant OAuth token (AUTH_HELPED) or registered client — no passive exploit path. API gateway fully gated. Vercel me.sumup.com adds non-Cloudflare surface but auth-gated at edge. Portal introduces third-party supply-chain risk (iriscrm.com) if in-scope confirmed — now returns 200 (live page) enabling parameter enum. Admin panel hardened. Financial attack surface concentrated but access-controlled.
## 2026-09-05 12:09:52 UTC [target] (model nemotron3)
[NEW] api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence (modern me.sumup.com callback rejected) — contradicts prior 404 finding
[NEW] api.sumup.com/authorize exposes wildcard CORS (access-control-allow-origin:*, broad allow-methods, max-age) + SameSite=None cookies on Domain=sumup.com — absent on auth.sumup.com/oauth2/auth
[NEW] portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface accessible
[CHANGED] auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404
[CHANGED] me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
[PRIO] api.sumup.com/authorize,8.3,a=9,b=9,t=9,g=8,c=8,f=7
[PRIO] auth.sumup.com,7.9,a=8,b=10,t=9,g=3,c=8,f=8
[PRIO] portal.sumup.com,6.8,a=7,b=7,t=6,g=5,c=6,f=7
[PRIO] me.sumup.com,6.2,a=7,b=8,t=6,g=5,c=7,f=6
[PRIO] web.sumup.com,5.5,a=6,b=5,t=4,g=9,c=4,f=5
[PRIO] admin.sumup.com,4.8,a=5,b=7,t=3,g=2,c=6,f=5
[HYP] Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration
class: OATH
asset: api.sumup.com/authorize
confidence: 70
reasoning: Endpoint LIVE (302→error flow). Returns invalid_client for unknown IDs vs invalid_request for registered 'dashboard' client. Modern dashboard redirect (https://me.sumup.com/api/sso/callback) rejected on legacy gateway (invalid_request redirect-mismatch) but accepted on modern auth.sumup.com (303 invalid_state). Wildcard CORS + SameSite=None cookies on Domain=sumup.com enable cross-origin interaction.
evidence_needed: Enumeration of legacy callback hosts registered for 'dashboard' and other client_ids on the legacy gateway; successful authorization code flow via legacy callback host
verify_steps: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://me.sumup.com/api/sso/callback&response_type=code&scope=classic&state=test1234 → observe invalid_request redirect-mismatch; GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test1234 → test legacy callback; GET https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=... → enumerate known SDK client_ids for valid redirect patterns
impact: OAuth code theft via legacy callback host → merchant account takeover, full scope access (accounting/invoices/api_keys/lending/readers) — CRITICAL
testability: PASSIVE
[HYP] OAuth PAR request_uri registration bypass via unauthenticated client on auth.sumup.com
class: OATH
asset: auth.sumup.com
confidence: 50
reasoning: Discovery shows require_request_uri_registration=true, PAR endpoint routed (400 on POST), request_uri_parameter_supported=true. If PAR accepts request_uri without client_auth binding, attacker could register malicious redirect_uri.
evidence_needed: Successful PAR POST returning request_uri usable in authorize flow without client credentials
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic → observe if request_uri accepted; then GET https://auth.sumup.com/oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri:...
impact: OAuth code theft via malicious request_uri → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] portal.sumup.com (iriscrm.com) webhook/callback SSRF via supply-chain parameter injection
class: SSRF
asset: portal.sumup.com
confidence: 45
reasoning: portal.sumup.com CNAME → sumup.iriscrm.com (third-party CRM). Returns 200 with React app. CRM platforms typically have webhook/callback/config endpoints accepting URLs. CSP allows *.iriscrm.com for connect-src/frame-src/img-src.
evidence_needed: Discovery of URL/redirect/webhook/callback parameter on portal.sumup.com or iriscrm.com endpoints; SSRF payload execution against 169.254.169.254
verify_steps: GET https://portal.sumup.com/ → enumerate all forms/links/endpoints for parameters (url, redirect, callback, webhook, next, return_to, target). GET https://sumup.iriscrm.com/ → same enumeration. If parameter found, test with http://169.254.169.254/latest/meta-data/ (passive enum only)
impact: Cloud metadata exposure → IAM credentials, instance identity → full AWS account compromise — CRITICAL
testability: PASSIVE (enum) → HUMAN_ONLY (exploit requires in-scope confirmation)
[PARKED] OAuth PAR request_uri registration bypass via unauthenticated client: Confidence 50 but PAR requires client_auth (dashboard client rejects unauthenticated); request_uri registration likely bound to authenticated client context
[PARKED] portal.sumup.com SSRF via iriscrm.com: Confidence 45 but third-party domain (iriscrm.com) requires explicit in-scope confirmation; no webhook parameters discovered passively in React bundle
[FINAL] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (confidence 70)
[NEXT] PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test12345678
[LEARN] ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern dashboard callback rejected on legacy gateway
[LEARN] ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=sumup.com — endpoint-specific divergence from auth.sumup.com
[LEARN] ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 303 invalid_state (state≥8 enforced)
[LEARN] ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method not usable for dashboard client
[LEARN] ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
[LEARN] REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfirmed in-scope
[RISK] sumup: 72 — Auth server (auth.sumup.com) exposes critical OAuth config (alg=none, PAR, device flow, "none" client auth) with full scope catalog mapping to payment APIs. Legacy OAuth gateway (api.sumup.com/authorize) is LIVE with client_id oracle, redirect_uri allowlist divergence, and wildcard CORS — new attack surface for OAuth code theft. All high-impact vectors require merchant OAuth token (AUTH_HELPED) or registered client, but legacy gateway divergence enables passive callback host enumeration. API gateway fully gated. Vercel me.sumup.com adds non-Cloudflare surface but auth-gated at edge. Portal introduces third-party supply-chain risk (iriscrm.com) if in-scope confirmed. Admin panel hardened. Financial attack surface concentrated but access-controlled with new legacy OAuth vector.
## 2026-09-05 15:30:58 UTC [target] (model nemotron3)
[NEW] api.sumup.com/authorize is LIVE (302→auth.sumup.com/flows/oauth2/error) with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern me.sumup.com callback rejected on legacy gateway but accepted on modern auth.sumup.com
[NEW] api.sumup.com/authorize exposes wildcard CORS (access-control-allow-origin:*, broad allow-methods, max-age) + SameSite=None cookies on Domain=sumup.com — absent on auth.sumup.com/oauth2/auth
[NEW] portal.sumup.com returns 200 with React CRM login (iriscrm.com) — live parameter enumeration surface now accessible
[CHANGED] auth.sumup.com/oauth2/par and /oauth2/device return 400 on POST (routed, require client_auth) — not 404
[CHANGED] me.sumup.com/api/sso/callback returns 307 on anonymous GET (redirect to OAuth flow) — not 403
[PRIO] api.sumup.com/authorize,8.3,a=9,b=9,t=9,g=8,c=8,f=7
[PRIO] auth.sumup.com,7.9,a=8,b=10,t=9,g=3,c=8,f=8
[PRIO] portal.sumup.com,6.8,a=7,b=7,t=6,g=5,c=6,f=7
[PRIO] me.sumup.com,6.2,a=7,b=8,t=6,g=5,c=7,f=6
[PRIO] web.sumup.com,5.5,a=6,b=5,t=4,g=9,c=4,f=5
[PRIO] admin.sumup.com,4.8,a=5,b=7,t=3,g=2,c=6,f=5
[HYP] Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration
class: OATH
asset: api.sumup.com/authorize
confidence: 70
reasoning: Endpoint LIVE (302→error flow). Returns invalid_client for unknown IDs vs invalid_request for registered 'dashboard' client. Modern dashboard redirect (https://me.sumup.com/api/sso/callback) rejected on legacy gateway (invalid_request redirect-mismatch) but accepted on modern auth.sumup.com (303 invalid_state). Wildcard CORS + SameSite=None cookies on Domain=sumup.com enable cross-origin interaction.
evidence_needed: Enumeration of legacy callback hosts registered for 'dashboard' and other client_ids on the legacy gateway; successful authorization code flow via legacy callback host
verify_steps: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://me.sumup.com/api/sso/callback&response_type=code&scope=classic&state=test1234 → observe invalid_request redirect-mismatch; GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test1234 → test legacy callback; GET https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=... → enumerate known SDK client_ids for valid redirect patterns
impact: OAuth code theft via legacy callback host → merchant account takeover, full scope access (accounting/invoices/api_keys/lending/readers) — CRITICAL
testability: PASSIVE
[HYP] OAuth PAR request_uri registration bypass via unauthenticated client on auth.sumup.com
class: OATH
asset: auth.sumup.com
confidence: 50
reasoning: Discovery shows require_request_uri_registration=true, PAR endpoint routed (400 on POST), request_uri_parameter_supported=true. If PAR accepts request_uri without client_auth binding, attacker could register malicious redirect_uri.
evidence_needed: Successful PAR POST returning request_uri usable in authorize flow without client credentials
verify_steps: POST https://auth.sumup.com/oauth2/par with client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic → observe if request_uri accepted; then GET https://auth.sumup.com/oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri:...
impact: OAuth code theft via malicious request_uri → merchant account takeover — CRITICAL
testability: PASSIVE
[HYP] portal.sumup.com (iriscrm.com) webhook/callback SSRF via supply-chain parameter injection
class: SSRF
asset: portal.sumup.com
confidence: 45
reasoning: portal.sumup.com CNAME → sumup.iriscrm.com (third-party CRM). Returns 200 with React app. CRM platforms typically have webhook/callback/config endpoints accepting URLs. CSP allows *.iriscrm.com for connect-src/frame-src/img-src.
evidence_needed: Discovery of URL/redirect/webhook/callback parameter on portal.sumup.com or iriscrm.com endpoints; SSRF payload execution against 169.254.169.254
verify_steps: GET https://portal.sumup.com/ → enumerate all forms/links/endpoints for parameters (url, redirect, callback, webhook, next, return_to, target). GET https://sumup.iriscrm.com/ → same enumeration. If parameter found, test with http://169.254.169.254/latest/meta-data/ (passive enum only)
impact: Cloud metadata exposure → IAM credentials, instance identity → full AWS account compromise — CRITICAL
testability: PASSIVE (enum) → HUMAN_ONLY (exploit requires in-scope confirmation)
[PARKED] OAuth PAR request_uri registration bypass via unauthenticated client: Confidence 50 but PAR requires client_auth (dashboard client rejects unauthenticated); request_uri registration likely bound to authenticated client context
[PARKED] portal.sumup.com SSRF via iriscrm.com: Confidence 45 but third-party domain (iriscrm.com) requires explicit in-scope confirmation; no webhook parameters discovered passively in React bundle
[FINAL] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (confidence 70)
[NEXT] PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test12345678
[LEARN] ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern dashboard callback rejected on legacy gateway
[LEARN] ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=sumup.com — endpoint-specific divergence from auth.sumup.com
[LEARN] ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 303 invalid_state (state≥8 enforced)
[LEARN] ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method not usable for dashboard client
[LEARN] ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
[LEARN] REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfirmed in-scope
[RISK] sumup: 72 — Auth server (auth.sumup.com) exposes critical OAuth config (alg=none, PAR, device flow, "none" client auth) with full scope catalog mapping to payment APIs. Legacy OAuth gateway (api.sumup.com/authorize) is LIVE with client_id oracle, redirect_uri allowlist divergence, and wildcard CORS — new attack surface for OAuth code theft. All high-impact vectors require merchant OAuth token (AUTH_HELPED) or registered client, but legacy gateway divergence enables passive callback host enumeration. API gateway fully gated. Vercel me.sumup.com adds non-Cloudflare surface but auth-gated at edge. Portal introduces third-party supply-chain risk (iriscrm.com) if in-scope confirmed. Admin panel hardened. Financial attack surface concentrated but access-controlled with new legacy OAuth vector.
