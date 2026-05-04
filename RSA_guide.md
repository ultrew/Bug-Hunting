# ⚡ RSA Ultra-Quick Revision

## Core Idea
- Public key → (e, n)  
- Private key → (d, n)  
- Encrypt:  
  \[
  c = m^e \bmod n
  \]
- Decrypt:  
  \[
  m = c^d \bmod n
  \] 
:contentReference[oaicite:0]{index=0}

---

## Key Generation
1. Pick primes: p, q  
2. Compute:
   - \( n = p \cdot q \)
   - \( \phi = (p-1)(q-1) \)
3. Choose:
   - \( 1 < e < \phi \), gcd(e, φ)=1  
4. Compute:
   - \( d \equiv e^{-1} \mod \phi \)

---

## Minimal Example
- p=13, q=9  
- n=117, φ=96  
- e=11 → d=35  

Encrypt:
\[
10^{11} \mod 117 = 82
\]

Decrypt:
\[
82^{35} \mod 117 = 10
\] 
:contentReference[oaicite:1]{index=1}

---

## Practical Decryption
```python
m = pow(c, d, n)
