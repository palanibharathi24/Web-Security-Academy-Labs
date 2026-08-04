# Reflected XSS into HTML Context

## Lab Information

| Field          | Details                              |
| -------------- | ------------------------------------ |
| Platform       | PortSwigger Web Security Academy     |
| Vulnerability  | Reflected Cross-Site Scripting (XSS) |
| Impact         | Arbitrary JavaScript Execution       |
| Difficulty     | Apprentice                           |
| Tool           | Web Browser                          |
| Testing Method | Manual Testing                       |
| Status         | Completed                            |

## Objective

Identify and exploit a Reflected XSS vulnerability in the search functionality by injecting JavaScript into an HTML context and triggering the `alert()` function.

## Methodology

Opened the search functionality and entered a normal search term.

Observed that the search input was reflected back in the application's HTML response.

Tested the search parameter manually with a controlled XSS payload.

Used the following payload in the authorized lab:

```html
<script>alert(1)</script>
```

Submitted the payload through the search functionality.

Observed that the JavaScript was executed by the browser and the `alert()` function was triggered.

This confirmed the presence of a Reflected XSS vulnerability.

## Impact

Successful exploitation of this vulnerability could allow an attacker to:

* Execute arbitrary JavaScript in a victim's browser
* Modify webpage content
* Perform actions on behalf of the victim
* Potentially access sensitive information available to client-side JavaScript
* Redirect users to malicious websites

## Root Cause

The application reflected user-controlled input directly into the HTML response without applying appropriate output encoding.

## Remediation

* Apply context-appropriate output encoding
* Use secure templating frameworks with automatic HTML escaping
* Validate user input where appropriate
* Avoid inserting untrusted input directly into HTML
* Implement Content Security Policy (CSP) as an additional defense-in-depth control

## Tools Used

* Web Browser
* PortSwigger Web Security Academy

<img width="1877" height="960" alt="image" src="https://github.com/user-attachments/assets/2dc13db6-a641-461e-ba5c-00ad1ee8f5d3" />
<img width="1900" height="957" alt="image" src="https://github.com/user-attachments/assets/8f3dbf7e-c550-496f-a8e8-1f0dae5ba657" />
<img width="1884" height="895" alt="image" src="https://github.com/user-attachments/assets/0dfb4aac-98fb-4c11-b2ca-10fe99db5836" />
