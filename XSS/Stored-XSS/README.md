# Stored XSS into HTML Context

## Lab Information

| Field          | Details                           |
| -------------- | --------------------------------- |
| Vulnerability  | Stored Cross-Site Scripting (XSS) |
| Platform       | PortSwigger Web Security Academy  |
| Testing Method | Manual Testing                    |
| Difficulty     | Apprentice                        |
| Context        | HTML                              |
| Status         | Completed                         |

## Objective

Identify and exploit a Stored XSS vulnerability in the blog comment functionality by injecting JavaScript that executes when the blog post is viewed.

## Methodology

1. Opened the vulnerable blog post.
2. Identified the comment functionality as a potential injection point.
3. Submitted a controlled XSS payload in the comment field.
4. Used the following payload in the authorized lab:

```html
<script>alert(1)</script>
```

5. Entered the required name, email, and website fields.
6. Posted the comment.
7. Returned to the blog post.
8. Observed the JavaScript alert execution, confirming Stored XSS.

## Impact

Successful exploitation may allow an attacker to:

* Execute JavaScript in visitors' browsers
* Modify webpage content
* Perform actions on behalf of users
* Redirect users to malicious websites
* Access information available to client-side JavaScript

## Root Cause

The application stored user-controlled comment content and rendered it in the HTML response without appropriate output encoding.

## Remediation

* Apply context-appropriate output encoding
* Sanitize HTML input when HTML content is required
* Use secure templating frameworks with automatic escaping
* Validate user input where appropriate
* Implement Content Security Policy (CSP)

## Tools Used

* Web Browser
* PortSwigger Web Security Academy

## Result

Successfully completed the lab through manual testing and demonstrated a Stored XSS vulnerability by injecting JavaScript into the comment functionality and triggering execution when the blog post was viewed.

<img width="1887" height="912" alt="image" src="https://github.com/user-attachments/assets/f33f4010-a57e-48f9-b8d9-f6538a66c674" />
<img width="1901" height="956" alt="image" src="https://github.com/user-attachments/assets/b573cac7-248a-4b02-a01a-788a46c637ed" />
<img width="1888" height="906" alt="image" src="https://github.com/user-attachments/assets/ce29106f-d1f3-400f-948d-c3ab0b645203" />
