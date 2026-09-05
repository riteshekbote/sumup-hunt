
## 2026-09-02 21:55:21 UTC


## 2026-09-03 00:07:18 UTC


## 2026-09-03 04:11:41 UTC


## 2026-09-03 09:02:20 UTC


## 2026-09-03 13:32:16 UTC


## 2026-09-03 17:26:05 UTC
https://api.sumup.com/v1/ -> HTTP 404
https://api.sumup.com/v1/merchants -> HTTP 404
https://api.sumup.com/v1/payments -> HTTP 404
https://api.sumup.com/swagger.json -> HTTP 404
https://api.sumup.com/openapi.json -> HTTP 404
https://api.sumup.com/api-docs -> HTTP 404
https://api.sumup.com/health -> HTTP 404
https://api.sumup.com/v2/ -> HTTP 404
https://api.sumup.com/graphql -> HTTP 404
https://admin.sumup.com/admin -> HTTP 403
https://admin.sumup.com/login -> HTTP 403
https://admin.sumup.com/health -> HTTP 403

## 2026-09-03 19:59:19 UTC
https://api.sumup.com/v1/ -> HTTP 404
https://auth.sumup.com/flows/login -> HTTP 403
https://admin.sumup.com/ -> HTTP 403
https://api.sumup.com/v1/merchants -> HTTP 404
https://api.sumup.com/v1/payments -> HTTP 404
https://api.sumup.com/swagger.json -> HTTP 404
https://api.sumup.com/openapi.json -> HTTP 404
https://api.sumup.com/api-docs -> HTTP 404
https://api.sumup.com/health -> HTTP 404
https://api.sumup.com/v2/ -> HTTP 404
https://api.sumup.com/graphql -> HTTP 404
https://admin.sumup.com/admin -> HTTP 403

## 2026-09-03 22:32:38 UTC
https://api.sumup.com/v1/ -> HTTP 404
https://auth.sumup.com/flows/login -> HTTP 403
https://admin.sumup.com/ -> HTTP 403
https://api.sumup.com/v1/merchants -> HTTP 404
https://api.sumup.com/v1/payments -> HTTP 404
https://api.sumup.com/swagger.json -> HTTP 404
https://api.sumup.com/openapi.json -> HTTP 404
https://api.sumup.com/api-docs -> HTTP 404
https://api.sumup.com/health -> HTTP 404
https://api.sumup.com/v2/ -> HTTP 404
https://api.sumup.com/graphql -> HTTP 404
https://admin.sumup.com/admin -> HTTP 403

## 2026-09-04 00:36:25 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://auth.sumup.com/oauth2/token -> HTTP 405
https://app.sumup.com.attacker.com -> ERR <urlopen error [SSL: TLSV1_ALERT_INTERNAL_ERROR] t
https://auth.sumup.com/oauth2/auth?client_id=test&redirect_uri=https://evil.com&response_type=code&scope=merchants.read -> HTTP 405
https://sumup.com.evil.com -> ERR <urlopen error [Errno -2] Name or service not know
https://app.sumup.com/../evil.com -> ERR <urlopen error [Errno -2] Name or service not know
https://sub.sumup.com -> ERR <urlopen error [Errno -2] Name or service not know
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404
https://api.sumup.com/v1/ -> HTTP 404
https://auth.sumup.com/flows/login -> HTTP 403
https://admin.sumup.com/ -> HTTP 403
https://me.sumup.com/api/sso/callback` -> HTTP 404

## 2026-09-04 05:13:02 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://auth.sumup.com/oauth2/token -> HTTP 405
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404

## 2026-09-04 09:55:25 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://auth.sumup.com/oauth2/token -> HTTP 405
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404

## 2026-09-04 14:14:42 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://auth.sumup.com/oauth2/token -> HTTP 405
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJGuAC2I2g2sZMk&code_challenge_method=S256&scope=openid+accounting.read -> HTTP 403
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJGuAC2I2g2sZMk&code_challenge_method=S256&scope=openid+accounting.read` -> HTTP 403

## 2026-09-04 17:50:25 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://auth.sumup.com/oauth2/token -> HTTP 405
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJGuAC2I2g2sZMk&code_challenge_method=S256&scope=openid+accounting.read -> HTTP 403
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJGuAC2I2g2sZMk&code_challenge_method=S256&scope=openid+accounting.read` -> HTTP 403

