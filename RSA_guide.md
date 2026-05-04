## ⚡ RSA — Ultra-Fast Revision Notes (CTF / Practical Focus)

---

## 🔑 Core Concept (Don’t Overthink It)

* Asymmetric crypto = **2 keys**

  * Public → encrypt
  * Private → decrypt
* RSA works entirely on **integers + modular arithmetic** 

---

## 🧠 Variables You Must Recognize Instantly

| Symbol      | Meaning                 |
| ----------- | ----------------------- |
| ( p, q )    | large primes            |
| ( n )       | modulus = ( p \cdot q ) |
| ( \phi(n) ) | Euler totient           |
| ( e )       | public exponent         |
| ( d )       | private exponent        |
| ( m )       | message                 |
| ( c )       | ciphertext              |

---

## 📐 Core Equations (Non-Negotiable)

### Encryption

[
c = m^e \bmod n
]

### Decryption

[
m = c^d \bmod n
]

---

## ⚙️ Key Generation (Burn This Into Memory)

1. Choose primes:
   [
   p, q
   ]

2. Compute modulus:
   [
   n = p \cdot q
   ]

3. Compute totient:
   [
   \phi(n) = (p-1)(q-1)
   ]

4. Choose:
   [
   1 < e < \phi(n), \quad \gcd(e, \phi) = 1
   ]

5. Compute inverse:
   [
   d = e^{-1} \bmod \phi(n)
   ]

---

## ⚡ Minimal Example (Understand Structure)

From source :

* ( p = 13,; q = 9 ) *(toy, insecure)*
* ( n = 117 )
* ( \phi = 96 )
* ( e = 11 )
* ( d = 35 )

### Encrypt

[
10^{11} \bmod 117 = 82
]

### Decrypt

[
82^{35} \bmod 117 = 10
]

---

## 🧪 Real Attack Workflow (CTF Mindset)

### Case 1: You have p, q (like your problem)

You **already win**:

```python
phi = (p-1)*(q-1)
d = pow(e, -1, phi)
m = pow(c, d, n)
```

---

### Case 2: Small n → factor it

Use:

* [https://www.alpertron.com.ar/ECM.HTM](https://www.alpertron.com.ar/ECM.HTM)
* [https://factordb.com](https://factordb.com)

Then proceed normally.

---

### Case 3: Extract n,e from cert

```bash
openssl x509 -in cert.pem -text -noout
```

---

## 🔍 Critical Sanity Checks (Most People Skip → Fail)

### Check 1: d correctness

[
(d \cdot e) \bmod \phi(n) = 1
]

If false → everything is garbage.

---

### Check 2: Plaintext extraction

```python
m_bytes = m.to_bytes((m.bit_length()+7)//8, 'big')
print(m_bytes)
```

---

## 🧱 Padding Awareness (Where You’ll Get Trapped)

If output looks random:

* Likely **PKCS#1 v1.5 padding**

Pattern:

```
00 02 [random] 00 [actual message]
```

👉 Strip everything before last `00`

---

## ⚠️ Common Failure Modes (Brutal Reality)

| Mistake                          | Result                      |
| -------------------------------- | --------------------------- |
| Wrong φ(n)                       | Completely wrong plaintext  |
| Wrong d                          | Garbage output              |
| Ignoring padding                 | Looks random                |
| Using floats/scientific notation | Catastrophic precision loss |
| Trying manual exponentiation     | Waste of time               |

---

## ⚡ Your Exact Problem (What You Did Right/Wrong)

You:

* ✔ Factored n
* ✔ Computed d
* ✔ Decrypted

Now:

* ❗ You stopped at integer `m` → **halfway done**

---

## 🔬 Final Step You Must Always Do

```python
m = 310924024341754586049069014240097859710998385184245570453341488382426297925855600
print(m.to_bytes((m.bit_length()+7)//8, 'big'))
```

---

## 🌐 Useful Tools (Speed > Ego)

* Factorization:

  * [https://factordb.com](https://factordb.com)
  * [https://www.alpertron.com.ar/ECM.HTM](https://www.alpertron.com.ar/ECM.HTM)

* Modular inverse:

  * [https://planetcalc.com/3298/](https://planetcalc.com/3298/)

* Python (best tool, no debate)

---

## 🧠 Mental Model (Compress Everything)

RSA =

> “Raise to exponent mod n — twice — with inverse relationship”

Security =

> “Factoring n must be infeasible”

---

## 🧨 If You Want to Level Up (Next Targets)

* Small exponent attack (e=3)
* Wiener's attack (small d)
* Common modulus attack
* Broadcast attack
* Padding oracle

---

## 🧾 One-Line Summary

> RSA breaks into: **factor → φ → inverse → pow → decode**

---

If your byte output still looks random, your math is wrong. Don’t guess—verify each step.
