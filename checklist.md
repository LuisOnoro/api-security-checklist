# API Security Checklist (OWASP API Security Top 10, 2023)
 
**API / service:** ______  **Version:** ______  **Date:** ______  **Reviewer:** ______
 
> Test only systems you own or have explicit authorization to test. Examples use `https://api.example.com`.
> Status per item: OK / FAIL / N/A. Attach evidence and open a ticket for each FAIL.
 
---
 
## API1: Broken Object Level Authorization (BOLA)
The user accesses objects that don't belong to them by changing an identifier.
 
**What to check**
- [ ] Every endpoint that receives an object ID verifies that the authenticated user has permission over that object.
- [ ] The system doesn't rely on IDs being hard to guess.
**Test example**
```bash
# Using user A's token, request a resource belonging to user B
curl -H "Authorization: Bearer <TOKEN_A>" https://api.example.com/v1/orders/<B_ID>
```
Expected result: `403` or `404`. If it returns B's data, it's a FAIL.
 
**Evidence:** request, response, and the two users involved.
**Mitigation:** server-side ownership check on every object access.
 
---
 
## API2: Broken Authentication
Flaws in the authentication process that allow impersonating another user.
 
**What to check**
- [ ] Protected endpoints reject requests with no token, an expired token, or a tampered token.
- [ ] Login and password recovery have attempt limits.
- [ ] Tokens (e.g. JWT) are validated: signature, expiration, and algorithm.
- [ ] No credentials or tokens are passed in the URL.
**Test example**
```bash
curl -i https://api.example.com/v1/profile                                  # no token
curl -i -H "Authorization: Bearer <EXPIRED_TOKEN>" https://api.example.com/v1/profile
```
Expected result: `401` in both cases.
 
**Mitigation:** server-side token validation, attempt limiting, short expiration, and rotation.
 
---
 
## API3: Broken Object Property Level Authorization
A sensitive object property is exposed or can be modified when the user shouldn't see or change it.
 
**What to check**
- [ ] Responses don't include sensitive fields (hashes, internal roles, other users' data).
- [ ] A user can't modify protected fields (`role`, `isAdmin`, `balance`) by sending them in the request body.
**Test example**
```bash
curl -X PATCH -H "Authorization: Bearer <TOKEN>" -H "Content-Type: application/json" \
  -d '{"name":"Test","role":"admin"}' https://api.example.com/v1/users/me
```
Expected result: the `role` field is ignored or the request is rejected.
 
**Mitigation:** allowlists of fields on both input and output.
 
---
 
## API4: Unrestricted Resource Consumption
The API doesn't limit resource consumption, allowing abuse or denial of service.
 
**What to check**
- [ ] There are rate limits per user or IP.
- [ ] Payload size, pagination (`limit` cap), and execution time are limited.
- [ ] Costly operations (uploads, exports, SMS or email sending) have limits.
**Test example**
```bash
curl "https://api.example.com/v1/items?limit=1000000"
```
Expected result: a cap is applied or an error is returned, not the entire database.
 
**Mitigation:** rate limiting, quotas, size and pagination limits.
 
---
 
## API5: Broken Function Level Authorization
A user accesses functions meant for another role, such as admin functions.
 
**What to check**
- [ ] Administrative endpoints enforce the correct role server-side.
- [ ] Changing the HTTP method (GET to DELETE) doesn't bypass access control.
**Test example**
```bash
curl -i -X DELETE -H "Authorization: Bearer <REGULAR_USER_TOKEN>" \
  https://api.example.com/v1/admin/users/123
```
Expected result: `403`.
 
**Mitigation:** centralized role-based access control, deny by default.
 
---
 
## API6: Unrestricted Access to Sensitive Business Flows
A legitimate business flow is abused through automation (mass purchases, bookings, mass registrations).
 
**What to check**
- [ ] Sensitive flows (registration, purchase, booking, coupons) have anti-automation controls.
- [ ] Anomalous usage patterns are detected and limited.
**Test example:** repeat a sensitive flow via automation in a test environment and check whether any control stops it.
 
**Mitigation:** per-user limits, additional verification at critical steps, monitoring.
 
---
 
## API7: Server Side Request Forgery (SSRF)
The API makes a request to a URL controlled by the user, potentially reaching the internal network.
 
**What to check**
- [ ] Parameters that accept a URL are validated against an allowlist of destinations.
- [ ] Internal addresses or cloud metadata endpoints can't be reached.
**Test example**
```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"url":"http://127.0.0.1:8080/"}' https://api.example.com/v1/import
```
Expected result: the request is rejected.
 
**Mitigation:** allowlist of destinations, blocking of internal ranges, don't return the raw response.
 
---
 
## API8: Security Misconfiguration
Insecure configuration of the server, the API, or its dependencies.
 
**What to check**
- [ ] HTTPS only, with up-to-date TLS.
- [ ] Appropriate security headers and restrictive CORS.
- [ ] Errors don't reveal stack traces, versions, or internal paths.
- [ ] Unnecessary HTTP methods are disabled.
- [ ] No default credentials or debug mode in production.
**Test example**
```bash
curl -i -X OPTIONS https://api.example.com/v1/items
curl -i https://api.example.com/v1/does-not-exist        # check the error message
```
 
**Mitigation:** hardened, reviewed configuration and separated environments.
 
---
 
## API9: Improper Inventory Management
Forgotten APIs, old versions, or environments exposed without control.
 
**What to check**
- [ ] An inventory of APIs, versions, and environments exists.
- [ ] Old versions and test endpoints are retired or protected.
- [ ] API documentation (Swagger/OpenAPI) isn't exposed without control in production.
**Test example:** check whether `/v1/`, `/v2/`, `/swagger`, `/api-docs` are still unexpectedly accessible.
 
**Mitigation:** up-to-date inventory, planned retirement of old versions, periodic reviews.
 
---
 
## API10: Unsafe Consumption of APIs
The API trusts data received from other APIs or services without validating it.
 
**What to check**
- [ ] Data coming from third parties is validated the same way as user input.
- [ ] Connections to third parties use TLS and handle redirects and errors safely.
**Test example (code or design review):** locate where third-party responses are consumed and verify their format and content are validated before being used or stored.
 
**Mitigation:** strict validation, timeouts, and safe error handling.
 
---
 
## Review summary
 
| Item | Status | Endpoint(s) | Ticket |
|---|---|---|---|
| API1 BOLA | | | |
| API2 Authentication | | | |
| API3 Object property auth | | | |
| API4 Resource consumption | | | |
| API5 Function level auth | | | |
| API6 Business flows | | | |
| API7 SSRF | | | |
| API8 Misconfiguration | | | |
| API9 Inventory | | | |
| API10 API consumption | | | |
 
