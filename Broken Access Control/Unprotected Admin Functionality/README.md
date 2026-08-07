# Unprotected Admin Functionality

## 📌 Lab Information

* **Platform:** PortSwigger Web Security Academy
* **Lab:** Unprotected Admin Functionality
* **Difficulty:** Apprentice
* **Vulnerability:** Broken Access Control / Unprotected Admin Functionality
* **Status:** ✅ Solved

---

## 🎯 Objective

The objective of this lab is to identify an unprotected administrator panel and delete the user **`carlos`**.

---

## 🔍 Vulnerability Description

The application contains an administrator panel that does not properly enforce authorization.

An unauthenticated user can discover the admin panel URL and access administrative functionality without having administrator privileges.

This is an example of **Broken Access Control**.

---

## 🧪 Steps to Solve

### Step 1 – Access `robots.txt`

First, append `/robots.txt` to the lab URL:

```text
https://YOUR-LAB-ID.web-security-academy.net/robots.txt
```

The `robots.txt` file contains a `Disallow` entry revealing the administrator panel:

```text
User-agent: *
Disallow: /administrator-panel
```

This reveals the hidden administrative endpoint.

---

### Step 2 – Access the Admin Panel

Replace:

```text
/robots.txt
```

with:

```text
/administrator-panel
```

Example:

```text
https://YOUR-LAB-ID.web-security-academy.net/administrator-panel
```

The administrator panel is accessible without administrator authentication.

---

### Step 3 – Delete the User

Inside the administrator panel:

1. Locate the user **`carlos`**.
2. Click **Delete**.
3. The user is removed.
4. The lab is successfully solved.

---

## 🔐 Root Cause

The application fails to perform proper **server-side authorization checks** on the administrator endpoint.

The application should verify whether the current user has administrator privileges before allowing access.

Instead, the endpoint can be accessed directly:

```text
GET /administrator-panel
```

without requiring administrator authorization.

---

## 💥 Impact

If this vulnerability exists in a real application, an attacker may be able to:

* Access administrative functionality
* Delete users
* Modify user accounts
* Change application settings
* Access sensitive administrative data
* Perform unauthorized administrative actions

The impact depends on what functionality is available within the exposed admin panel.

---

## 🛡️ Remediation

Implement **server-side authorization checks** for every administrative endpoint.

For example:

```text
User requests /administrator-panel
            ↓
Check authentication
            ↓
Check administrator role
            ↓
      ┌─────┴─────┐
      ↓           ↓
   Admin       Non-admin
      ↓           ↓
   Allow       Deny (403)
```

Do not rely on:

* Hidden URLs
* `robots.txt`
* Client-side JavaScript
* Removing links from the UI
* Obscurity

Authorization must be enforced on the **server side**.

---

## 🧠 Key Learning

> `robots.txt` is not an access-control mechanism.

It can unintentionally reveal sensitive paths, but the real vulnerability is the lack of authorization protection on the administrator endpoint.

### Important VAPT Takeaway

During a web application security assessment, always test whether sensitive endpoints can be accessed directly by unauthorized users.

Examples include:

```text
/admin
/admin-panel
/administrator
/management
/dashboard
/backend
/cms
```

The endpoint name may be different in a real application, so proper application enumeration and authorization testing are important.

---

## 🛠️ Tools Used

* Web Browser
* PortSwigger Web Security Academy
* Burp Suite (optional)

---

## 📚 Vulnerability Category

**OWASP Top 10 – Broken Access Control**

This lab demonstrates how missing authorization controls can allow an unauthorized user to access administrative functionality.

---

## ✅ Result

**Lab successfully solved by accessing the unprotected administrator panel and deleting the user `carlos`.**

<img width="1227" height="587" alt="image" src="https://github.com/user-attachments/assets/1bcdb00d-514f-473e-b714-64d07e6cdf4d" />
<img width="1888" height="991" alt="image" src="https://github.com/user-attachments/assets/e97a9a1a-b934-4770-b130-ede6133be897" />
<img width="1925" height="913" alt="image" src="https://github.com/user-attachments/assets/784ce423-c4bd-4c9d-a89c-ef05aaa1df58" />
<img width="1886" height="942" alt="image" src="https://github.com/user-attachments/assets/b345df55-c8c1-4047-875f-2ef569de6253" />
<img width="1886" height="893" alt="image" src="https://github.com/user-attachments/assets/2522b376-ef29-4c86-88ab-02ecfdfb09bf" />
