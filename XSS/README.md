# Cross-Site Scripting (XSS)

## Overview

Cross-Site Scripting (XSS) is a web application vulnerability that occurs when untrusted user input is processed in an unsafe manner, allowing attacker-controlled JavaScript to execute in a user's browser.

## XSS Types

### 1. Reflected XSS

User-controlled input is immediately reflected in the application's response without proper encoding.

**Common Areas:**

* Search parameters
* URL parameters
* Form inputs
* Error messages

### 2. Stored XSS

Malicious input is stored by the application and executed when users later access the affected content.

**Common Areas:**

* Comments
* Reviews
* User profiles
* Messages

### 3. DOM-Based XSS

XSS occurs when client-side JavaScript processes attacker-controlled input and modifies the DOM in an unsafe manner.

**Common Sources:**

* `location.search`
* `location.hash`
* `document.URL`

**Common Sinks:**

* `innerHTML`
* `document.write()`
* `eval()`

## Testing Approach

1. Identify user-controlled input.
2. Test how the application processes the input.
3. Identify the XSS context.
4. Test with controlled input in an authorized environment.
5. Validate JavaScript execution.
6. Classify the vulnerability as Reflected, Stored, or DOM-Based XSS.
7. Document impact, root cause, and remediation.

## Security Impact

XSS may allow an attacker to:

* Execute JavaScript in a victim's browser
* Modify webpage content
* Perform actions on behalf of users
* Redirect users to malicious websites
* Access information available to client-side JavaScript

## Remediation

* Apply context-appropriate output encoding
* Use secure templating frameworks
* Validate input where appropriate
* Avoid unsafe DOM manipulation
* Use safe DOM APIs
* Implement Content Security Policy (CSP)

## Learning Areas

* Reflected XSS
* Stored XSS
* DOM-Based XSS
* XSS Contexts
* DOM Sources & Sinks
* Input Validation
* Output Encoding
* Vulnerability Remediation

## Platforms & Tools

* PortSwigger Web Security Academy
* Web Browser
* Burp Suite — where applicable

