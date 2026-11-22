## Title

Reflected XSS - PubMed: Biomedical Literature Search

## Description

The website's search functionality appears vulnerable to Reflected XSS through the `term` parameter. A crafted JavaScript payload is supplied through the parameter to test whether it executes when the modified URL is opened.

**Affected URL:** https://pubmed.pwnedbox.site/

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://pubmed.pwnedbox.site/index.php?term=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

2. Allow the page to load.
3. Observe whether a JavaScript alert dialog displaying `1` appears.
4. Dismiss the alert and inspect the reflected search term on the page.

## POC (Proof of Concept):

**Target URL:** https://pubmed.pwnedbox.site/

**Affected Parameter:** `term`

**Payload:**
```html
<script>alert(1)</script>
```

**Observed Result:** The payload is intended to display a JavaScript alert containing `1`. Successful execution needs to be confirmed in the browser.

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/473badbd-7267-4d51-91b7-542b116136fd" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c711f671-8ec8-47b1-bd66-4aec3b81c7ea" />

## Impact

1.An attacker could execute arbitrary JavaScript in a visitor's browser under the PubMed website's origin.
2.Malicious scripts could manipulate displayed search results or inject deceptive content, potentially misleading visitors.
3.Attackers could redirect visitors to malicious websites through injected scripts, exposing them to phishing or other attacks.

## Remediation

1. Apply context-aware output encoding wherever the `term` parameter is displayed.
2. Treat search terms strictly as data and prevent them from being interpreted as HTML or JavaScript.
3. Validate search input and use safe DOM APIs when dynamically displaying search terms.
