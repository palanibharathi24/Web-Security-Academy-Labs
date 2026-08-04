# SQL Injection UNION Attack – Determining Number of Columns

## Lab Information

| Field          | Details                          |
| -------------- | -------------------------------- |
| Vulnerability  | SQL Injection                    |
| Attack Type    | UNION-Based SQL Injection        |
| Platform       | PortSwigger Web Security Academy |
| Difficulty     | Practitioner                     |
| Testing Method | Manual Testing                   |
| Status         | Completed                        |

## Objective

Identify the number of columns returned by the application's SQL query using a UNION-based SQL injection technique.

## Methodology

1. Opened the product category functionality.
2. Identified the `category` parameter as the injection point.
3. Tested the parameter with a UNION query containing a single `NULL` value.

```sql
' UNION SELECT NULL--
```

4. Observed that the application returned an error.
5. Added another `NULL` value to test for two columns.

```sql
' UNION SELECT NULL,NULL--
```

6. The error disappeared and the response was returned successfully.
7. This confirmed that the original SQL query returns **2 columns**.

## Payload Testing

| Columns Tested | Payload                      | Result     |
| -------------: | ---------------------------- | ---------- |
|              1 | `' UNION SELECT NULL--`      | Error      |
|              2 | `' UNION SELECT NULL,NULL--` | Successful |

## Key Concept

A UNION attack requires the injected `SELECT` statement to contain the same number of columns as the original query.

By incrementally adding `NULL` values until the error disappears, the number of columns returned by the original query can be determined.

## Impact

If a SQL Injection vulnerability is successfully exploited, an attacker may potentially:

* Access unauthorized database information
* Retrieve sensitive application data
* Bypass application logic
* Modify database information
* Perform further database-level attacks

The actual impact depends on the application's database permissions and security controls.

## Root Cause

The application incorporated user-controlled input directly into an SQL query without using parameterized queries or prepared statements.

## Remediation

* Use parameterized queries / prepared statements
* Avoid constructing SQL queries through string concatenation
* Apply appropriate server-side input validation
* Use least-privilege database accounts
* Properly handle database errors

## Tools Used

* Web Browser
* PortSwigger Web Security Academy
* Burp Suite — where applicable

## Result

Successfully determined that the vulnerable SQL query returns **2 columns** using a UNION-based SQL injection technique.
<img width="1863" height="938" alt="image" src="https://github.com/user-attachments/assets/3a54c0de-fd25-4e96-beca-a3860e35f1d1" />
<img width="1893" height="965" alt="image" src="https://github.com/user-attachments/assets/d9264b57-2296-48c9-83e3-5576807b2b4b" />
