## Title

Reflected XSS - Search Portal: First Name and Last Name Parameters

## Description

The website's search functionality appears vulnerable to Reflected XSS through the `fname` and `lname` parameters. Crafted JavaScript payloads are supplied through both parameters to test whether they execute when the modified URL is opened.

**Affected URL:** https://search.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://search.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FsCRipt%3E&lname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FsCRipt%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect how the supplied first and last names are reflected on the page.

## POC (Proof of Concept)

**Target URL:** https://search.pwnedbox.site/

**Affected Parameters:** `fname`, `lname`

**Payload:**
```html
hakama'"<><sCRipt>alert(1)</sCRipt>
```

**Observed Result:** The payload is designed to test JavaScript execution through both name parameters. Successful execution should be confirmed in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3e0c8227-6308-45ea-9f58-608c455da7df" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b60db2b2-58a4-443b-8921-8598f0b2360e" />

## Impact

1. An attacker could potentially execute JavaScript in a visitor's browser under the search portal's origin.
2. Malicious scripts could manipulate displayed search results or inject misleading content into the page.
3. Visitors opening a specially crafted link could be exposed to phishing attempts or unauthorized browser-side actions.

## Remediation

1. Apply context-aware output encoding to both `fname` and `lname` wherever their values are displayed.
2. Validate name fields against expected input formats and reject unexpected markup where appropriate.
3. Use safe DOM methods instead of inserting user-controlled values directly into HTML.