## 2026-09-04 20:03:04 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 -> HTTP 403
https://auth.sumup.com/oauth2/token -> HTTP 405
https://api.sumup.com/v1/merchants/{other_merchant_id -> HTTP 404
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256` -> HTTP 403
https://api.sumup.com/authorize?client_id={known -> HTTP 404
https://api.sumup.com/authorize?client_id={l -> HTTP 404
https://developer.sumup.com/sitemap.xml -> HTTP 404

## 2026-09-04 22:21:10 UTC
https://api.sumup.com/authorize?client_id={legacy_client_id -> HTTP 404
https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=https://attacker.com/callback&response_type=code&scope=classic -> HTTP 404
https://auth.sumup.com/oauth2/par -> HTTP 404
https://dashboard.sumup.com/callback&response_type=code&code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM&code_challenge_method=S256 -> HTTP 403
https://auth.sumup.com/oauth2/token -> HTTP 405
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri=https://example.com/callback&response_type=code&scope=classic -> HTTP 404

## 2026-09-05 00:24:53 UTC
https://auth.sumup.com/oauth2/auth?client_id=dashboard&request=eyJhbGciOiJub25lIn0.eyJyZXNwb25zZV90eXBlIjoiY29kZSIsImNsaWVudF9pZCI6ImRhc2hib2FyZCIsInJlZGlyZWN0X3VyaSI6Imh0dHBzOi8vZGFzaGJvYXJkLnN1bXVwLmNvbS9jYWxsYmFjayIsInNjb3BlIjoiY2xhc3NpYyJ9.&response_type=code&redirect_uri=https://dashboard.sumup -> HTTP 405
https://me.sumup.com/api/sso/callback?error=test -> HTTP 403
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/_vercel/insights -> HTTP 404
https://portal.sumup.com/ -> 200 len=?

## 2026-09-05 04:44:52 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic -> HTTP 403
https://portal.sumup.com/ -> 200 len=?
https://me.sumup.com/api/sso/callback?state=eyJhbGciOiJub25lIn0.eyJhcHBTdGF0ZSI6e30nfQ -> HTTP 403
https://me.sumup.com/api/sso/callback?state=malformed -> HTTP 403
https://me.sumup.com/api/sso/callback -> HTTP 403

## 2026-09-05 08:45:31 UTC
https://auth.sumup.com/oauth2/par -> HTTP 404
https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic -> HTTP 403
https://auth.sumup.com/oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri: -> HTTP 404
https://portal.sumup.com/ -> 200 len=?
https://me.sumup.com/api/sso/callback?state=eyJhbGciOiJub25lIn0.eyJhcHBTdGF0ZSI6e30nfQ -> HTTP 403
https://me.sumup.com/api/sso/callback?state=malformed -> HTTP 403
https://me.sumup.com/api/sso/callback -> HTTP 403
https://me.sumup.com/api/sso/callback` -> HTTP 404

## 2026-09-05 12:10:21 UTC
https://me.sumup.com/api/sso/callback -> HTTP 403
https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://me.sumup.com/api/sso/callback&response_type=code&scope=classic&state=test1234 -> HTTP 404
https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test1234 -> HTTP 404
https://api.sumup.com/authorize?client_id=sumup-ios-sdk&redirect_uri= -> HTTP 404
https://auth.sumup.com/oauth2/par -> HTTP 404
https://dashboard.sumup.com/callback&request_uri=https://attacker.com/malicious.json&response_type=code&scope=classic -> HTTP 403
https://auth.sumup.com/oauth2/auth?client_id=dashboard&request_uri=urn:ietf:params:oauth:request_uri: -> HTTP 404
https://portal.sumup.com/ -> 200 len=?
https://api.sumup.com/authorize?client_id=dashboard&redirect_uri=https://dashboard.sumup.com/callback&response_type=code&scope=classic&state=test12345678 -> HTTP 404
