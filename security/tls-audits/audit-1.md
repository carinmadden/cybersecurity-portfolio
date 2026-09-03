# TLS/Certificate Audit — Multi-Site Comparison

**Sites audited:** dh480.badssl.com · untrusted-root.badssl.com · www.google.com · kernel.org
**Method:** SSL Labs (SSL Server Test) + manual protocol/cipher review

---

## 🔴 dh480.badssl.com

| | |
|---|---|
| **Grade** | F |
| **Severity** | High |
| **Certificate** | Valid, expires October 26, 2026 |

**Protocols in use:** TLS 1.2, TLS 1.1 ⚠️, TLS 1.0 ⚠️
**Deprecated protocols:** TLS 1.1, TLS 1.0

**Weak ciphers:**

| Cipher Suite | Bits |
|---|---|
| TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 | 128 |
| TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 | 256 |
| TLS_DHE_RSA_WITH_AES_128_CBC_SHA256 | 128 |
| TLS_DHE_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_DHE_RSA_WITH_AES_256_CBC_SHA256 | 256 |
| TLS_DHE_RSA_WITH_AES_256_CBC_SHA | — |

**What an attacker can do:**
Because the server uses crackable 480-bit Diffie-Hellman, an attacker can break the encryption, decrypt all traffic, and impersonate the server — making full man-in-the-middle interception trivial.

**Remediation:**
Disable DHE entirely, remove TLS 1.0/1.1, and allow only modern ECDHE + AES-GCM/ChaCha20 cipher suites so attackers can no longer break the key exchange or downgrade the connection.

---

## 🔴 untrusted-root.badssl.com

| | |
|---|---|
| **Grade** | T |
| **Severity** | High |
| **Certificate** | Valid, expires August 31, 2028 |

**Protocols in use:** TLS 1.2, TLS 1.1 ⚠️, TLS 1.0 ⚠️
**Deprecated protocols:** TLS 1.1, TLS 1.0

**Weak ciphers:**

| Cipher Suite | Bits |
|---|---|
| TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 | 128 |
| TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 | — |
| TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 | 128 |
| TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 | 256 |
| TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_DHE_RSA_WITH_AES_128_CBC_SHA256 | 128 |
| TLS_DHE_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_DHE_RSA_WITH_AES_256_CBC_SHA256 | 256 |
| TLS_DHE_RSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA | 112 |
| TLS_RSA_WITH_AES_128_GCM_SHA256 | 128 |
| TLS_RSA_WITH_AES_256_GCM_SHA384 | 256 |
| TLS_RSA_WITH_AES_128_CBC_SHA256 | 128 |
| TLS_RSA_WITH_AES_256_CBC_SHA256 | 256 |
| TLS_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_RSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_RSA_WITH_3DES_EDE_CBC_SHA | 112 |
| TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA | 256 |
| TLS_RSA_WITH_CAMELLIA_256_CBC_SHA | 256 |
| TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA | 128 |
| TLS_RSA_WITH_CAMELLIA_128_CBC_SHA | 128 |

**What an attacker can do:**
Full man-in-the-middle attacks — downgrade to TLS 1.0/1.1, exploit RSA key-exchange suites, exploit CBC cipher suites, and exploit 3DES (Sweet32).

**Remediation:**
Disable TLS 1.0/1.1, remove RSA, CBC, 3DES, and Camellia suites, and replace the certificate with one signed by a trusted certificate authority. Enable ECDHE, GCM, and TLS 1.3.

---

## 🟡 www.google.com

| | |
|---|---|
| **Grade** | B |
| **Severity** | Medium |
| **Certificate** | Valid, expires November 2, 2026 |

**Protocols in use:** TLS 1.3, TLS 1.2, TLS 1.1 ⚠️, TLS 1.0 ⚠️
**Deprecated protocols:** TLS 1.1, TLS 1.0

**Weak ciphers:**

| Cipher Suite | Bits |
|---|---|
| TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_RSA_WITH_AES_128_GCM_SHA256 | 128 |
| TLS_RSA_WITH_AES_256_GCM_SHA384 | 256 |
| TLS_RSA_WITH_AES_128_CBC_SHA | 128 |
| TLS_RSA_WITH_AES_256_CBC_SHA | 256 |
| TLS_RSA_WITH_3DES_EDE_CBC_SHA | — |

**What an attacker can do:**
Could attempt to downgrade to TLS 1.0/1.1, force CBC cipher suite negotiation, exploit RSA key exchange suites, or attempt a 3DES downgrade.

**Remediation:**
Nothing critical to fix — Google is aware of these legacy suites and keeps them enabled intentionally for backward compatibility. The site itself is not insecure.

---

## 🟢 kernel.org

| | |
|---|---|
| **Grade** | A+ |
| **Severity** | Low |
| **Certificate** | Valid, expires November 12, 2026 |

**Protocols in use:** TLS 1.3, TLS 1.2
**Deprecated protocols:** None
**Weak ciphers:** None

**What an attacker can do:**
Nothing meaningful — the configuration is fully hardened. The only realistic attack paths left are social engineering, client-side compromise, or DNS spoofing.

**Remediation:**
None needed — the site is fully hardened.

---

## 📊 Comparison Summary

| Site | Grade | Severity | Legacy Protocols | Notes |
|---|:---:|:---:|:---:|---|
| dh480.badssl.com | F | High | TLS 1.0 / 1.1 | Crackable 480-bit DHE key exchange |
| untrusted-root.badssl.com | T | High | TLS 1.0 / 1.1 | Untrusted cert + weak/legacy cipher support |
| www.google.com | B | Medium | TLS 1.0 / 1.1 | Legacy suites kept intentionally for compatibility |
| kernel.org | A+ | Low | None | Fully modern, no deprecated protocols or ciphers |

**Takeaway:**
Well-configured sites stay safe by removing old TLS versions, disabling weak ciphers, and using certificates browsers actually trust — allowing only modern encryption like ECDHE with AES-GCM or ChaCha20. Poorly configured sites leave outdated protocols (TLS 1.0/1.1) and weak ciphers (DHE, CBC, 3DES) in place, which attackers can break or downgrade. Clean certificate chains matter too — untrusted or misconfigured certificates make impersonation easier. Overall, strong TLS security requires ongoing maintenance: cryptography becomes unsafe over time if configurations are never revisited, and the poorly configured sites here show exactly what happens when that maintenance never happens.



