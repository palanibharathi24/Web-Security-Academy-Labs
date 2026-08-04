# DOM XSS in document.write Sink Using location.search

## Lab Information

| Field          | Details                              |
| -------------- | ------------------------------------ |
| Vulnerability  | DOM-Based Cross-Site Scripting (XSS) |
| Platform       | PortSwigger Web Security Academy     |
| Testing Method | Manual Testing                       |
| Difficulty     | Apprentice                           |
| Source         | `location.search`                    |
| Sink           | `document.write()`                   |
| Context        | HTML / Attribute                     |
| Status         | Completed                            |

## Objective

Identify and exploit a DOM-based XSS vulnerability caused by unsafe use of `location.search` with the `document.write()` sink.

## Methodology

1. Opened the vulnerable search functionality.
2. Entered a random alphanumeric string into the search box.
3. Inspected the page source using browser Developer Tools.
4. Observed that the search input was inserted into an `img` `src` attribute.
5. Identified `location.search` as the source and `document.write()` as the vulnerable sink.
6. Tested a controlled XSS payload to break out of the `img` attribute:

```html
"><svg onload=alert(1)>
```

7. Submitted the payload through the search functionality.
8. Observed successful execution of the JavaScript `alert()` function.

## Impact

Successful exploitation may allow an attacker to:

* Execute JavaScript in a victim's browser
* Modify webpage content
* Perform actions on behalf of the victim
* Redirect users to malicious websites
* Access information available to client-side JavaScript

## Root Cause

The application used attacker-controlled data from `location.search` with the `document.write()` function without appropriate output encoding or safe DOM handling.

## Remediation

* Avoid using `document.write()` with untrusted data
* Apply context-appropriate output encoding
* Use safe DOM APIs instead of unsafe HTML insertion
* Validate user-controlled input where appropriate
* Implement Content Security Policy (CSP)

## Tools Used

* Web Browser
* Browser Developer Tools
* PortSwigger Web Security Academy

## Result

Successfully completed the lab through manual testing and demonstrated a DOM-Based XSS vulnerability by exploiting the `location.search` source and `document.write()` sink.
<img width="1886" height="802" alt="image" src="https://github.com/user-attachments/assets/98bd1d01-a81f-4b2b-8456-9712451c4138" />
<img width="1902" height="934" alt="image" src="https://github.com/user-attachments/assets/323a7c26-141f-487a-b0e2-18646a8c7621" />
<img width="1865" height="894" alt="image" src="https://github.com/user-attachments/assets/11a564ea-5f5d-4630-8bd0-e666dd84b406" />
