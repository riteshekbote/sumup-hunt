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

- 5 lead(s) marked VALID at 2026-09-10 21:43:51 UTC
  - | 1 | `auth.sam-app.ro` unauthenticated RFC 7591 dynamic client registration → mintable JWTs | staging auth | AUTH | 7.5 | VALID |
  - | 2 | `api.sumup.com/authorize` client_id oracle (invalid_client vs invalid_request) | api gateway | INFO | 5.3 | VALID |
  - | 3 | `mcp.sumup.com` wildcard CORS (`*`) on Bearer-protected MCP endpoint | MCP server | MISCONFIG | 5.3 | VALID |
  - | 4 | `help.sumup.com` Contentful Preview API token leak → 1,082 draft entries readable | help center | MISCONFIG | 5.3 | VALID |
  - | 9 | API BOLA via dashboard-client scopes | Requires valid merchant OAuth token |

- 2 lead(s) marked VALID at 2026-09-12 00:39:58 UTC
  - **Verdict: VALID** (finding now closed — token rotated 2026-09-11)
  - | 1 | Contentful Preview Token Leak (help.sumup.com) | **VALID** (closed) | Report if still live at time of submission; token rotated |

- 5 lead(s) marked VALID at 2026-09-16 20:16:23 UTC
  - | Q7 | Reasonable triager accept? | YES — staging exposure with weaker security posture is valid |
  - **Verdict: VALID** — Publicly reachable staging infrastructure with potential weaker controls.
  - **Verdict: VALID**
  - | 2 | Internal Staging Domains (sam-app.ro) | **VALID** | 5.3 |
  - | 3 | Wildcard CORS on MCP Server | **VALID** | 5.3 |

- 15 lead(s) marked VALID at 2026-09-17 12:13:01 UTC
  - | Q5 Novel/unreported? | PARTIALLY — previously marked VALID 2026-09-09, token rotated 2026-09-11. **Need to re-verify if token is still live**. If token was rotated and new token not leaked, this is 
  - | Q3 Real impact? | YES — attacker can register arbitrary OAuth clients, mint real JWT access tokens via client_credentials grant. JWTs are valid at api.sam-app.ro gateway (confirmed by structured err
  - | Q5 Novel/unreported? | YES — not in valid-bugs.md as filed report; staging-only but staging often leads to prod pivot |
  - | Q7 Reasonable triager? | YES — unauthenticated client registration on staging auth server with mintable tokens is a valid finding |
  - **Verdict: VALID**
  - | Q3 Real impact? | LOW — error taxonomy (`invalid_client` vs `invalid_request`) is standard OAuth error behavior. Wildcard CORS exists but the endpoint only redirects to error pages; no code/token le
  - | Q5 Novel/unreported? | ALREADY REPORTED — marked VALID in valid-bugs.md (2026-09-10) |
  - | Q3 Real impact? | LOW — wildcard CORS + `Authorization: Bearer` header-only auth. No cookies ambient creds. Hardening-only config. Already marked VALID in valid-bugs.md. |
  - | Q2 Attacker reachable? | Partially — all unauthenticated paths return 404. Requires valid merchant OAuth token. |
  - | Q4 Provable non-invasively? | **NO** — requires valid merchant OAuth token with dashboard scopes. All unauthenticated paths 404. Cannot test passively. |
  - | Q2 Attacker reachable? | Partially — OIDC discovery is public, but actual token flow requires valid client registration |
  - **Verdict: HOLD** — Interesting theoretical vector but insufficient proof. redirect_uri strict allowlist + PAR unrouted + no public client registration = mitigated. Could be VALID if a public client r
  - **Verdict: HOLD** — Valid code issue (private key material in log output) but in a development tool, not production infrastructure. May be accepted as informational. Needs scoping clarification.
  - | 1 | Contentful Preview Token Leak (help.sumup.com) | **HOLD** | 5.3 | Re-verify token liveness; if live → VALID, FILE REPORT |
  - | 2 | auth.sam-app.ro unauthenticated client registration → JWTs | **VALID** | 7.5 | FILE REPORT |

- 13 lead(s) marked VALID at 2026-09-18 01:28:35 UTC
  - | Q3 | Real impact? | **YES** — attacker-controlled OAuth clients mint real JWT access tokens via `client_credentials` grant. Tokens valid at `api.sam-app.ro` gateway (structured error response). Stag
  - | Q5 | Novel/unreported? | **YES** — not in `valid-bugs.md` as filed report. |
  - | Q7 | Reasonable triager? | **YES** — unauthenticated client registration on staging auth server with mintable tokens is a valid finding. |
  - **Verdict: VALID**
  - | Q5 | Novel/unreported? | **ALREADY REPORTED** — marked VALID in `valid-bugs.md` (2026-09-10). |
  - | Q7 | Reasonable triager? | **YES, but low bounty** — valid information disclosure, but standard OAuth errors. Already filed. |
  - | Q5 | Novel/unreported? | **ALREADY REPORTED** — marked VALID in `valid-bugs.md`. |
  - | Q5 | Novel/unreported? | **YES at time of discovery** — marked VALID 2026-09-09. |
  - | Q2 | Attacker reachable? | **NO** — all unauthenticated paths return 404. Requires valid merchant OAuth token. |
  - | Q4 | Provable non-invasively? | **NO** — requires valid merchant OAuth token with dashboard scopes to test ID-swapping. AUTH_HELPED. |
  - | Q3 | Real impact? | **NO** — OPTIONS returns 204 (no content), wildcard CORS on a token endpoint that rejects all POST requests with `invalid_client` (no valid client credentials known). The `x-envo
  - | Q5 | Novel/unreported? | **Possibly novel** — not in `valid-bugs.md`. |
  - | 1 | `auth.sam-app.ro` unauth dynamic client registration → JWTs | **VALID** | 7.5 | FILE REPORT |
