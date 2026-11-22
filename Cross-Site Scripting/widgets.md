## Title

Reflected XSS - Reddit

## Description

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the `post_id` parameter on the widgets page. User-controlled input is reflected into the page without proper sanitization, allowing JavaScript execution through an injected `onmouseover` event handler when a user hovers over the affected button.

**Affected URL:** https://widgets.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   ```text
   https://widgets.pwnedbox.site/?post_id=t3_u9po1l%20onmouseover=alert(document.domain)%20y=
   ```

2. Allow the page to load.

3. Locate the affected button on the Reddit-style page.

4. Hover over the affected button.

5. Observe the JavaScript alert displaying `widgets.pwnedbox.site`, confirming successful JavaScript execution.

## POC (Proof of Concept)

**Lab Address:**  
https://widgets.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `post_id`

**Payload:**
```html
t3_u9po1l onmouseover=alert(document.domain) y=
```

**Crafted URL:**
```text
https://widgets.pwnedbox.site/?post_id=t3_u9po1l%20onmouseover=alert(document.domain)%20y=
```

**Observed Result:**

The injected `onmouseover` event handler was reflected into the affected button. When the user hovered over the button, JavaScript executed successfully and displayed an alert containing `widgets.pwnedbox.site`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a4e54d91-7a34-48cc-8b7e-fcc3d9f7d0ee" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/568e0268-3e74-4c51-a7e2-7e54b14fa69a" />

## Impact

- An attacker can craft a malicious URL and trick victims into interacting with the affected button.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed page content.

## Remediation

- Apply context-aware output encoding to the `post_id` parameter before rendering it in HTML.
- Validate the `post_id` parameter using an appropriate allowlist and prevent untrusted input from entering HTML attributes.
- Implement a restrictive Content Security Policy (CSP) to block unauthorized inline JavaScript execution.
