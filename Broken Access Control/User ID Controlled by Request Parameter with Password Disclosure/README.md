User ID Controlled by Request Parameter with Password Disclosure

## Lab Information

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Vulnerability:** Broken Access Control / IDOR
* **Impact:** Administrator Account Compromise
* **Tool:** Burp Suite
* **Status:** Solved ✅

---

## Objective

The application contains an account page where the user's password is present inside a masked password field.

The goal was to:

1. Login as `wiener`.
2. Manipulate the user ID parameter.
3. Access the administrator's account.
4. Retrieve the administrator's password.
5. Login as administrator.
6. Delete the user `carlos`.

### Credentials

```text
Username: wiener
Password: peter
```

---

## Methodology

### 1. Login as `wiener`

I logged into the application using:

```text
wiener:peter
```

After login, I accessed the **My Account** page.

---

### 2. Identify the User ID Parameter

Using **Burp Suite → Proxy → HTTP history**, I captured the account request:

```http
GET /my-account?id=wiener HTTP/2
Host: <LAB-HOST>
Cookie: session=<SESSION>
```

The `id` parameter controls which user's account is requested.

I sent the request to **Burp Repeater**.

---

### 3. Test for IDOR / Broken Access Control

I changed:

```text
id=wiener
```

to:

```text
id=administrator
```

Modified request:

```http
GET /my-account?id=administrator HTTP/2
Host: <LAB-HOST>
Cookie: session=<SESSION>
```

The application returned the administrator's account page even though I was authenticated as `wiener`.

This confirmed a **Broken Access Control / IDOR-style vulnerability**.

---

### 4. Retrieve Administrator Password

I inspected the administrator account response in Burp Repeater.

The password was present inside the HTML password field:

```html
<input type="password" name="password" value="ADMIN_PASSWORD">
```

Although the browser displayed the password as:

```text
********
```

the actual value was present in the HTTP response.

I extracted the administrator password.

---

### 5. Login as Administrator

I used the discovered credentials to authenticate as:

```text
Username: administrator
Password: <ADMIN_PASSWORD>
```

The login was successful.

---

### 6. Delete `carlos`

After logging in as administrator, I accessed the administrative functionality and deleted:

```text
carlos
```

The lab was successfully completed.

---

## Attack Flow

```text
Login as wiener
       ↓
/my-account?id=wiener
       ↓
Change ID to administrator
       ↓
/my-account?id=administrator
       ↓
Access administrator account
       ↓
Password disclosed in HTML
       ↓
Login as administrator
       ↓
Delete carlos
       ↓
LAB SOLVED
```

---

## Root Cause

The application does not properly validate whether the authenticated user is authorized to access the requested account.

The server trusts the user-controlled:

```text
?id=administrator
```

parameter.

Additionally, the administrator's existing password is unnecessarily returned to the client.

---

## Security Impact

An attacker could potentially:

* Access other users' account information.
* Access administrator information.
* Obtain sensitive credentials.
* Compromise an administrator account.
* Perform privileged actions.

---

## Remediation

* Implement proper server-side authorization checks.
* Do not trust user-controlled IDs for authorization.
* Restrict users to their own account resources.
* Never return existing plaintext passwords to the client.
* Store passwords using secure password-hashing algorithms.
* Apply authorization checks to all sensitive administrative functions.

---

## Tools Used

* **Burp Suite**

  * Proxy
  * HTTP History
  * Repeater
* **Web Browser**
* **PortSwigger Web Security Academy**

---

<img width="1252" height="495" alt="image" src="https://github.com/user-attachments/assets/213c6915-4cf8-4ad2-9bd5-2d66f497bac1" />
<img width="1592" height="547" alt="image" src="https://github.com/user-attachments/assets/7f0bb8ad-f75d-4aa7-a6f3-e0bd1256153c" />
<img width="1887" height="897" alt="image" src="https://github.com/user-attachments/assets/0e62af2b-f1ea-4509-9fe4-a72ba16ac235" />
<img width="1832" height="865" alt="image" src="https://github.com/user-attachments/assets/d562e45a-eb43-40df-9daa-d1347c4156bd" />
<img width="1902" height="805" alt="image" src="https://github.com/user-attachments/assets/ed334f4d-c7a7-4d65-8986-8e5b38ff24c7" />
