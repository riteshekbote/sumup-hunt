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
