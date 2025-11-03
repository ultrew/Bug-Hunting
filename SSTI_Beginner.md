# Server Side Template Injection (SSTI) - Beginner's Guide

## What is SSTI?

Server Side Template Injection (SSTI) is a web security vulnerability where an attacker can inject malicious input into a web application's template engine. This can allow the attacker to execute arbitrary code on the server, leading to serious security risks.

## What is a Template Engine?

A template engine generates dynamic web pages by combining static templates with data. It takes placeholders in the template (like variables or expressions) and replaces them with actual data at runtime.

Example using Python Flask with Jinja2 engine:

```
from flask import Flask, render_template_string
app = Flask(__name__)

@app.route("/profile/<user>")
def profile(user):
    template = f"<h1>Welcome to {user}'s profile!</h1>"
    return render_template_string(template)

app.run()
```

**Warning:** If user input is concatenated directly into the template string (like above), it may lead to SSTI.

## How to Detect SSTI?

- **Find Injection Points:** Look for places where user input is inserted into templates, such as URL parameters, form inputs, or headers.
- **Fuzzing:** Test by injecting template syntax characters like `{{`, `}}`, `{%`, `%}`, or payloads like `{{7*7}}` to see if the server evaluates them.
- **Check Outputs:** If the response contains evaluated expressions or errors related to template syntax, SSTI may be present.

## How to Identify the Template Engine?

- Observe error messages or outputs for clues about the template engine type.
- Use crafted payloads adapting to popular engines like Jinja2, Twig, Velocity, etc.
- Follow decision trees available online or in security resources to confirm the engine.

## Understanding Template Syntax (Example: Jinja2)

- Print variables or output: `{{ variable }}`
- Control structures (loops, conditions): `{% statement %}`
- Comments: `{# comment #}`

Refer to the [Jinja2 documentation](https://jinja.palletsprojects.com/en/2.11.x/) for full syntax details.

## Exploiting SSTI

- Inject payloads within user input fields that the server renders.
- Use Python introspection in Jinja2 to access internal objects and execute commands.

Example payload that runs shell commands in Jinja2:

```
{{ ''.__class__.__mro__.__subclasses__()('whoami', shell=True, stdout=-1).communicate() }}[11]
```

## How Template Processing Works

- The template engine substitutes placeholders with evaluated code.
- For example, input `{{7 * 7}}` evaluates to `49` in the output.
- If user input is unsafely evaluated, it opens the door for SSTI.

## Preventing SSTI

- **Never concatenate user input directly into templates.**
- Pass user data safely as variables:

  ```
  template = "<h1>Welcome to {{ user }}'s profile!</h1>"
  return render_template_string(template, user=user)
  ```

- Sanitize all user input rigorously, whitelist allowed characters.

  ```
  import re
  user = re.sub("[^A-Za-z0-9]", "", user)
  ```

- Regularly review template engine documentation and security advisories.

## Real-World Example: HackerOne Bug Bounty

- In 2016, an SSTI was found on an Uber subdomain related to profile name changes.
- The attacker controlled input reflected in emails.
- Even without remote code execution, the vulnerability earned a $10,000 bounty.
- [Read full report](https://hackerone.com/reports/125980)

## Additional Resources

- Extensive payload collection: [PayloadsAllTheThings SSTI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
- PortSwigger Web Security Academy: [SSTI Tutorial](https://portswigger.net/web-security/server-side-template-injection)

---

*Use this guide responsibly for learning and authorized security testing.*
