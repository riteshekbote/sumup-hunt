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
