Good catch. Your issue is formatting, not concepts. Git (and most markdown renderers) will break when you mix LaTeX-style symbols.

Here is the same content in **clean, plain text (Git-safe, no symbols):**

---

# RSA Quick Notes (Plain Text Version)

## Core Idea

RSA is an asymmetric encryption system:

* Public key is used to encrypt
* Private key is used to decrypt
* Everything works on integers using modular arithmetic

---

## Variables

- p, q = prime numbers

- n = p * q (modulus)

- phi = (p - 1) * (q - 1)

- e = public exponent

- d = private exponent

- m = plaintext (as integer)

- c = ciphertext

---

## Core Equations

Encryption:
c = (m ^ e) mod n

Decryption:
m = (c ^ d) mod n

---

## Key Generation

1. Choose two prime numbers:
   p, q

2. Compute modulus:
   n = p * q

3. Compute totient:
   phi = (p - 1) * (q - 1)

4. Choose e such that:
   1 < e < phi
   gcd(e, phi) = 1

5. Compute d:
   d = modular inverse of e mod phi

---

## Minimal Example

p = 13
q = 9

n = 117
phi = 96

e = 11
d = 35

Encryption:
c = (10 ^ 11) mod 117 = 82

Decryption:
m = (82 ^ 35) mod 117 = 10

---

## Practical Workflow (CTF)

If p and q are known:

```python
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m = pow(c, d, n)
```

---

## Validation (Do Not Skip)

Check d is correct:
(d * e) mod phi must equal 1

If not → everything is wrong

---

## Convert Result to Text

```python
m_bytes = m.to_bytes((m.bit_length() + 7) // 8, 'big')
print(m_bytes)
```

---

## Padding (Important)

If output looks random:

Likely format:
00 02 random_bytes 00 actual_message

You must remove everything before the second 00

---

## Common Mistakes

* Writing n incorrectly (it is p * q, not a pair)
* Wrong phi value
* Wrong d value
* Using floats instead of integers
* Ignoring padding

---

## Mental Model

RSA flow:
p, q → n → phi → d → decrypt → bytes → remove padding → message

---

## One-Line Summary

RSA = modular exponentiation + modular inverse + decoding

---

If Git breaks this, the problem is not RSA — it's your formatting discipline.
