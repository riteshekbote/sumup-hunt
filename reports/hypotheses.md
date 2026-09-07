# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-02 21:55:21 UTC

## RANKED HYPOTHESES 2026-09-03 00:07:18 UTC

## RANKED HYPOTHESES 2026-09-03 04:11:41 UTC

## RANKED HYPOTHESES 2026-09-03 09:02:20 UTC

## RANKED HYPOTHESES 2026-09-03 13:32:16 UTC

## RANKED HYPOTHESES 2026-09-03 17:25:51 UTC
- [65] api.sumup.com: api.sumup.com open API surface exploration (from art/lead_bigpickle.txt)
- [65] api.sumup.com: API versioned endpoint IDOR/BOLA on merchant resources (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/h
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/v1/ (HEAD) → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, 
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-

## RANKED HYPOTHESES 2026-09-03 19:59:04 UTC
- [65] api.sumup.com: api.sumup.com open API surface exploration (from art/lead_bigpickle.txt)
- [65] api.sumup.com: API versioned endpoint IDOR/BOLA on merchant resources (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagg
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sumup.com/swagger.json && GET https://api.sumup.com/openapi.json && GET https://api.sumup.com/v1/merchants && GET https://api.sumup.com/h
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passive
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-

## RANKED HYPOTHESES 2026-09-03 22:32:22 UTC
- [65] api.sumup.com: API versioned endpoint IDOR/BOLA on merchant resources (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sumup.com/v1/ (HEAD) → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, 
- NEXT(hypotheses-nemotron3.txt): PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagg
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-
- LEARN: ACCEPTED OATH @ auth.sumup.com: OIDC/OAuth discovery documents are public and enumerate live endpoints + full scope/resource model; /oauth2/auth is a live inter
- LEARN: ACCEPTED IDOR @ api.sumup.com: scope catalog names the API resource model, but all unauthenticated paths 404 — BOLA test requires a merchant OAuth token (AUTH_H
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: x-envoy-decorator-operation leaks k8s service name identity.svc.cluster.local — header/banner leak is explicit out-of-scope
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passive
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.

## RANKED HYPOTHESES 2026-09-04 00:36:06 UTC
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation (from art/lead_nemotron3.txt)
- [65] api.sumup.com: API versioned endpoint IDOR/BOLA on merchant resources (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: HEAD https://api.sumup.com/v1/ → observe status, Server header, WWW-Authenticate, rate-limit headers. If 404, repeat for /v2/, /beta/, /internal/, /swagg
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMT
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passive
- LEARN: ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-
- LEARN: ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passive
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.

## RANKED HYPOTHESES 2026-09-04 05:12:55 UTC
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_nemotron3.txt)
- [50] me.sumup.com: me.sumup.com Vercel surface / OAuth2-app-registration UX logic (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://me.sumup.com/api/sso/callback (anonymous, observe error taxonomy + CORS/allow headers), GET https://me.sumup.com/_vercel/insights or well-kno
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMT
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc

## RANKED HYPOTHESES 2026-09-04 09:55:16 UTC
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_nemotron3.txt)
- [60] api.sumup.com: api.sumup.com BOLA via dashboard-client per-client scope catalog (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMT
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc

## RANKED HYPOTHESES 2026-09-04 14:14:31 UTC
- [70] auth.sumup.com: auth.sumup.com OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_bigpickle.txt)
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Me
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMT
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc

## RANKED HYPOTHESES 2026-09-04 17:50:13 UTC
- [70] auth.sumup.com: auth.sumup.com OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_bigpickle.txt)
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Me
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=test&redirect_uri=https://example.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMT
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc

## RANKED HYPOTHESES 2026-09-04 20:02:49 UTC
- [70] auth.sumup.com: OAuth token_endpoint_auth_methods=none enables public client impersonation via PAR flow (from art/lead_nemotron3.txt)
- [55] api.sumup.com/authorize: Legacy api.sumup.com/authorize client_id oracle + loose redirect_uri on legacy-registered test/dev OAuth clients (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sumup.com/authorize?client_id={l} where l iterates legacy-format SDK client IDs (e.g. sumup-ios-sdk, sumup.pos, reader, sales, virtual-te
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sumup.com/oauth2/par with `client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Me
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test.
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources.
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog (accounting/invoices/api_keys/lending/receivables/unified_customer_directory/readers) maps a broa
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog from auth.sumup.com defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing (Host, X-Forwarded-For, X-Original-URL) yields identical 403 — no auth bypass via passive header manipulation.
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks apigateway2-headless.identity.svc.cluster.local — header/banner leak is explicit out-of-sc

## RANKED HYPOTHESES 2026-09-04 22:20:56 UTC
- [55] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + loose redirect_uri on api.sumup.com/authorize (from art/lead_nemotron3.txt)
- [45] api.sumup.com/authorize: Legacy api.sumup.com/authorize registry divergence: older client profile with unknown (possibly attacker-reachable) registered redirect_uri (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=https://example.com/callback&response_type=code&scope=classic → observe status, 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint exists on API gateway (distinct from auth.sumup.com) with legacy SDK client_ids and pot
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" — public client impersonation vector; requires live PAR+token flow test
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: Full discovery docs expose PAR, device flow, request_object alg "none", scope catalog mapping 1:1 to merchant API resources
- LEARN: ACCEPTED OATH @ auth.sumup.com: redirect_uri is strictly allowlisted per client (client_id=dashboard confirmed) — naive redirect_uri/subdomain/path-traversal by
- LEARN: ACCEPTED AUTH @ me.sumup.com: me.sumup.com is a distinct Vercel-served merchant self-service asset behind dashboard OAuth (client_id=dashboard) — new non-Cloudf
- LEARN: ACCEPTED MISCONFIG @ auth.sumup.com: /oauth2/par & /oauth2/device documented but return 404 on OPTIONS (unrouted) while /oauth2/token & /oauth2/revoke return 20
- LEARN: ACCEPTED OATH @ auth.sumup.com: dashboard-client scope catalog maps broader hidden api.sumup.com resource model than OIDC discovery
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths 404 unauthenticated; scope catalog defines resource model but requires merchant token
- LEARN: REJECTED AUTH @ admin.sumup.com: Header spoofing yields identical 403 — no auth bypass via passive header manipulation
- LEARN: REJECTED MISCONFIG @ api.sumup.com: x-envoy-decorator-operation leaks k8s service name — header/banner leak explicit out-of-scope class

## RANKED HYPOTHESES 2026-09-05 00:24:44 UTC
- [65] auth.sumup.com: OAuth request_object alg=none enables algorithm confusion on auth.sumup.com (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): RAG: Fetch SumUp bug bounty program scope.yml to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
- LEARN: REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sale
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likel
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authen
- LEARN: ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector docu
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS found 
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 04:44:37 UTC
- [55] auth.sumup.com: OAuth PAR endpoint accepts unauthenticated request_uri registration bypass (from art/lead_nemotron3.txt)
- [45] web.sumup.com: web.sumup.com subdomain takeover via decommissioned dedicated A record (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: query https://rdap.db.ripe.net/ip/77.246.42.130 and https://rdap.db.ripe.net/ip/77.246.42.0/23 for netname/org/ASN to determine whether web.sumup.com's IP 
- NEXT(hypotheses-nemotron3.txt): RAG: Fetch SumUp bug bounty program scope.yml (or program page) to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
- LEARN: REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sale
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likel
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authen
- LEARN: ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector docu
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307/403 to OAuth flow — no debug endpoints or permissive CORS fo
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (403, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 08:45:15 UTC
- [55] api.sumup.com/authorize: api.sumup.com/authorize ORACLE + legacy redirect-set divergence → hidden legacy callback host (from art/lead_bigpickle.txt)
- [50] auth.sumup.com: OAuth PAR request_uri registration bypass via unauthenticated client (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: enumerate SumUp legacy OAuth callback constants from public SDK/docs (github.com/sumup SDKS + archived SumUp mobile-web docs) to reconstruct the legacy `da
- NEXT(hypotheses-nemotron3.txt): RAG: Fetch SumUp bug bounty program scope.yml (or program page) to confirm if portal.sumup.com / iriscrm.com is in-scope for SSRF testing
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint is LIVE (302→auth.sumup.com/flows/oauth2/error), not 404; the earlier "404 for all legacy client_ids" log ran 
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: access-control-allow-origin:* + broad allow-methods + max-age on legacy OAuth gateway responses, absent on auth.su
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback yields 303 invalid_state (sta
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint is LIVE (302→auth.sumup.com/flows/oauth2/error), not 404 — the recorded "legacy authorize dead" logged an unex
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: access-control-allow-origin:* + broad allow-methods + max-age on legacy OAuth gateway responses, absent on auth.su
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback returns 303 invalid_state obj
- LEARN: ACCEPTED MISCONFIG @ web.sumup.com: 77.246.42.130 is in Rackspace lease pool UK-RACKSPACE-20070509 (Rackspace Ltd.), not a SumUp-owned netblock — supporting inf
- LEARN: REJECTED OATH @ api.sumup.com/authorize: Legacy OAuth authorize endpoint returns 404 for all known legacy SDK client_ids (sumup-ios-sdk, sumup.pos, reader, sale
- LEARN: ACCEPTED AUTH @ auth.sumup.com: token_endpoint_auth_methods_supported includes "none" but dashboard client rejects unauthenticated token requests — "none" likel
- LEARN: ACCEPTED OAUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) endpoints ARE routed and respond to POST (not 404) but require client authen
- LEARN: ACCEPTED OATH @ auth.sumup.com: request_object_signing_alg_values_supported includes "none" + request_parameter_supported=true — algorithm confusion vector docu
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset confirmed; all anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS found 
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 12:10:03 UTC
- [70] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test1234
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 303 invali
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 15:31:08 UTC
- [70] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (from art/lead_nemotron3.txt)
- [55] api.sumup.com/authorize: api.sumup.com/authorize legacy redirect-set divergence → hidden legacy callback host (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: harvest legacy SumUp OAuth callback constants from public SDKs/docs (github.com/sumup repos, archived SumUp mobile/web docs, crt.sh historical certs for *.
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test1234
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint is LIVE (302→auth.sumup.com/flows/oauth2/error), not 404 — the recorded "legacy authorize dead" logged an unex
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: access-control-allow-origin:* + broad allow-methods + max-age on legacy OAuth gateway responses, absent on auth.su
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback returns 303 invalid_state obj
- LEARN: ACCEPTED MISCONFIG @ web.sumup.com: 77.246.42.130 is in Rackspace lease pool UK-RACKSPACE-20070509 (Rackspace Ltd.), not a SumUp-owned netblock — supporting inf
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 303 invali
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 17:52:35 UTC
- [75] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (from art/lead_nemotron3.txt)
- [55] api.sumup.com/authorize: Legacy api.sumup.com/authorize redirect-set divergence confirmed but no accepted callback host recovered (dormant) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: enumerate crt.sh historical certificate subdomains of *.sumup.com (both q=%.sumup.com current + historical) and filter for OAuth-callback-shaped hosts (oau
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://legacy.sumup.com/callback&response_type=code&scope=classic&state=test1234567
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Side-by-side control confirms identical dashboard+me.sumup.com/api/sso/callback request is ACCEPTED (302→auth-callback 
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com/authorize: RAG of public SumUp SDKs/docs (github.com/sumup: sumup-php, sumup-dotnet, sumup-go, sumup-developer, developer.sumu
- LEARN: REJECTED OATH @ api.sumup.com/authorize: Bounded legacy-callback host enumeration (dashboard/app/app-sumup/my/secure/me/www.sumup.com × /callback|/api/callback|
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 19:34:42 UTC
- [75] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri allowlist divergence enables callback host enumeration (from art/lead_nemotron3.txt)
- [55] api.sumup.com/authorize: Legacy api.sumup.com/authorize redirect-set divergence — callback host enumeration via crt.sh historical subdomains (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://legacy.sumup.com/callback&response_type=code&scope=classic&state=test1234567
- LEARN: REJECTED OATH @ api.sumup.com/authorize: Common legacy callback host candidates (dashboard/app/my/secure/me/www.sumup.com × callback paths) all return invalid_r
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com/authorize: RAG of public SumUp SDKs/docs yields only the current strict per-registration model — no legacy callback host const
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live — https://me.sumup.com/api/sso/callback returns 302→login flow on the accepted h
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Side-by-side control confirms identical dashboard+me.sumup.com/api/sso/callback request ACCEPTED on auth.sumup.com/oaut
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 21:50:26 UTC
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- [40] portal.sumup.com: portal.sumup.com (iriscrm.com) webhook/callback SSRF via supply-chain parameter injection (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://legacy.sumup.com/callback&response_type=code&scope=classic&state=test1234567
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir

## RANKED HYPOTHESES 2026-09-05 23:45:33 UTC
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- [20] help.sumup.com: help.sumup.com Zendesk-backed content API exposes non-public article/draft data anonymously (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Legacy OAuth redirect recovery is closed on all executable-passive surface (52 combos invalid_request; classification now complete: app-auth/settlements-
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface

## RANKED HYPOTHESES 2026-09-06 04:05:10 UTC
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- [65] api.sumup.com: api.sumup.com open API surface exploration (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Legacy OAuth redirect recovery is closed on all executable-passive surface (52 combos invalid_request; classification now complete: app-auth/settlements-
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Official SumUp MCP (Cloudflare Worker, bearer JWKS from auth.sumup.com, Durable Object agent) LIVE and absent from inventory — s
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface

## RANKED HYPOTHESES 2026-09-06 08:47:51 UTC
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- [65] auth.sam-app.ro/oauth2/register: Unauthenticated RFC 7591 dynamic client registration on staging identity host enables self-issued OAuth clients (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://auth.sam-app.ro/oauth2/token with the registered client (`cl_2F13S19SAB94CA24XV3KQFC4G7`) + client_secret_basic + empty scope via client_cred
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: RFC 7591 dynamic client registration LIVE unauthenticated (POST /oauth2/register → 201 client_id+secret+chosen redirect_uris) d
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Official SumUp MCP (Cloudflare Worker, bearer JWKS from auth.sumup.com, Durable Object agent) LIVE and absent from inventory — s
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class

## RANKED HYPOTHESES 2026-09-06 12:51:50 UTC
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- [65] auth.sam-app.ro/oauth2/register: Legacy OAuth allowlist recovery via stage oracle + api-gateway registry sync gap (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://auth.sam-app.ro/oauth2/token with the registered client (`cl_2F13S19SAB94CA24XV3KQFC4G7`) + client_secret_basic + empty scope via client_cred
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: RFC 7591 dynamic client registration LIVE unauthenticated (POST /oauth2/register → 201 client_id+secret+chosen redirect_uris) d
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: RFC 7591 dynamic client registration LIVE unauthenticated (POST /oauth2/register → 201 client_id+secret+chosen redirect_uris) d
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback yields 303 invalid_state (sta
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback returns 303 invalid_state obj
- LEARN: ACCEPTED OATH @ auth.sumup.com: modern dashboard client registered redirect confirmed live — https://me.sumup.com/api/sso/callback returns 303 invalid_state obj
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Official SumUp MCP (Cloudflare Worker, bearer JWKS from auth.sumup.com, Durable Object agent) LIVE and absent from inventory — s
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: RFC 7591 dynamic client registration LIVE unauthenticated (POST /oauth2/register → 201 client_id+secret+chosen redirect_uris) d
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed

## RANKED HYPOTHESES 2026-09-06 16:26:36 UTC

## RANKED HYPOTHESES 2026-09-06 18:44:57 UTC
- [82] auth.sam-app.ro: Staging auth server issues attacker-controlled audience JWTs via unauthenticated dynamic registration (from art/lead_bigpickle.txt)
- [80] api.sumup.com/authorize: Legacy OAuth authorize endpoint client_id oracle + redirect_uri divergence + wildcard CORS enables callback host enumeration and OAuth code theft (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: Register a client on auth.sam-app.ro with `grant_types:["authorization_code"]` + `response_types:["code"]` and complete a full authorization_code+PKCE fl
- NEXT(hypotheses-nemotron3.txt): RAG: fetch crt.sh for q=%.sumup.com (current + historical certificates), extract all unique subdomains, filter for OAuth-callback-shaped names (oauth, auth, sso
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: ACCEPTED AUTH @ auth.sumup.com: PAR (/oauth2/par) and device flow (/oauth2/device) routed (400 on POST) but require client authentication — "none" auth_method n
- LEARN: ACCEPTED AUTH @ me.sumup.com: Vercel-served asset; anonymous /api/* routes return 307 to OAuth flow — no debug endpoints or permissive CORS
- LEARN: REJECTED MISCONFIG @ me.sumup.com: No CORS misconfiguration on /api/sso/callback (307 redirect, no CORS headers)
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com: All versioned paths (/v0,/v0.1,/v1,/v2,/beta,/internal) return 404 unauthenticated — API fully gated at gateway
- LEARN: REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) confirmed but webhook/callback parameters not discovered passively; supply-chain risk unconfir
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Raw-curl ground truth re-confirms 302 error-flow (invalid_client vs invalid_request); earlier "404" logs are harness re
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5 shapes, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sum
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Official SumUp MCP (Cloudflare Worker, bearer JWKS from auth.sumup.com, Durable Object agent) LIVE and absent from inventory — s
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: RFC 7591 dynamic client registration LIVE unauthenticated (POST /oauth2/register → 201 client_id+secret+chosen redirect_uris) d
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed

## RANKED HYPOTHESES 2026-09-06 20:56:58 UTC
- [85] auth.sam-app.ro/oauth2/register: Unauthenticated dynamic client registration on staging auth enables self-issued OAuth clients with attacker-controlled redirect_uris and audience, minting valid JWTs with empty scope but controlled aud (from art/lead_nemotron3.txt)
- [52] auth-theta.sam-app.ro/oauth2/register: Theta canary OAuth registration config diverges from non-theta staging (scope/PKCE/allowlist relaxation) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://auth-theta.sam-app.ro/oauth2/register {"client_name":"sh-ana","redirect_uris":["https://me.sumup.com/api/sso/callback"],"token_endpoint_auth
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sam-app.ro/oauth2/register with Content-Type: application/json body {"redirect_uris":["https://evil.com/callback"],"grant_types":["auth
- LEARN: ACCEPTED OATH @ auth.sam-app.ro/oauth2/register: RFC 7591 dynamic client registration LIVE unauthenticated (POST → 201 client_id+secret+chosen redirect_uris) de
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sumup.com 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
- LEARN: ACCEPTED OATH @ auth.sam-app.ro/oauth2/register: RFC 7591 dynamic client registration LIVE unauthenticated (POST → 201 client_id+secret+chosen redirect_uris) de
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sumup.com 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth

## RANKED HYPOTHESES 2026-09-06 22:52:02 UTC
- [85] auth.sam-app.ro/oauth2/register: Unauthenticated dynamic client registration on staging auth enables self-issued OAuth clients with attacker-controlled redirect_uris and audience, minting valid JWTs with empty scope but controlled aud (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sam-app.ro/oauth2/register with Content-Type: application/json body {"redirect_uris":["https://evil.com/callback"],"grant_types":["auth
- LEARN: ACCEPTED OATH @ auth.sam-app.ro/oauth2/register: RFC 7591 dynamic client registration LIVE unauthenticated (POST → 201 client_id+secret+chosen redirect_uris) de
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sumup.com 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth

## RANKED HYPOTHESES 2026-09-07 00:55:00 UTC
- [85] auth.sam-app.ro/oauth2/register: Unauthenticated dynamic client registration on staging auth enables self-issued OAuth clients with attacker-controlled redirect_uris and audience, minting valid JWTs with empty scope but controlled aud (from art/lead_nemotron3.txt)
- [45] api.sumup.com/.well-known/oauth-protected-resource: Prod API gateway may publish RFC 9728 resource-server discovery diverging scope/audience model (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://auth.sam-app.ro/oauth2/register with Content-Type: application/json body {"redirect_uris":["https://evil.com/callback"],"grant_types":["auth
- LEARN: ACCEPTED OATH @ auth.sam-app.ro/oauth2/register: RFC 7591 dynamic client registration LIVE unauthenticated (POST → 201 client_id+secret+chosen redirect_uris) de
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sumup.com 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth

## RANKED HYPOTHESES 2026-09-07 05:57:20 UTC
- [75] api.sumup.com/.well-known/oauth-protected-resource: RFC 9728 resource server metadata divergence enables audience confusion / token relay across resource servers (from art/lead_nemotron3.txt)
- [38] auth.sam-app.ro/.well-known/jwks.json: Staging JWKS `loadtesting` kid indicates a separate signing key for load-testing tokens — if accessible, those tokens may have broader scope or bypass auth_method enforcement (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: All passive well-known/discovery surfaces on api.sumup.com (RFC 9728, openid-configuration, oauth-authorization-server) are now fully characterized. Stag
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://api.sumup.com/.well-known/oauth-protected-resource (already confirmed 200) → POST https://auth.sumup.com/oauth2/token with grant_type=client_
- LEARN: ACCEPTED BUSLOGIC @ api.sumup.com/.well-known/oauth-protected-resource: RFC 9728 resource-server metadata LIVE (200 JSON) declaring auth.sumup.com as sole autho
- LEARN: ACCEPTED BUSLOGIC @ api.sam-app.ro/.well-known/oauth-protected-resource: Staging variant LIVE (200 JSON) declaring auth.sam-app.ro; identical structure to prod;
- LEARN: ACCEPTED OTHER @ mcp.sumup.com/.well-known/mcp.json: 404 JSON-RPC response (not static file); MCP app-directory spec metadata not published by this worker.
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/.well-known/openid-configuration: 404 structured problem+json; gateway-handled, no auth-server discovery on API gateway (corr
- LEARN: ACCEPTED OTHER @ api.sumup.com/.well-known/oauth-authorization-server: 404 structured problem+json; same gateway handling.
- LEARN: ACCEPTED OTHER @ JWKS prod vs staging kid comparison: Prod 8 keys, staging 11 keys, ZERO kid overlap. Cross-env key isolation confirmed at JWKS kid level (expla
- LEARN: ACCEPTED OATH @ api.sumup.com/.well-known/oauth-protected-resource: RFC 9728 resource server metadata LIVE (200) declaring authorization_servers=["https://auth.
- LEARN: ACCEPTED OATH @ auth.sam-app.ro/oauth2/register: RFC 7591 dynamic client registration LIVE unauthenticated (POST → 201 client_id+secret+chosen redirect_uris) de
- LEARN: ACCEPTED OATH @ auth.sam-app.ro: Dynamic clients forced to EMPTY scope (requesting openid → invalid_scope "exceeds allowed scopes"), require PKCE code_challenge
- LEARN: ACCEPTED AUTH @ auth.sam-app.ro: Dynamic client registration yields real JWT access tokens via client_credentials (client_secret_post); JWT contains empty scp b
- LEARN: ACCEPTED AUTH @ api.sam-app.ro: Gateway validates JWTs (structured problem+json vs plain 404 with/without token) but empty scope blocks all resource access.
- LEARN: ACCEPTED OTHER @ mcp.sumup.com: Prod JWKS rejects staging tokens — cross-environment key isolation confirmed.
- LEARN: REJECTED MISCONFIG @ mcp.sumup.com: Wildcard CORS + Authorization allow-header is NOT token-stealing (bearer_methods_supported=["header"], no cookies/ambient cr
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Endpoint LIVE with client_id oracle (invalid_client vs invalid_request) and legacy redirect-set divergence — modern das
- LEARN: ACCEPTED MISCONFIG @ api.sumup.com/authorize: Wildcard CORS (access-control-allow-origin:*) + broad allow-methods + max-age + SameSite=None cookies on Domain=su
- LEARN: ACCEPTED OATH @ auth.sumup.com: Modern dashboard client redirect confirmed live on modern auth server — https://me.sumup.com/api/sso/callback returns 302→login 
- LEARN: REJECTED OATH @ api.sumup.com/authorize: crt.sh-derived callback candidates (app-auth×5, checkout, pay, collect, ze-dashboard, gateway, read-api, api.sumup.com 
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: sumup-ios-sdk and unknown IDs → invalid_client ("does not exist") on legacy gateway — legacy SDK clients not registered
- LEARN: ACCEPTED AUTH @ checkout.sumup.com: New Vercel asset (76.76.21.61); uniform 403 text/plain on all paths — edge-gated same as me.sumup.com; no anonymous surface.
- LEARN: REJECTED MISCONFIG @ auth.sumup.com: No dynamic registration endpoint in prod — register route absent (404 GET/POST/OPTIONS), not exposed.
- LEARN: REJECTED AUTH @ auth.sam-app.ro: token_endpoint_auth_method enforcement not implemented — server stores preference but does not enforce at token endpoint.
- LEARN: ACCEPTED OATH @ api.sumup.com/authorize: Redirect-allowlist divergence + wildcard CORS is the api-gateway template (identical on api.sam-app.ro), distinct from 
- LEARN: REJECTED MISCONFIG @ tap-to-pay-sdk.fleet.live.sumup.net: 401 Cloudflare Maven host + Fleet CD naming = infra/banner class.
- LEARN: ACCEPTED OATH @ sam-app.ro: Staging stack (mcp/mcp-theta/api/api-theta/auth/auth-theta) publicly reachable; replicates prod gates byte-for-byte (/mcp 401, /auth
