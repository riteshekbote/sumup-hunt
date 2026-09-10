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

- 3 lead(s) marked VALID at 2026-09-09 15:21:03 UTC
  - **Verdict: VALID**
  - | Q3 | Real security impact? | Low — error taxonomy (`invalid_client` vs `invalid_request`) is a standard OAuth error pattern; wildcard CORS on an authorize endpoint that only redirects to error pages
  - | 1 | help.sumup.com Contentful Preview Token Leak | **VALID** | 5.3 (Medium) | **FILE REPORT** |

- 3 lead(s) marked VALID at 2026-09-10 16:19:46 UTC
  - **VERDICT: VALID**
  - **VERDICT: HOLD** — Missing webhook HMAC verification in Vendure plugin is a valid code issue, but responsibility may lie with integrators who deploy the plugin. Needs clarification on whether this is
  - | 1 | Contentful Preview API Token Leak (help.sumup.com) | **VALID** | 5.3 |

- 4 lead(s) marked VALID at 2026-09-10 19:11:04 UTC
  - | Q7 Reasonable triager | YES — client_id enumeration via error differentiation is a valid finding |
  - **Verdict: VALID** — Client ID oracle on legacy OAuth endpoint. Read-only proof: GET requests to `/authorize` with different client_ids show error differentiation. Impact: Medium (information disclosu
  - | Q4 Provable | NO — requires valid merchant OAuth token (AUTH_HELPED) |
  - | VALID | 1 | #6: Legacy OAuth client_id oracle on api.sumup.com/authorize |
