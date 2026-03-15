# Fake-Store-API-testing-documentation-example
Example of test documentation for a portfolio. The goal of the test is a Fake API Store to demonstrate the ability to use Postman app, including: Collections, Environments, Variables, Pre-request Scripts, Test Scripts, Assertions, Collection Runners, Data Driven Testing, Mock Servers, and Monitoring.

## Overview
This collection contains requests for the **Fake Store API** (a sample e-commerce API) organized by resource.

Use it to:
- Authenticate (get a JWT-like token)
- Explore **Products** and **Carts** endpoints
- Run requests individually or run the whole collection as a quick regression check

---

## Base URL & Environments
All requests use:

- `{{baseUrl}}`

The active environment is **Production**. Ensure `baseUrl` is set there.

### Required/commonly used environment variables
| Variable | Purpose |
|---|---|
| `baseUrl` | API host, e.g. `https://fakestoreapi.com` |
| `accessToken` | Populated automatically after successful login (see below) |

---

## Authentication workflow (Login → env token)
1. Send **Login** (`POST {{baseUrl}}/auth/login`).
2. On success, the request’s **Tests** script reads `token` from the JSON response and stores it into the active environment variable:
   - `accessToken`

After that, requests can use the token via header:
```http
Authorization: Bearer {{accessToken}}
```

Notes:
- This collection currently stores the token at the **environment** scope (`pm.environment.set`).
- If you switch environments (Dev/Staging/Mock), you’ll need to login again in that environment.

---

## How to run
### Run a single request
Open a request and click **Send**.

### Run the whole collection
Use **Run collection** from the collection menu or Collection Runner.

Tips:
- Start by running **Auth → Login** first so `{{accessToken}}` is available.
- If a request uses a path variable like `:id`, set it in the request URL path variable editor or update it inline.

---

## Common troubleshooting
- **`Could not get any response` / DNS / SSL errors**: verify `{{baseUrl}}` is correct and reachable.
- **401/403 responses**: run **Login** again and confirm `accessToken` is set in the active environment.
- **Invalid JSON parsing / missing `token`**: inspect the Login response body; the test script expects a JSON response with a `token` field.
- **Path variables not set (e.g., `:id`)**: set the `id` value in the request before sending.

---

## Key requests
- [Login](request/42626421-7b3a2436-1183-4a0c-9541-62a58fce9492)
- [Get Single Product](request/42626421-e9e3526a-ab3c-4637-ba8d-dcf5b438ddae)
