## Title
Reflected XSS - GIGW: Government Website Guidelines Portal

## Description
The website's search functionality appears vulnerable to Reflected Cross-Site Scripting (XSS) through the `s` parameter. A JavaScript payload is supplied through the search parameter, which may execute in the browser if the input is reflected without proper output encoding.

**Affected URL:** https://guidelines.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://guidelines.pwnedbox.site/index.php?s=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect the reflected content on the page.

## POC (Proof of Concept)

**Target URL:** https://guidelines.pwnedbox.site/

**Affected Parameter:** `s`

**Payload:**
```html
<script>alert(1)</script>
```

**Observed Result:** The payload is intended to trigger a JavaScript alert displaying `1`. Successful execution should be confirmed in the browser.

**Screenshots:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c80a42a3-1834-48af-a5d8-3a2624132fb2" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/19811650-67cb-49c8-af75-ae8c5dd1fc54" />

## Impact
1. Successful exploitation could allow attackers to execute JavaScript in the context of the GIGW portal's origin.
2. Attackers may use this behavior to display deceptive content or create phishing interfaces targeting visitors.
3. The vulnerability could affect users who open a maliciously crafted link, subject to the application's security controls.

## Remediation
1. Apply context-aware output encoding to the `s` parameter wherever its value is rendered in HTML.
2. Avoid inserting untrusted search input directly into executable HTML or JavaScript contexts.
3. Implement a restrictive Content Security Policy (CSP) as an additional security layer.
