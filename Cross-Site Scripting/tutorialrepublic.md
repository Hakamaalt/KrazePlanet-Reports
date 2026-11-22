## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search

## Description
During testing, I identified a Reflected XSS vulnerability in the website's search functionality through the `q` parameter. By submitting a crafted JavaScript payload, the browser successfully executed the injected code and displayed a prompt containing `1`. This confirms that user input is not properly handled before being reflected in the browser. The vulnerability can be triggered simply by opening a specially crafted URL without requiring any further interaction.

## Steps to Reproduce

1. Open the following crafted URL in a browser:

   `https://tutorialrepublic.pwnedbox.site/?q=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

2. Allow the page to load.
3. Observe the JavaScript prompt displaying `1`.
4. Dismiss the prompt and observe the reflected content on the page.

## POC (Proof of Concept)

**Lab Address:** https://tutorialrepublic.pwnedbox.site/

**Affected Parameter:** `q`

**Payload:**
```html
<ScRiPt>alert(1)</ScRiPt>
```

**Observed Result:** The JavaScript prompt displaying `1` in the browser.

**Screenshots:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d3020b14-1b4e-4f7e-95c3-81e18e7d001d" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b877be59-db21-4f84-989b-419ded1cf6fb" />

## Impact
1. An attacker could execute arbitrary JavaScript in a victim's browser using a specially crafted URL.
2. This could potentially enable phishing attacks, unauthorized actions, or access to sensitive session data, depending on application security controls.

## Remediation
1. Implement proper input validation and context-aware output encoding.
2. Configure a Content Security Policy (CSP) to restrict unauthorized script execution.
