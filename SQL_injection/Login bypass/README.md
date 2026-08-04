

# SQL Injection – Login Bypass

## Lab Information

| Field         | Details                          |
| ------------- | -------------------------------- |
| Platform      | PortSwigger Web Security Academy |
| Vulnerability | SQL Injection                    |
| Impact        | Authentication Bypass            |
| Difficulty    | Apprentice                       |
| Tool          | Burp Suite                       |
| Status        | Completed                        |

## Objective

Identify and exploit a SQL Injection vulnerability in the login functionality to bypass authentication and access the application as the administrator.

## Methodology

1. Opened the login page and captured the login request using **Burp Suite**.
2. Identified the `username` parameter as an injection point.
3. Tested the parameter with controlled SQL Injection input.
4. Used the following payload in the authorized lab:

```text
administrator'--
```

5. Forwarded the modified request and analyzed the response.
6. Successfully bypassed authentication and logged in as the **administrator** user.

## Impact

Successful exploitation of this vulnerability could allow an attacker to:

* Bypass authentication
* Access unauthorized accounts
* Gain administrative privileges
* Access or modify sensitive application data

## Root Cause

The application improperly incorporated user-controlled input into the SQL query without using parameterized queries.

## Remediation

* Use **prepared statements / parameterized queries**
* Implement proper server-side input validation
* Apply least-privilege database access
* Avoid exposing detailed database errors

## Tools Used

* Burp Suite
* Web Browser
* PortSwigger Web Security Academy

## Result

**Successfully completed the lab and demonstrated SQL Injection-based authentication bypass.**

> Testing was performed only in the authorized PortSwigger Web Security Academy lab environment.

<img width="1901" height="1015" alt="image" src="https://github.com/user-attachments/assets/88e624de-a8b4-4937-8c79-a015512db1f5" />

<img width="1903" height="1013" alt="image" src="https://github.com/user-attachments/assets/ee96ab5f-bfc2-4dec-be07-affb848af5fd" />
<img width="1900" height="1009" alt="image" src="https://github.com/user-attachments/assets/7d91ccd7-aea7-446d-ac7b-2884f9667368" />
