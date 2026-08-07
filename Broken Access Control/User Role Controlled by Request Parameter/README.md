# User Role Controlled by Request Parameter

## 📌 Lab Information

| Item              | Details                                   |
| ----------------- | ----------------------------------------- |
| **Lab Name**      | User role controlled by request parameter |
| **Platform**      | PortSwigger Web Security Academy          |
| **Difficulty**    | Apprentice                                |
| **Vulnerability** | Broken Access Control                     |
| **Attack Type**   | Vertical Privilege Escalation             |
| **Tool Used**     | Burp Suite                                |
| **Status**        | ✅ Solved                                  |

---

## 🎯 Objective

The objective of this lab is to access the administrator panel and delete the user `carlos`.

The application uses a client-controlled cookie named `Admin` to determine whether the logged-in user has administrator privileges.

Valid credentials provided by the lab:

```text
Username: wiener
Password: peter
```

---

## 🔍 Vulnerability Description

The application determines the user's role based on the value of the `Admin` cookie.

After logging in as the normal user `wiener`, the server sets:

```http
Set-Cookie: Admin=false
```

Because this value is controlled by the client, it can be modified.

By changing:

```text
Admin=false
```

to:

```text
Admin=true
```

the application incorrectly treats the normal user as an administrator.

This results in **Broken Access Control** and allows **Vertical Privilege Escalation**.

---

# 🛠️ Tools Used

* Burp Suite
* Web Browser
* PortSwigger Web Security Academy

---

# 🧪 Lab Walkthrough

## Step 1 – Login

Open the lab and navigate to the login page.

Use the provided credentials:

```text
Username: wiener
Password: peter
```

Burp Suite was configured to intercept the HTTP traffic.

---

## Step 2 – Capture the Login Response

After submitting the login form, the response was intercepted using:

```text
Burp Suite
    ↓
Proxy
    ↓
Intercept
```

The response contained the following cookie:

```http
Set-Cookie: Admin=false
```

This was an important finding because the application appeared to use this cookie to determine the user's role.

---

## Step 3 – Identify the Access Control Issue

The application was trusting the client-controlled cookie:

```text
Admin=false
```

The expected behavior was:

```text
Admin=false → Normal User
Admin=true  → Administrator
```

Since the cookie was controlled by the browser, it could be modified.

---

## Step 4 – Modify the Cookie

The following value:

```http
Set-Cookie: Admin=false
```

was changed to:

```http
Set-Cookie: Admin=true
```

The modified response was then forwarded through Burp Suite.

The browser subsequently stored:

```text
Admin=true
```

---

## Step 5 – Access the Admin Panel

The administrator endpoint was:

```text
/admin
```

The request now contained:

```http
GET /admin HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: Admin=true
```

Because the application trusted the modified cookie, access to the administrator panel was granted.

---

## Step 6 – Delete `carlos`

Inside the administrator panel, the user:

```text
carlos
```

was identified.

The **Delete** functionality was used to remove the user.

The corresponding request was sent through Burp Suite and forwarded to the server.

The lab was successfully completed.

---

# 🔄 Attack Flow

```text
             Login
               │
               ▼
        wiener : peter
               │
               ▼
       Server Response
               │
               ▼
    Set-Cookie: Admin=false
               │
               ▼
       Intercept using
          Burp Suite
               │
               ▼
     Admin=false → true
               │
               ▼
          /admin
               │
               ▼
      Admin Panel Access
               │
               ▼
       Delete "carlos"
               │
               ▼
          Lab Solved
```

---

# 🔎 HTTP Request/Response Analysis

### Original Response

```http
HTTP/1.1 302 Found
Location: /
Set-Cookie: Admin=false
```

### Modified Cookie

```http
Admin=true
```

### Request to Admin Panel

```http
GET /admin HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: Admin=true
```

The server incorrectly trusted the `Admin=true` value and granted administrator privileges.

---

# ⚠️ Vulnerability Impact

An attacker who can modify the client-side role indicator may be able to:

* Access administrator functionality
* View administrative information
* Modify application data
* Delete users
* Perform unauthorized administrative actions
* Escalate from a normal user to an administrator

Depending on the application's functionality, this could result in **high-impact unauthorized access**.

---

# 🧠 Root Cause

The root cause is that the application relies on a **client-controlled cookie** to make an authorization decision.

Insecure logic:

```text
if Admin == "true":
    allow_admin_access()
```

The attacker can modify:

```text
Admin=false
```

to:

```text
Admin=true
```

Therefore, the authorization mechanism can be bypassed.

---

# ✅ Recommended Remediation

The application should never trust client-controlled values for authorization decisions.

Instead, authorization should be determined server-side using a secure session.

### Secure approach

```text
User Login
    ↓
Server authenticates user
    ↓
Server creates session
    ↓
Session ID stored in browser
    ↓
Server retrieves user information
    ↓
Server checks role server-side
    ↓
Allow / Deny access
```

For example:

```text
Session ID
    ↓
User = wiener
    ↓
Role = normal_user
    ↓
/admin
    ↓
403 Forbidden
```

Even if an attacker modifies a cookie, the server should still determine the user's actual role from trusted server-side data.

Additional recommendations:

* Implement server-side authorization checks.
* Do not store privilege information in modifiable client-side parameters.
* Apply role-based access control (RBAC).
* Return `403 Forbidden` for unauthorized administrative requests.
* Validate authorization on every sensitive endpoint.
* Do not rely on hidden UI elements to enforce security.

---

# 📊 Security Classification

| Category           | Classification                                      |
| ------------------ | --------------------------------------------------- |
| **OWASP**          | Broken Access Control                               |
| **CWE**            | CWE-639 / CWE-862 related access-control weaknesses |
| **Attack**         | Privilege Escalation                                |
| **Privilege Type** | Vertical                                            |
| **Root Cause**     | Client-controlled authorization parameter           |
| **Impact**         | Unauthorized administrative access                  |

---

# 💡 Key Learning

This lab demonstrates an important security principle:

> **Never trust client-controlled data when making authorization decisions.**

Authentication establishes **who the user is**.

Authorization determines **what the user is allowed to do**.

In this lab:

```text
Authentication
      ↓
wiener successfully logged in
```

But authorization was vulnerable:

```text
Normal User
      ↓
Admin=true
      ↓
Administrator access
```

Therefore, the vulnerability resulted in **vertical privilege escalation through broken access control**.

---

## 🏁 Result

**Lab successfully solved. ✅**

The `Admin` cookie was manipulated from:

```text
Admin=false
```

to:

```text
Admin=true
```

This allowed access to `/admin` and enabled deletion of the `carlos` user.

---

## 📚 Reference

PortSwigger Web Security Academy – Access Control Labs

https://portswigger.net/web-security/access-control

<img width="1306" height="801" alt="image" src="https://github.com/user-attachments/assets/fb800c06-488b-4cb4-be8f-22b504acfd56" />
<img width="1888" height="954" alt="image" src="https://github.com/user-attachments/assets/ff7ad046-6e0b-442e-9f94-5dbddbc31076" />
<img width="1390" height="923" alt="image" src="https://github.com/user-attachments/assets/8ca40976-20c9-440d-ba55-874370185aaa" />
<img width="1346" height="747" alt="image" src="https://github.com/user-attachments/assets/2d10d2f7-d404-4ed9-b090-44cbd69b4d41" />
<img width="1870" height="977" alt="image" src="https://github.com/user-attachments/assets/78ad3cc1-eec6-4d4a-bc4f-fe21f8e36a0e" />
<img width="1906" height="858" alt="image" src="https://github.com/user-attachments/assets/2ac1676e-b310-4c2a-8a69-759025ecd2c5" />

<img width="1301" height="937" alt="image" src="https://github.com/user-attachments/assets/a76fa87f-2e5f-4c5b-90d6-3c41ef348d10" />

