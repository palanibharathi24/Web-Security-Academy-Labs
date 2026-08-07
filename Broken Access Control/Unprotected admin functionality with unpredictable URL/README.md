# Unprotected Admin Functionality with Unpredictable URL

## 📌 Lab Information

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Vulnerability:** Broken Access Control
* **Tool:** Burp Suite
* **Status:** ✅ Solved

## 🎯 Objective

The objective of this lab is to discover the unpredictable admin panel URL and use the admin functionality to delete the user `carlos`.

## 🔍 Steps to Solve

### 1. Intercept the Home Page Request

Opened the lab in the browser with **Burp Suite Proxy** enabled.

In:

```text
Proxy → HTTP History
```

located the request for the home page:

```http
GET / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
```

### 2. Analyze the Response

Selected the corresponding response and inspected the HTML/JavaScript source code.

Searched for:

```text
admin
```

The JavaScript code contained the path to the administrator panel.

Example:

```javascript
var adminPanel = "/<unpredictable-admin-path>";
```

The actual path was unique to the lab.

### 3. Access the Admin Panel

Copied the admin panel path discovered in the response and accessed it directly through the browser.

```text
https://YOUR-LAB-ID.web-security-academy.net/<admin-path>
```

The admin panel was accessible without administrator authorization.

### 4. Delete `carlos`

Inside the admin panel, located the user:

```text
carlos
```

Clicked **Delete**.

The lab was successfully solved.

## 🔐 Vulnerability

The application does not properly enforce **server-side authorization** on the administrator panel.

Although the admin URL is unpredictable, the application exposes the URL through client-side JavaScript.

An attacker can inspect the application's responses and discover the hidden endpoint.

## 🛠️ Tools Used

* Burp Suite
* Web Browser
* PortSwigger Web Security Academy

## 💥 Impact

An unauthorized user may be able to access administrative functionality and perform privileged actions such as:

* Delete users
* Modify user accounts
* Access administrative features
* Perform unauthorized actions

## 🛡️ Remediation

Implement proper **server-side authorization checks** for all administrative endpoints.

Do not rely on:

* Unpredictable URLs
* Hidden endpoints
* Client-side JavaScript
* UI restrictions

The server should verify the user's privileges before allowing access to administrative functionality.

## 🧠 Key Learning

An unpredictable URL does not provide real security.

During VAPT testing, inspect **HTML, JavaScript, API responses, and Burp Suite HTTP history** for sensitive endpoints that may be exposed to unauthorized users.

**Root Cause:** Missing server-side authorization
**Category:** Broken Access Control

<img width="1219" height="543" alt="image" src="https://github.com/user-attachments/assets/c3eb965e-8fa6-4550-9ef5-7b1519770258" />
<img width="1918" height="560" alt="image" src="https://github.com/user-attachments/assets/d50eead8-5ca5-4c64-aa8d-32e4b3b92b2f" />
<img width="1903" height="970" alt="image" src="https://github.com/user-attachments/assets/8c3fdf1a-7d99-4be5-befc-7a7d659dbd01" />
<img width="1915" height="1012" alt="image" src="https://github.com/user-attachments/assets/8c314df1-499d-4da0-a794-279d05590c6f" />
<img width="1872" height="777" alt="image" src="https://github.com/user-attachments/assets/f2881ef6-13f2-488a-9d25-18af7272ace4" />
<img width="1897" height="821" alt="image" src="https://github.com/user-attachments/assets/f0cbd972-164c-4682-be9c-b955b36911be" />
