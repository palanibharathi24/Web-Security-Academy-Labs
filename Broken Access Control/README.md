# Broken Access Control – PortSwigger Labs

## 📌 Overview

This section contains hands-on **Broken Access Control** labs completed on the PortSwigger Web Security Academy.

These labs focus on identifying and exploiting improper authorization and access control mechanisms in web applications.

## 🧪 Completed Labs

| # | Lab                                                                   | Difficulty | Status   |
| - | --------------------------------------------------------------------- | ---------- | -------- |
| 1 | Unprotected admin functionality                                       | Apprentice | ✅ Solved |
| 2 | Unprotected admin functionality with unpredictable URL                | Apprentice | ✅ Solved |
| 3 | User role controlled by request parameter                             | Apprentice | ✅ Solved |
| 4 | User role can be modified in user profile                             | Apprentice | ✅ Solved |
| 5 | User ID controlled by request parameter                               | Apprentice | ✅ Solved |
| 6 | User ID controlled by request parameter with unpredictable user IDs   | Apprentice | ✅ Solved |
| 7 | User ID controlled by request parameter with data leakage in redirect | Apprentice | ✅ Solved |
| 8 | User ID controlled by request parameter with password disclosure      | Apprentice | ✅ Solved |
| 9 | Insecure direct object references (IDOR)                              | Apprentice | ✅ Solved |

## 🔍 Key Concepts Practiced

* Missing authorization checks
* Privilege escalation
* Horizontal privilege escalation
* Vertical privilege escalation
* IDOR
* Parameter manipulation
* User role manipulation
* Unauthorized access to admin functionality
* Sensitive information disclosure

## 🛠️ Tools Used

* Burp Suite
* Web Browser
* PortSwigger Web Security Academy

## 🎯 Key Learning

Broken Access Control occurs when an application fails to properly enforce what an authenticated or unauthenticated user is allowed to access or modify.

During testing, I practiced manipulating:

```text
URL parameters
Request parameters
User IDs
Role parameters
Profile settings
Administrative endpoints
```

and verified whether unauthorized actions or data access were possible.

## 🛡️ Remediation

Applications should implement proper **server-side authorization checks** for every sensitive resource and action.

Do not rely on:

* Hidden URLs
* Client-side controls
* User-controlled role parameters
* Predictable IDs
* UI restrictions

Authorization should always be enforced on the server side.

## 📚 Reference

[PortSwigger Web Security Academy – Access Control](https://portswigger.net/web-security/access-control)
