## Title

Reflected XSS - Feedback Portal: First Name and Last Name Parameters

## Description

The website's feedback functionality appears vulnerable to Reflected XSS through the `fname` and `lname` parameters. Crafted JavaScript payloads are supplied through both parameters to test whether they execute when the modified URL is opened.

**Affected URL:** https://feedback.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://feedback.pwnedbox.site/?fname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FSCript%3E&lname=hakama%27%22%3C%3E%3CsCRipt%3Ealert%281%29%3C%2FSCript%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect how the supplied first and last names are reflected on the page.

## POC (Proof of Concept)

**Target URL:** https://feedback.pwnedbox.site/

**Affected Parameters:** `fname`, `lname`

**Payload:**
```html
hakama'"><><sCRipt>alert(1)</sCRipt>
```

**Observed Result:** The payload is intended to test JavaScript execution through both name parameters. Successful execution should be confirmed in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5a8eee8e-b9c0-4dee-b705-44f56c375f1d" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/089238cd-8e1e-4f54-b4aa-c0b616f6cfed" />

## Impact

1. An attacker could potentially execute JavaScript in a visitor's browser under the feedback portal's origin.
2. Injected scripts could manipulate feedback forms or display misleading content to visitors.
3. Attackers could use the vulnerable functionality to present fraudulent feedback or identity-related forms to unsuspecting users.

## Remediation

1. Apply context-aware output encoding to the `fname` and `lname` parameters wherever they are reflected.
2. Validate name inputs on the server side and reject unexpected HTML or script markup.
3. Use safe DOM APIs when displaying user-submitted feedback data instead of directly inserting it as HTML.
