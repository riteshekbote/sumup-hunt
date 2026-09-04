## 2026-09-03 17:15:24 UTC [target] (model bigpickle)
[PRIO] api.sumup.com,8.0,attack_surface:8 business_value:9 tech_exposure:8 gate_ease:7 cloud_surface:7 freshness:5
[PRIO] admin.sumup.com,7.2,attack_surface:6 business_value:9 tech_exposure:7 gate_ease:5 cloud_surface:7 freshness:5
[PRIO] auth.sumup.com,7.0,attack_surface:7 business_value:8 tech_exposure:7 gate_ease:4 cloud_surface:7 freshness:5
[PRIO] portal.sumup.com,6.8,attack_surface:6 business_value:7 tech_exposure:6 gate_ease:5 cloud_surface:5 freshness:5
[PRIO] dashboard.sumup.com,6.5,attack_surface:6 business_value:8 tech_exposure:6 gate_ease:5 cloud_surface:5 freshness:5
[PRIO] web.sumup.com,5.5,attack_surface:5 business_value:5 tech_exposure:5 gate_ease:6 cloud_surface:5 freshness:5
[HYP] api.sumup.com open API surface exploration
class: BUSLOGIC
asset: api.sumup.com
confidence: 65
reasoning: api.sumup.com returns 404 on root; Cloudflare-fronted; common payment API patterns suggest versioned endpoints (/v1/, /v2/, /merchants/, /payments/) likely exist behind 404. Gateway non-standard ports (2082/2083/8080/8443) warrant probing. No prior enumeration done.
evidence_needed: Valid API endpoints returning 200/401/400 with documented schema (swagger/openapi).
verify_steps: GET https://api.sumup.com/v1/merchants, GET https://api.sumup.com/v1/payments, GET https://api.sumup.com/swagger.json, GET https://api.sumup.com/openapi.json, GET https://api.sumup.com/api-docs, GET https://api.sumup.com/health, GET https://api.sumup.com/v2/, GET https://api.sumup.com/graphql.
impact: Unauthenticated API discovery leaks merchant data, payment endpoints, or internal service maps. Severity: medium-high.
testability: PASSIVE
[HYP] admin.sumup.com internal admin panel
class: AUTH
asset: admin.sumup.com
confidence: 58
reasoning: nginx/1.26.1 serving 403 on root (not cloudflare); backed by AWS ELB in eu-west-1; 403 suggests auth-gated admin panel; common pattern for internal tooling (merchant management, support dashboards).
evidence_needed: Login page, SSO redirect, or leaked path returning 200; framework fingerprinting.
verify_steps: GET https://admin.sumup.com/admin, GET https://admin.sumup.com/login, GET https://admin.sumup.com/health, GET https://admin.sumup.com/api/health, OPTIONS https://admin.sumup.com/, GET https://admin.sumup.com/robots.txt.
impact: Admin panel access could lead to merchant data manipulation or account takeover. Severity: high.
testability: PASSIVE
[HYP] auth.sumup.com OAuth flow enumeration
class: OATH
asset: auth.sumup.com
confidence: 52
reasoning: Redirects to /flows/login; Cloudflare-fronted; SumUp likely uses OAuth2/OIDC for merchant auth across web/mobile; redirect_uri validation flaws are common in payment platforms.
evidence_needed: OAuth authorize endpoint with exposed client_id/redirect_uri parameters; callback endpoints.
verify_steps: GET https://auth.sumup.com/.well-known/openid-configuration, GET https://auth.sumup.com/oauth/authorize, GET https://auth.sumup.com/flows/login (with Referer), GET https://auth.sumup.com/.well-known/oauth-authorization-server.
impact: OAuth misconfig could enable account takeover or session hijacking. Severity: critical.
testability: PASSIVE
[FINAL] 1. api.sumup.com — API surface exploration: confidence 65, highest business+tech value, no auth barriers yet
[FINAL] 2. admin.sumup.com — internal admin panel: confidence 58, high value but 403 gate
[FINAL] 3. auth.sumup.com — OAuth flow enumeration: confidence 52, critical impact if found, limited passive evidence
[NEXT] PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/health && GET https://api.sumup.com/graphql (HEAD first on each, then GET, ≤1 rps)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
[RISK] SumUp: 25 — well-provisioned payment company with Cloudflare edge, likely mature security; low-permission research surface without auth assistance.
## 2026-09-03 19:58:37 UTC [target] (model bigpickle)
[NEW] api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
[CHANGED] admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
[CHANGED] portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.
[PRIO] api.sumup.com,8.0,attack_surface:8 business_value:9 tech_exposure:8 gate_ease:7 cloud_surface:7 freshness:5
[PRIO] admin.sumup.com,7.2,attack_surface:6 business_value:9 tech_exposure:7 gate_ease:5 cloud_surface:7 freshness:5
[PRIO] auth.sumup.com,7.0,attack_surface:7 business_value:8 tech_exposure:7 gate_ease:4 cloud_surface:7 freshness:5
[HYP] api.sumup.com open API surface exploration
class: BUSLOGIC
asset: api.sumup.com
confidence: 65
reasoning: api.sumup.com returns 404 on root; Cloudflare-fronted; common payment API patterns suggest versioned endpoints (/v1/, /v2/, /merchants/, /payments/) likely exist behind 404. Gateway non-standard ports (2082/2083/8080/8443) warrant probing. No prior enumeration done.
evidence_needed: Valid API endpoints returning 200/401/400 with documented schema (swagger/openapi).
verify_steps: GET https://api.sumup.com/v1/merchants, GET https://api.sumup.com/v1/payments, GET https://api.sumup.com/swagger.json, GET https://api.sumup.com/openapi.json, GET https://api.sumup.com/api-docs, GET https://api.sumup.com/health, GET https://api.sumup.com/v2/, GET https://api.sumup.com/graphql.
impact: Unauthenticated API discovery leaks merchant data, payment endpoints, or internal service maps. Severity: medium-high.
testability: PASSIVE
[HYP] admin.sumup.com internal admin panel
class: AUTH
asset: admin.sumup.com
confidence: 58
reasoning: nginx/1.26.1 serving 403 on root (not cloudflare); backed by AWS ELB in eu-west-1; 403 suggests auth-gated admin panel; common pattern for internal tooling (merchant management, support dashboards).
evidence_needed: Login page, SSO redirect, or leaked path returning 200; framework fingerprinting.
verify_steps: GET https://admin.sumup.com/admin, GET https://admin.sumup.com/login, GET https://admin.sumup.com/health, GET https://admin.sumup.com/api/health, OPTIONS https://admin.sumup.com/, GET https://admin.sumup.com/robots.txt.
impact: Admin panel access could lead to merchant data manipulation or account takeover. Severity: high.
testability: PASSIVE
[HYP] auth.sumup.com OAuth flow enumeration
class: OATH
asset: auth.sumup.com
confidence: 52
reasoning: Redirects to /flows/login; Cloudflare-fronted; SumUp likely uses OAuth2/OIDC for merchant auth across web/mobile; redirect_uri validation flaws are common in payment platforms.
evidence_needed: OAuth authorize endpoint with exposed client_id/redirect_uri parameters; callback endpoints.
verify_steps: GET https://auth.sumup.com/.well-known/openid-configuration, GET https://auth.sumup.com/oauth/authorize, GET https://auth.sumup.com/flows/login (with Referer), GET https://auth.sumup.com/.well-known/oauth-authorization-server.
impact: OAuth misconfig could enable account takeover or session hijacking. Severity: critical.
testability: PASSIVE
[FINAL] 1. api.sumup.com — API surface exploration: confidence 65, highest business+tech value, no auth barriers yet
[FINAL] 2. admin.sumup.com — internal admin panel: confidence 58, high value but 403 gate
[FINAL] 3. auth.sumup.com — OAuth flow enumeration: confidence 52, critical impact if found, limited passive evidence
[NEXT] PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/health && GET https://api.sumup.com/graphql (HEAD first on each, then GET, ≤1 rps)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[RISK] SumUp: 25 — well-provisioned payment company with Cloudflare edge, likely mature security; low-permission research surface without auth assistance.
## 2026-09-03 22:32:13 UTC [target] (model bigpickle)
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
[PRIO] api.sumup.com,8.0,attack_surface:8 business_value:9 tech_exposure:8 gate_ease:7 cloud_surface:7 freshness:5
[PRIO] admin.sumup.com,7.2,attack_surface:6 business_value:9 tech_exposure:7 gate_ease:5 cloud_surface:7 freshness:5
[PRIO] auth.sumup.com,7.0,attack_surface:7 business_value:8 tech_exposure:7 gate_ease:4 cloud_surface:7 freshness:5
[PRIO] portal.sumup.com,6.8,attack_surface:6 business_value:7 tech_exposure:6 gate_ease:5 cloud_surface:5 freshness:5
[PRIO] dashboard.sumup.com,6.5,attack_surface:6 business_value:8 tech_exposure:6 gate_ease:5 cloud_surface:5 freshness:5
[PRIO] web.sumup.com,5.5,attack_surface:5 business_value:5 tech_exposure:5 gate_ease:6 cloud_surface:5 freshness:5
[HYP] api.sumup.com open API surface exploration
class: BUSLOGIC
asset: api.sumup.com
confidence: 65
reasoning: api.sumup.com returns 404 on root; Cloudflare-fronted; common payment API patterns suggest versioned endpoints (/v1/, /v2/, /merchants/, /payments/) likely exist behind 404. Gateway non-standard ports (2082/2083/8080/8443) warrant probing. No prior enumeration done.
evidence_needed: Valid API endpoints returning 200/401/400 with documented schema (swagger/openapi).
verify_steps: GET https://api.sumup.com/v1/merchants, GET https://api.sumup.com/v1/payments, GET https://api.sumup.com/swagger.json, GET https://api.sumup.com/openapi.json, GET https://api.sumup.com/api-docs, GET https://api.sumup.com/health, GET https://api.sumup.com/v2/, GET https://api.sumup.com/graphql.
impact: Unauthenticated API discovery leaks merchant data, payment endpoints, or internal service maps. Severity: medium-high.
testability: PASSIVE
[HYP] admin.sumup.com internal admin panel
class: AUTH
asset: admin.sumup.com
confidence: 58
reasoning: nginx/1.26.1 serving 403 on root (not cloudflare); backed by AWS ELB in eu-west-1; 403 suggests auth-gated admin panel; common pattern for internal tooling (merchant management, support dashboards).
evidence_needed: Login page, SSO redirect, or leaked path returning 200; framework fingerprinting.
verify_steps: GET https://admin.sumup.com/admin, GET https://admin.sumup.com/login, GET https://admin.sumup.com/health, GET https://admin.sumup.com/api/health, OPTIONS https://admin.sumup.com/, GET https://admin.sumup.com/robots.txt.
impact: Admin panel access could lead to merchant data manipulation or account takeover. Severity: high.
testability: PASSIVE
[HYP] auth.sumup.com OAuth flow enumeration
class: OATH
asset: auth.sumup.com
confidence: 52
reasoning: Redirects to /flows/login; Cloudflare-fronted; SumUp likely uses OAuth2/OIDC for merchant auth across web/mobile; redirect_uri validation flaws are common in payment platforms.
evidence_needed: OAuth authorize endpoint with exposed client_id/redirect_uri parameters; callback endpoints.
verify_steps: GET https://auth.sumup.com/.well-known/openid-configuration, GET https://auth.sumup.com/oauth/authorize, GET https://auth.sumup.com/flows/login (with Referer), GET https://auth.sumup.com/.well-known/oauth-authorization-server.
impact: OAuth misconfig could enable account takeover or session hijacking. Severity: critical.
testability: PASSIVE
[FINAL] 1. api.sumup.com — API surface exploration: confidence 65, highest business+tech value, no auth barriers yet
[FINAL] 2. admin.sumup.com — internal admin panel: confidence 58, high value but 403 gate
[FINAL] 3. auth.sumup.com — OAuth flow enumeration: confidence 52, critical impact if found, limited passive evidence
[NEXT] PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/health && GET https://api.sumup.com/graphql (HEAD first on each, then GET, ≤1 rps)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
[RISK] SumUp: 25 — well-provisioned payment company with Cloudflare edge, likely mature security; low-permission research surface without auth assistance.
[NEW] api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
[CHANGED] admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
[CHANGED] portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.
[PRIO] api.sumup.com,8.0,attack_surface:8 business_value:9 tech_exposure:8 gate_ease:7 cloud_surface:7 freshness:5
[PRIO] admin.sumup.com,7.2,attack_surface:6 business_value:9 tech_exposure:7 gate_ease:5 cloud_surface:7 freshness:5
[PRIO] auth.sumup.com,7.0,attack_surface:7 business_value:8 tech_exposure:7 gate_ease:4 cloud_surface:7 freshness:5
[HYP] api.sumup.com open API surface exploration
class: BUSLOGIC
asset: api.sumup.com
confidence: 65
reasoning: api.sumup.com returns 404 on root; Cloudflare-fronted; common payment API patterns suggest versioned endpoints (/v1/, /v2/, /merchants/, /payments/) likely exist behind 404. Gateway non-standard ports (2082/2083/8080/8443) warrant probing. No prior enumeration done.
evidence_needed: Valid API endpoints returning 200/401/400 with documented schema (swagger/openapi).
verify_steps: GET https://api.sumup.com/v1/merchants, GET https://api.sumup.com/v1/payments, GET https://api.sumup.com/swagger.json, GET https://api.sumup.com/openapi.json, GET https://api.sumup.com/api-docs, GET https://api.sumup.com/health, GET https://api.sumup.com/v2/, GET https://api.sumup.com/graphql.
impact: Unauthenticated API discovery leaks merchant data, payment endpoints, or internal service maps. Severity: medium-high.
testability: PASSIVE
[HYP] admin.sumup.com internal admin panel
class: AUTH
asset: admin.sumup.com
[FINAL] 3. auth.sumup.com — OAuth flow enumeration: confidence 52, critical impact if found, limited passive evidence
[NEXT] PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/health && GET https://api.sumup.com/graphql (HEAD first on each, then GET, ≤1 rps)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
[RISK] SumUp: 25 — well-provisioned payment company with Cloudflare edge, likely mature security; low-permission research surface without auth assistance.
[NEW] api.sumup.com: non-standard ports (2082/2083/2086/2087/8080/8443) detected; shared edge/proxy noted but verify with proper scan.
[CHANGED] admin.sumup.com: nginx/1.26.1 + AWS ELB (eu-west-1); 403 on root confirmed.
[CHANGED] portal.sumup.com: third-party CRM (iriscrm.com) CNAME confirmed; SSRF surface plausible via webhook/callback.
[PRIO] api.sumup.com,8.0,attack_surface:8 business_value:9 tech_exposure:8 gate_ease:7 cloud_surface:7 freshness:5
[PRIO] admin.sumup.com,7.2,attack_surface:6 business_value:9 tech_exposure:7 gate_ease:5 cloud_surface:7 freshness:5
[PRIO] auth.sumup.com,7.0,attack_surface:7 business_value:8 tech_exposure:7 gate_ease:4 cloud_surface:7 freshness:5
[HYP] api.sumup.com open API surface exploration
class: BUSLOGIC
asset: api.sumup.com
confidence: 65
reasoning: api.sumup.com returns 404 on root; Cloudflare-fronted; common payment API patterns suggest versioned endpoints (/v1/, /v2/, /merchants/, /payments/) likely exist behind 404. Gateway non-standard ports (2082/2083/8080/8443) warrant probing. No prior enumeration done.
evidence_needed: Valid API endpoints returning 200/401/400 with documented schema (swagger/openapi).
verify_steps: GET https://api.sumup.com/v1/merchants, GET https://api.sumup.com/v1/payments, GET https://api.sumup.com/swagger.json, GET https://api.sumup.com/openapi.json, GET https://api.sumup.com/api-docs, GET https://api.sumup.com/health, GET https://api.sumup.com/v2/, GET https://api.sumup.com/graphql.
impact: Unauthenticated API discovery leaks merchant data, payment endpoints, or internal service maps. Severity: medium-high.
testability: PASSIVE
[HYP] admin.sumup.com internal admin panel
class: AUTH
asset: admin.sumup.com
confidence: 58
reasoning: nginx/1.26.1 serving 403 on root (not cloudflare); backed by AWS ELB in eu-west-1; 403 suggests auth-gated admin panel; common pattern for internal tooling (merchant management, support dashboards).
evidence_needed: Login page, SSO redirect, or leaked path returning 200; framework fingerprinting.
verify_steps: GET https://admin.sumup.com/admin, GET https://admin.sumup.com/login, GET https://admin.sumup.com/health, GET https://admin.sumup.com/api/health, OPTIONS https://admin.sumup.com/, GET https://admin.sumup.com/robots.txt.
impact: Admin panel access could lead to merchant data manipulation or account takeover. Severity: high.
testability: PASSIVE
[HYP] auth.sumup.com OAuth flow enumeration
class: OATH
asset: auth.sumup.com
confidence: 52
reasoning: Redirects to /flows/login; Cloudflare-fronted; SumUp likely uses OAuth2/OIDC for merchant auth across web/mobile; redirect_uri validation flaws are common in payment platforms.
evidence_needed: OAuth authorize endpoint with exposed client_id/redirect_uri parameters; callback endpoints.
verify_steps: GET https://auth.sumup.com/.well-known/openid-configuration, GET https://auth.sumup.com/oauth/authorize, GET https://auth.sumup.com/flows/login (with Referer), GET https://auth.sumup.com/.well-known/oauth-authorization-server.
impact: OAuth misconfig could enable account takeover or session hijacking. Severity: critical.
testability: PASSIVE
[FINAL] 1. api.sumup.com — API surface exploration: confidence 65, highest business+tech value, no auth barriers yet
[FINAL] 2. admin.sumup.com — internal admin panel: confidence 58, high value but 403 gate
[FINAL] 3. auth.sumup.com — OAuth flow enumeration: confidence 52, critical impact if found, limited passive evidence
[NEXT] PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/health && GET https://api.sumup.com/graphql (HEAD first on each, then GET, ≤1 rps)
[LEARN] ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[RISK] SumUp: 25 — well-provisioned payment company with Cloudflare edge, likely mature security; low-permission research surface without auth assistance.
[NEW] auth.sumup.com OIDC/OAuth discovery docs fully exposed: `.well-known/openid-configuration` and `.well-known/oauth-authorization-server` return 200 with complete endpoint map + scope catalog. Live endpoints confirmed: `/oauth2/auth` (302→login), `/oauth2/token` (405), `/oauth2/nonce` (405), `/userinfo` (401), `.well-known/jwks.json` (200).
[NEW] Scope catalog on auth.sumup.com enumerates the merchant API resource model: merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/receipts/sales/roles + read/write variants — directly maps the hidden api.sumup.com surface.
[NEW] Security-relevant OAuth settings revealed: PAR endpoint `/oauth2/par` (404 via GET, POST-only), device flow `/oauth2/device`, `token_endpoint_auth_methods_supported` includes `"none"`, request_object alg includes `"none"` + RS256, DPoP supported, PKCE S256.
[CHANGED] api.sumup.com: uniform 404 on ALL enumerated paths (v0/v0.1/v1/v2/merchants/checkouts/transactions/etc.) — unauthenticated surface fully gated at gateway; scope-derived paths also 404. API enumeration dead without auth.
[NEW] auth.sumup.com: OIDC discovery fully exposed (.well-known/openid-configuration + oauth-authorization-server return 200) revealing complete endpoint map (/oauth2/auth, /oauth2/token, /oauth2/par, /oauth2/device, /oauth2/nonce, /userinfo, /oauth2/revoke, /oauth2/sessions/logout) and live service (auth 302→login, token/nonce 405, jwks 200).
[NEW] auth.sumup.com: scope catalog documents merchant API resource model (merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/receipts/sales/roles + read/write) — maps hidden api.sumup.com surface.
[NEW] auth.sumup.com: security-relevant OAuth flags exposed — PAR + request_uri supported (require_request_uri_registration), device flow, token_endpoint_auth_methods incl "none", request_object alg incl "none", DPoP, PKCE S256 (OIDC doc also lists "plain").
[CHANGED] api.sumup.com: uniform 404 on every enumerated path (v0/v0.1/v1/v2, scope-derived resources) — unauthenticated API surface fully gated; enumeration dead without auth.
[PRIO] auth.sumup.com,8.6,a=8,b=9,t=9,g=5,c=8,f=7
[PRIO] api.sumup.com,7.4,a=8,b=9,t=8,g=5,c=8,f=4
[PRIO] admin.sumup.com,7.0,a=6,b=9,t=7,g=3,c=8,f=5
[HYP] OAuth request_uri / PAR token theft on auth.sumup.com
class: OATH
asset: auth.sumup.com
confidence: 60
reasoning: Discovery confirms request_parameter and request_uri_parameter both supported (require_request_uri_registration:true) and request_object_signing_alg includes "none" + RS256; PAR endpoint /oauth2/par is POST-only. JWT request objects with alg "none" plus request_uri handling are classic OAuth code-theft / CSRF vectors. auth endpoint /oauth2/auth is live (302 to login).
evidence_needed: /oauth2/par POST behavior with attacker-controlled client + request_uri pointing to attacker, or request object accepted with alg=none.
verify_steps: PASSIVE only: HEAD/GET /oauth2/auth with valid+invalid client_id/redirect_uri to observe error codes that reveal redirect_uri validation; OPTIONS /oauth2/par; GET /oauth2/par to confirm 405 POST-only. No token exchange (interactive).
impact: Authorization-code theft / account-takeover of merchant OAuth sessions — CRITICAL.
testability: AUTH_HELPED
[HYP] api.sumup.com scope-mapped resource BOLA via OAuth tokens
class: IDOR
asset: api.sumup.com
confidence: 60
reasoning: Scope catalog (merchants/transactions/payouts/readers/checkouts/customers/api_keys/refunds/sales) precisely names the hidden API resources; api.sumup.com 404s all unauthenticated paths, so surface is auth-gated — BOLA/IDOR must be tested with a granted merchant token cross-tenant.
evidence_needed: Authenticated requests where changing a resource id (merchant/transaction/reader) returns another tenant's data.
verify_steps: Requires valid OAuth merchant token; cross-tenant id manipulation on scope-derived endpoints. AUTH_HELPED. Not passable without token.
impact: Cross-tenant financial/PII data exposure — HIGH/CRITICAL.
testability: AUTH_HELPED
[HYP] auth.sumup.com client_credentials / token auth-method "none" misuse
class: AUTH
asset: auth.sumup.com
reasoning: token_endpoint_auth_methods_supported includes "none" and grant_types include client_credentials and device_code; device_code grant with "none" auth is a known account-linking/abuse vector. /oauth2/token and /oauth2/device are live (405/404 on GET = POST endpoints).
evidence_needed: device authorization accepting a client_id that allows arbitrary merchant linking without issuer verification.
verify_steps: POST /oauth2/device and /oauth2/par with scoped client to observe error taxonomy — passive-safe; no code redemption.
impact: OAuth device-flow abuse / token impersonation — HIGH.
confidence: 55
testability: PASSIVE
[FINAL] 1. OAuth request_uri/PAR token theft on auth.sumup.com (conf 60) — live endpoint, confirmed settings, passive-testable.
[FINAL] 2. api.sumup.com scope-mapped resource BOLA (conf 60) — high value, needs merchant token (AUTH_HELPED).
[FINAL] 3. client_credentials / device-flow "none" auth misuse on auth.sumup.com (conf 55) — passive error-taxonomy.
[PARKED] SSRF @ portal.sumup.com (iriscrm.com): third-party CNAME, in-scope unconfirmed, needs active interaction.
[NEXT] PROBE: GET https://auth.sumup.com/oauth2/auth?response_type=code&client_id=<test>&redirect_uri=https://attacker.example.com/cb&scope=openid (observe error vs redirect to reveal redirect_uri allowlist validation); GET https://auth.sumup.com/oauth2/auth?response_type=code&client_id=<real-public-cid>&redirect_uri=evil (compare). Then OPTIONS /oauth2/par, /oauth2/device. ≤1 rps.
[LEARN] ACCEPTED OATH @ auth.sumup.com: OIDC/OAuth discovery documents are public and enumerate live endpoints + full scope/resource model; /oauth2/auth is a live interactive flow. High-value deep-hunt surface.
[LEARN] ACCEPTED IDOR @ api.sumup.com: scope catalog names the API resource model, but all unauthenticated paths 404 — BOLA test requires a merchant OAuth token (AUTH_HELPED), not passive.
[LEARN] REJECTED MISCONFIG @ auth.sumup.com: x-envoy-decorator-operation leaks k8s service name identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
[RISK] SumUp: 28 — Cloudflare edge + Envoy/Istio + fully-gated API; unauthenticated surface thin (only OIDC discovery + login), real findings require a scoped merchant OAuth token; mature security posture.
## 2026-09-04 00:35:55 UTC [target] (model bigpickle)
[FINAL] 2. OAuth redirect_uri validation bypass on auth.sumup.com flows (confidence 55)
[FINAL] 3. Admin panel authentication bypass or info leak on admin.sumup.com (confidence 45)
[NEXT] PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagger.json, /openapi.json, /api-docs.
[LEARN] ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
[LEARN] ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
[LEARN] REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
[LEARN] ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passively.
[RISK] sumup: 75 — Core payment API (api.sumup.com) and auth server (auth.sumup.com) are high-value targets with confirmed live HTTP, Cloudflare/AWS edge, and standard OAuth/API patterns that historically yield IDOR/OAuth flaws. Wildcard-dominated DNS limits subdomain enum but concentrates attack surface on known hosts. Third-party portal (iriscrm.com) adds supply-chain risk if in-scope. Admin panel (admin.sumup.com) exposes internal tooling surface behind ELB/nginx.
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
[NEW] me.sumup.com identified as a distinct merchant self-service asset served by Vercel (not Cloudflare/nginx/ELB). Root and /settings/oauth2-applications both 307 → auth.sumup.com OAuth with public `client_id=dashboard`, registered redirect_uri `https://me.sumup.com/api/sso/callback`.
[NEW] Real public OAuth client `dashboard` exposed; its production scope catalog differs from OIDC discovery and developer docs: `openid offline classic accounting.read/write invoices.read/write api_keys api_keys:write readers.read/write lending.read/write receivables.read/write unified_customer_directory:read/write` — wider hidden API resource model.
[NEW] OAuth `state` on the dashboard flow is an HS256-signed JWT carrying `appState{flow,pathname,queryParams}`; server enforces state≥8 chars.
[CHANGED] auth.sumup.com redirect_uri validation CONFIRMED strict allowlist for `client_id=dashboard`: attacker host, subdomain-confusion, and path-traversal redirect_uri all rejected (`invalid_request` → error on server flow page, never redirected to unregistered URI); known-good `https://me.sumup.com/api/sso/callback` produces 303 to me.sumup.com callback. Naive redirect_uri bypass refuted.
[CHANGED] /oauth2/par and /oauth2/device return 404 on OPTIONS (documented but not routed), while /oauth2/token and /oauth2/revoke return 200 on OPTIONS — PAR/device grant routes likely not deployed at routing level.
[PRIO] auth.sumup.com,8.6,a=9,b=9,t=9,g=4,c=8,f=8
[PRIO] me.sumup.com,7.9,a=7,b=9,t=8,g=6,c=8,f=8
[PRIO] api.sumup.com,7.0,a=8,b=9,t=8,g=4,c=8,f=4
[PRIO] admin.sumup.com,6.9,a=6,b=9,t=7,g=3,c=8,f=5
[HYP] OAuth state JWT appState path sink on auth.sumup.com/me.sumup.com
class: OATH
asset: auth.sumup.com
confidence: 45
reasoning: state is an HS256 JWT with appState{flow,pathname:'/',queryParams:{}} issued by the dashboard backend and returned on the OAuth callback (me.sumup.com/api/sso/callback?state=...). The pathname/queryParams ride inside the signed state; if any downstream consumer trusts appState.pathname to drive a post-login redirect or UI step without re-validating, manipulation of the post-auth redirect sink is possible. HS256 forgery requires the app secret, so exploitability hinges on a weak secret or an accepted-lesser-alg path.
evidence_needed: Verify callback re-validates state JWT signature AND uses appState.pathname for a redirect (open-redirect-to-self or flow-skip) — requires session or a way to observe post-login handling. Not resolvable by anonymous HEAD/GET alone.
verify_steps: PASSIVE limit: GET /oauth2/auth with client_id=dashboard, known-good redirect_uri, crafted/forged state (alg=none header + forged appState) and observe whether the 303 callback reflects an altered error vs proceeds — a reflected difference indicates state is parsed/trusted. Note: forging requires secret; alg=none probe is a best-effort signal only.
impact: Post-auth redirect/flow manipulation or state-session confusion → account takeover vector. Severity: high if actionable.
testability: AUTH_HELPED
[HYP] me.sumup.com Vercel surface / OAuth2-app-registration UX logic
class: BUSLOGIC
asset: me.sumup.com
confidence: 50
reasoning: me.sumup.com is a fresh, non-Cloudflare (Vercel) merchant asset with a self-service OAuth2 application registry (/settings/oauth2-applications) reachable only via authenticated dashboard OAuth. Vercel origin differs from the hardened Cloudflare/Envoy edge seen elsewhere; a newer asset is a plausible spot for exposed serverless functions, API routes, or permissive CORS on the SSO callback. Authenticated interaction needed.
evidence_needed: Authenticated dashboard session to enumerate me.sumup.com/api/* routes and OAuth2-app registration fields (redirect_uri registration, scopes selectable).
verify_steps: PASSIVE: GET https://me.sumup.com/api/sso/callback (anonymous) to observe error handling/CORS; GET https://me.sumup.com/_vercel/insights or well-known; enumerate /api/* anonymously for non-401 responses. Full test AUTH_HELPED.
impact: Merchant self-service misconfig → OAuth app registration abuse, broad-scope token issuance, or data exposure. Severity: high.
testability: AUTH_HELPED
[HYP] api.sumup.com BOLA via dashboard-client scopes
class: IDOR
asset: api.sumup.com
confidence: 60
reasoning: Dashboard client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps the real hidden API resources; api.sumup.com 404s all anonymous paths. Cross-tenant BOLA requires a granted merchant token (from dashboard scope) then ID-swapping on resource ids.
evidence_needed: Authenticated requests where changing resource id returns another tenant's data.
verify_steps: Requires valid OAuth merchant token; cross-tenant id manipulation on scope-derived endpoints. Not passable without token.
impact: Cross-tenant financial/PII data exposure. Severity: critical.
testability: AUTH_HELPED
[PARKED] OAuth redirect_uri validation bypass on auth.sumup.com: REFUTED for naive vectors — strict allowlist confirmed for client_id=dashboard (attacker-host/subdomain/path-traversal all rejected, errors on server page). No viable passive redirect_uri bypass.
[PARKED] OAuth request_uri/PAR token theft: PAR+device routes return 404 on OPTIONS (unrouted) and redirect_uri is strictly allowlisted; requires a registered attacker client + key → not passive, low current yield.
[PARKED] SSRF @ portal.sumup.com (iriscrm.com): third-party CNAME, in-scope unconfirmed, needs active interaction.
[FINAL] 1. api.sumup.com scope-mapped BOLA (conf 60) — high financial value, needs merchant token (AUTH_HELPED).
[FINAL] 2. me.sumup.com Vercel merchant surface / OAuth2-app-registration logic (conf 50) — new host, non-Cloudflare origin, fresh UX surface.
[FINAL] 3. OAuth state JWT appState path sink (conf 45) — HS256 secret-dependent, weak passive signal.
[NEXT] PROBE: GET https://me.sumup.com/api/sso/callback (anonymous, observe error taxonomy + CORS/allow headers), GET https://me.sumup.com/_vercel/* well-known + /api/* anonymous enumeration (non-401), OPTIONS https://me.sumup.com/api/sso/callback. Then confirm dashboard-client scope reality by re-fetching the me.sumup.com redirect and capturing the full state/scope for offline JWT analysis. ≤1 rps, GET/HEAD/OPTIONS only.
[LEARN] ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal bypass refuted; errors surfaced on server flow page.
[LEARN] ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudflare attack surface.
[LEARN] ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 200 — PAR/device grants likely not deployed at routing level.
[LEARN] ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broader hidden api.sumup.com resource model than OIDC discovery.
[RISK] SumUp: 30 — Cloudflare+Envoy edge with strict OAuth redirect_uri allowlist and fully-gated api.sumup.com; anonymous surface thin and hardening effective (redirect_uri bypass refuted). Real findings concentrate in merchant-token-gated BOLA and the new Vercel-served me.sumup.com self-service surface.
