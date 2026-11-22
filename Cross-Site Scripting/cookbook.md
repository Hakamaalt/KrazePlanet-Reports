## Title

Reflected XSS - Cookbook: First Name and Last Name Parameters

## Description

The website appears vulnerable to Reflected XSS through the `fname` and `lname` parameters. Crafted JavaScript payloads are supplied through both parameters to test whether they execute when the modified URL is opened.

**Affected URL:** https://cookbook.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://cookbook.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FsCRipt%3E&lname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FsCRipt%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect how the supplied first and last names are reflected on the page.

## POC (Proof of Concept)

**Target URL:** https://cookbook.pwnedbox.site/

**Affected Parameters:** `fname`, `lname`

**Payload:**
```html
hakama'"><><sCRipt>alert(1)</sCRipt>
```

**Observed Result:** The payload is intended to test JavaScript execution through both name parameters. Successful execution should be confirmed in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3dc4b49a-dd84-463b-ae0c-f02d59393842" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6f3fb33a-c443-4992-ad5f-c41552f11d46" />

## Impact

1. An attacker could potentially execute JavaScript in a visitor's browser under the Cookbook website's origin.
2. Injected scripts could manipulate displayed recipe information or introduce deceptive content.
3. Visitors opening a crafted link could be exposed to phishing attempts or unauthorized browser-side actions.

## Remediation

1. Apply context-aware output encoding to both `fname` and `lname` wherever their values are displayed.
2. Validate user-submitted name fields on the server side and reject unexpected HTML or script markup.
3. Use safe DOM methods when rendering user-controlled data instead of directly inserting it as HTML.
