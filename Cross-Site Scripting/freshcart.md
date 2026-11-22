## Title

Reflected XSS - FreshCart: Online Grocery Shopping Website

## Description

The website's search functionality appears vulnerable to Reflected XSS through the `q` parameter. A crafted JavaScript payload is supplied through the parameter to test whether it executes when the modified URL is opened.

**Affected URL:** https://freshcart.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://freshcart.pwnedbox.site/index.php?q=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect the reflected search content on the page.

## POC (Proof of Concept)

**Target URL:** https://freshcart.pwnedbox.site/

**Affected Parameter:** `q`

**Payload:**
```html
<script>alert(1)</script>
```

**Observed Result:** The payload is intended to execute JavaScript and display an alert dialog containing `1` in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/eae52b55-0f1e-450b-8ad7-40803a134ca9" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/86d41c33-d03b-4e5f-a9e3-96556aadd609" />

## Impact

1. An attacker could execute arbitrary JavaScript in a visitor's browser under the FreshCart website's origin.
2. Malicious scripts could display fake login or payment forms to deceive customers.
3. Attackers could potentially manipulate displayed product information or interfere with the shopping experience.

## Remediation

1. Apply context-aware output encoding to the `q` parameter wherever search input is displayed.
2. Ensure product search results and dynamically generated page content treat user input as data rather than executable HTML.
3. Implement a restrictive Content Security Policy (CSP) to reduce the risk of unauthorized script execution.
