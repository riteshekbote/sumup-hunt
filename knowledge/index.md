# Knowledge Base (seed)
- 2026-09-03 ACCEPTED BUSLOGIC @ api.sumup.com: API 404 root is common for versioned REST APIs; enumeration is standard recon.
- 2026-09-03 ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling.
- 2026-09-03 ACCEPTED IDOR @ api.sumup.com: Versioned payment APIs are high-value; 404 on root is standard pattern — enumerate versions passively first.
- 2026-09-03 ACCEPTED OAUTH @ auth.sumup.com: /flows/login path confirms OAuth/OIDC flow; redirect_uri/state flaws are high-impact and testable passively via HEAD.
- 2026-09-03 REJECTED SSRF @ portal.sumup.com: Third-party CNAME (iriscrm.com) requires scope confirmation; webhook SSRF needs parameter discovery (active); parked until in-scope verified.
- 2026-09-03 ACCEPTED AUTH @ admin.sumup.com: 403 on nginx/ELB stack is consistent with auth-gated internal tooling; header-based auth misconfigurations are testable passively.
- 2026-09-03 ACCEPTED OATH @ auth.sumup.com: OIDC/OAuth discovery documents are public and enumerate live endpoints + full scope/resource model; /oauth2/auth is a live interactive flow. High-value deep-hunt surface.
- 2026-09-03 ACCEPTED IDOR @ api.sumup.com: scope catalog names the API resource model, but all unauthenticated paths 404 — BOLA test requires a merchant OAuth token (AUTH_HELPED), not passive.
- 2026-09-03 REJECTED MISCONFIG @ auth.sumup.com: x-envoy-decorator-operation leaks k8s service name identity.svc.cluster.local — header/banner leak is explicit out-of-scope class.
