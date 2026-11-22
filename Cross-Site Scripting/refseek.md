## Title

Reflected XSS - RefSeek: Academic Search Engine

## Description

The website's search functionality is vulnerable to Reflected XSS through the `q` parameter. A crafted JavaScript payload successfully executes in the browser when the modified URL is opened.

**Affected URL:** https://refseek.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://refseek.pwnedbox.site/index.php?q=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

2. Allow the page to load.
3. Observe the JavaScript alert dialog displaying `1`.
4. Dismiss the alert and inspect the reflected content on the page.

## POC (Proof of Concept)

**Target URL:** https://refseek.pwnedbox.site/

**Affected Parameter:** `q`

**Payload:**
```html
<script>alert(1)</script>
```

**Observed Result:** The injected JavaScript executes successfully and displays an alert dialog containing `1` in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/18631f5d-bdbd-437d-bc16-f8db5c33f6ce" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c607f8c3-d144-4428-adee-d2c1749a0227" />

## Impact

1. An attacker could execute arbitrary JavaScript in a visitor's browser under the RefSeek website's origin.
2. Malicious scripts could manipulate page content, display deceptive login forms, or perform unauthorized actions available to the victim's browser session.
3. Visitors accessing a specially crafted link could be affected without intentionally interacting with the search functionality.

## Remediation

1. Apply context-aware output encoding to all user-controlled search input before rendering it in HTML.
2. Avoid directly inserting untrusted input into executable script contexts or dynamically generated HTML.
3. Implement a restrictive Content Security Policy (CSP) to provide an additional layer of protection against script injection
