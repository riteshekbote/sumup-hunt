# Validated findings (running count 0)

- 1 lead(s) marked VALID at 2026-09-04 06:02:58 UTC
  - | Q4 Provable non-invasively? | **NO** — all unauthenticated paths return 404; requires valid merchant OAuth token to test. Probe results confirm 404 on every enumerated path (v0/v0.1/v1/v2/merchants/

- 7 lead(s) marked VALID at 2026-09-06 21:30:02 UTC
  - **Verdict: VALID**
  - | Impact | Attacker-controlled OAuth clients on staging; mintable JWTs (empty scope blocks resources but token is valid at gateway). Staging environments often lack prod hardening. |
  - **Verdict: VALID**
  - **Verdict: VALID**
  - | 1 | `auth.sam-app.ro` unauthenticated dynamic client registration | **VALID** | 7.5 |
  - | 2 | `api.sumup.com/authorize` client-ID oracle + wildcard CORS | **VALID** | 5.3 |
  - | 3 | `mcp.sumup.com` wildcard CORS on Bearer-protected endpoint | **VALID** | 5.3 |
