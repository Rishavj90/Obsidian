# Network Security Exam Notes: Hash Algorithms and Digital Signatures

## 1. Hash Algorithms

### 1.1 What is a hash algorithm?

A **hash algorithm** is a mathematical function that converts input data of arbitrary length into a fixed-length value called a **hash value**, **hash code**, or **digest**.

```text
Input message of any length
          |
          v
     Hash Algorithm
          |
          v
Fixed-length hash/digest
```

For example, a hash function can take a short message, a large file, or an entire software package and produce a fixed-size digest.

A cryptographic hash function is designed so that the digest can be used to verify the integrity of data without revealing the original message.

### 1.2 Basic properties of cryptographic hash functions

A good cryptographic hash function should have the following properties:

1. **Deterministic**
   - The same input always produces the same hash.
   - `H(M) = H(M)` every time.

2. **Fixed-length output**
   - Regardless of the input size, the output has a predefined length.
   - MD5: 128 bits
   - SHA-1: 160 bits
   - SHA-256: 256 bits

3. **Efficient to compute**
   - Given a message, its hash should be relatively quick to calculate.

4. **Preimage resistance**
   - Given a hash `h`, it should be computationally infeasible to find a message `M` such that:
     `H(M) = h`.

5. **Second-preimage resistance**
   - Given a message `M1`, it should be computationally infeasible to find a different message `M2` such that:
     `H(M1) = H(M2)`.

6. **Collision resistance**
   - It should be computationally infeasible to find any two different messages `M1` and `M2` for which:
     `H(M1) = H(M2)`.

7. **Avalanche effect**
   - A tiny change in the input should cause a large and apparently random change in the output.

### 1.3 Hashing vs encryption

Hashing and encryption are different.

| Hashing | Encryption |
|---|---|
| Usually one-way | Designed to be reversible with a key |
| Produces a fixed-size digest | Produces ciphertext related to plaintext |
| Mainly used for integrity and authentication-related functions | Mainly used for confidentiality |
| No decryption operation | Decryption recovers plaintext |
| Examples: SHA-256, SHA-3 | Examples: AES, RSA |

A hash is **not encryption**. You cannot normally "decrypt" a cryptographic hash.

### 1.4 Hash function example

Suppose a message is:

```text
HELLO
```

A hash function processes the message and produces a digest such as:

```text
H(HELLO) = <fixed-length hexadecimal value>
```

If the message changes to:

```text
HELLo
```

the resulting digest should be drastically different.

This makes hashes useful for detecting accidental or malicious modification.

### 1.5 Major applications

Hash algorithms are used for:

- Data integrity checking
- Digital signatures
- Message authentication constructions such as HMAC
- File and software integrity verification
- Certificate and public-key infrastructure operations
- Password storage when used with an appropriate password-hashing scheme
- Blockchain and distributed systems
- Deduplication and content identification

**Exam point:** A plain fast hash such as SHA-256 should not be used directly for password storage. Passwords require specialized password-hashing functions such as Argon2, scrypt, or bcrypt.

---

# 2. Message Digest Algorithms

The term **Message Digest** commonly refers to cryptographic hash algorithms that generate a compact fixed-length representation of a message.

Two historically important algorithms are **MD4** and **MD5**.

## 2.1 MD4

**MD4 (Message-Digest Algorithm 4)** was designed by Ronald Rivest in 1990.

### Main characteristics

- Digest size: **128 bits**
- Processes data in **512-bit blocks**
- Uses a **32-bit word** structure
- Designed primarily for speed
- Served as a basis for later algorithms such as MD5

### General operation

MD4 follows a typical iterative hash structure:

```text
Message
   |
Padding
   |
512-bit blocks
   |
Compression function
   |
Internal state
   |
128-bit digest
```

The message is padded and divided into 512-bit blocks. Each block is processed through multiple rounds that update an internal state.

### Security status

MD4 is **cryptographically broken** and should not be used for security-sensitive applications.

Its weaknesses include practical collision attacks and other cryptanalytic weaknesses.

**Exam point:** MD4 is historically important but obsolete for modern security.

---

# 3. MD5

**MD5 (Message-Digest Algorithm 5)** was also designed by Ronald Rivest and published in 1992 as an improvement over MD4.

### Main characteristics

- Digest size: **128 bits**
- Block size: **512 bits**
- Word size: **32 bits**
- Uses **four rounds**
- Produces a 128-bit message digest

### 3.1 MD5 processing

The basic process is:

```text
Input message
     |
     v
Padding
     |
     v
Append message length
     |
     v
Divide into 512-bit blocks
     |
     v
Initialize 4-word state
     |
     v
Four rounds of compression
     |
     v
128-bit MD5 digest
```

The internal state consists of four 32-bit registers:

```text
A, B, C, D
```

Together:

```text
4 × 32 bits = 128 bits
```

Each 512-bit block is processed through four rounds involving nonlinear functions, modular addition, rotations, and message words.

### 3.2 MD5 padding

Before processing:

1. A `1` bit is appended.
2. Enough `0` bits are appended so that the message length is 448 bits modulo 512.
3. The original message length is appended as a 64-bit value.

Thus, each padded message has a length that is a multiple of 512 bits.

### 3.3 Security weaknesses of MD5

MD5 is **not collision resistant**.

Researchers demonstrated practical collision attacks, meaning an attacker can construct different inputs having the same MD5 digest.

Therefore MD5 should not be used for:

- Digital signatures
- Certificate signatures
- Security-critical integrity protection
- New cryptographic protocols

It can still appear in legacy systems or non-adversarial checksums, but it should not be selected for new security designs.

### 3.4 MD4 vs MD5

| Feature | MD4 | MD5 |
|---|---:|---:|
| Designer | Ronald Rivest | Ronald Rivest |
| Digest | 128 bits | 128 bits |
| Block size | 512 bits | 512 bits |
| Rounds | 3 | 4 |
| Speed | Very fast | Faster than many older hashes, but slower than MD4 |
| Security | Broken | Broken |
| Modern use | Not recommended | Not recommended |

---

# 4. Secure Hash Algorithms

The **SHA (Secure Hash Algorithm)** family was designed to provide stronger security than older algorithms such as MD4 and MD5.

The important families for exams are:

- SHA-1
- SHA-2

## 4.1 SHA-1

**SHA-1** was developed by the U.S. National Security Agency and published by NIST.

### Characteristics

- Digest size: **160 bits**
- Block size: **512 bits**
- Uses **32-bit words**
- Processes the message through **80 rounds**
- Produces a 160-bit message digest

The internal state contains five 32-bit words:

```text
A, B, C, D, E
```

Therefore:

```text
5 × 32 = 160 bits
```

### SHA-1 processing

```text
Message
   |
Padding
   |
512-bit blocks
   |
Message schedule
   |
80 compression rounds
   |
Updated internal state
   |
160-bit digest
```

### Security problem

SHA-1 is considered **cryptographically broken for collision resistance**. Practical collision attacks have been demonstrated.

Therefore SHA-1 should not be selected for new security applications requiring collision resistance.

**Important distinction:** The fact that SHA-1 is broken for collisions does not mean that every possible use of SHA-1 instantly becomes an easy preimage attack. The main reason it has been retired is its inadequate collision resistance.

---

# 5. SHA-2

**SHA-2** is a family of hash algorithms standardized by NIST. It includes:

- SHA-224
- SHA-256
- SHA-384
- SHA-512
- SHA-512/224
- SHA-512/256

The most commonly encountered member is **SHA-256**.

## 5.1 SHA-256

### Characteristics

- Digest size: **256 bits**
- Block size: **512 bits**
- Word size: **32 bits**
- Number of rounds: **64**
- Internal state: eight 32-bit words

```text
8 × 32 = 256 bits
```

### SHA-256 processing

```text
Input message
      |
      v
Padding + length
      |
      v
512-bit message blocks
      |
      v
Message schedule
      |
      v
64 compression rounds
      |
      v
Eight-word internal state
      |
      v
256-bit digest
```

### 5.2 SHA-512

SHA-512 has:

- 512-bit digest
- 1024-bit block size
- 64-bit words
- 80 rounds
- Eight 64-bit internal state words

```text
8 × 64 = 512 bits
```

### 5.3 SHA-2 comparison

| Algorithm | Digest | Block size | Word size | Rounds |
|---|---:|---:|---:|---:|
| SHA-224 | 224 bits | 512 bits | 32 bits | 64 |
| SHA-256 | 256 bits | 512 bits | 32 bits | 64 |
| SHA-384 | 384 bits | 1024 bits | 64 bits | 80 |
| SHA-512 | 512 bits | 1024 bits | 64 bits | 80 |

SHA-2 remains widely used for cryptographic applications.

---

# 6. MD5 vs SHA-1 vs SHA-2

| Property | MD5 | SHA-1 | SHA-256 |
|---|---:|---:|---:|
| Digest size | 128 bits | 160 bits | 256 bits |
| Block size | 512 bits | 512 bits | 512 bits |
| Collision resistance | Broken | Broken | Considered secure for current general use |
| Recommended for new security systems? | No | No | Yes, where appropriate |
| Common historical use | Checksums, legacy systems | Legacy signatures/certificates | TLS, signatures, integrity, HMAC, etc. |

**Exam conclusion:** MD4 and MD5 are obsolete; SHA-1 is also obsolete for collision-resistant cryptographic use; SHA-2 is the important modern family among the algorithms in this topic.

---

# 7. Digital Signatures

## 7.1 Definition

A **digital signature** is a cryptographic mechanism that provides evidence that a digital message or document was signed by the holder of a private key and has not been modified after signing.

A digital signature primarily provides:

1. **Authentication** — identifies the signing key/person or organization, subject to proper key management.
2. **Integrity** — detects modification of the signed data.
3. **Non-repudiation** — in some legal/organizational contexts, a signature can support attribution, but cryptography alone does not guarantee a legal finding of non-repudiation.

### Digital signature concept

```text
                SIGNING
Message ----------------------+
   |                           |
   v                           |
Hash function                  |
   |                           |
   v                           |
Message Digest                 |
   |                           |
   v                           |
Private Key + Signature Algorithm
   |
   v
Digital Signature
```

The signature is normally generated over a hash of the message rather than the entire message directly.

---

# 8. Digital Signature Generation

Suppose Alice wants to digitally sign message `M`.

### Step 1: Hash the message

```text
h = H(M)
```

For example:

```text
M → SHA-256 → h
```

### Step 2: Use Alice's private key

Alice's digital-signature algorithm uses her private key to generate:

```text
S = Sign(private key, h)
```

The result `S` is the digital signature.

### Step 3: Send the message and signature

Alice sends:

```text
M + S
```

A certificate may also be supplied so that Bob can obtain and validate Alice's public key.

---

# 9. Digital Signature Verification

Bob receives the message and signature.

### Step 1: Hash the received message

```text
h1 = H(M)
```

### Step 2: Verify the signature using Alice's public key

```text
Verify(public key, M, S)
```

The signature algorithm checks whether the signature corresponds to the message and the public key.

If verification succeeds:

```text
Signature valid
```

This provides evidence that:

- The signature was generated using the corresponding private key.
- The signed data is consistent with the signature.

If the message was modified, verification should fail.

---

# 10. Why Hashing Is Used with Digital Signatures

Signing a large message directly with a public-key algorithm can be inefficient.

Instead:

```text
Large message
     |
     v
Hash function
     |
     v
Small fixed-size digest
     |
     v
Digital signature algorithm
```

Advantages:

- Faster processing
- Fixed-size input to the signing operation
- Efficient handling of large files
- Strong integrity protection when a secure hash and secure signature scheme are used correctly

**Important:** The security of the overall construction depends on both the signature scheme and the hash/signature encoding rules.

---

# 11. Digital Signature Example

Suppose Alice wants to sign:

```text
Transfer ₹10,000 to Bob
```

Alice calculates:

```text
h = SHA-256(message)
```

Then:

```text
S = Sign(Alice's private key, h)
```

She sends:

```text
Message + S
```

Bob uses Alice's public key to verify the signature.

If an attacker changes:

```text
₹10,000
```

to:

```text
₹90,000
```

the message hash changes. The original signature will no longer verify against the modified message.

---

# 12. Digital Signature vs MAC

Digital signatures and Message Authentication Codes (MACs) both provide integrity and authentication, but their key models differ.

| Digital Signature | MAC |
|---|---|
| Uses private/public key pair | Uses shared secret key |
| Sender signs with private key | Sender computes MAC with shared secret |
| Anyone with the public key can verify | Only parties possessing the secret can verify |
| Useful for public verification | Useful for closed groups/shared-secret systems |
| Common in certificates and signed documents | Common in network protocols and API/message authentication |

---

# 13. Digital Signature Standards

A **digital signature standard** specifies algorithms, formats, parameters, and procedures for creating and verifying digital signatures.

Important standards and schemes include:

- DSS
- DSA
- RSA-based signatures
- ECDSA
- EdDSA
- Modern standardized signature schemes such as ML-DSA

For a traditional Network Security exam, **DSS and DSA** are particularly important.

---

# 14. Digital Signature Standard (DSS)

The **Digital Signature Standard (DSS)** is a NIST standard for digital signatures.

The original DSS specified **DSA (Digital Signature Algorithm)** as the signature algorithm.

The DSS family has evolved over time and can accommodate multiple approved signature algorithms.

## 14.1 DSA

DSA is a public-key digital signature algorithm based on the discrete logarithm problem.

It uses:

- A private key for signing
- A public key for verification
- A cryptographic hash function
- Domain parameters

### Simplified DSA process

```text
Message
   |
   v
Hash
   |
   v
Signature generation
using private key + random per-signature value
   |
   v
(r, s)
```

The DSA signature consists of two integers:

```text
(r, s)
```

The verifier uses:

```text
Message
Public key
Signature (r, s)
Domain parameters
```

to determine whether the signature is valid.

---

# 15. DSA Key Components

DSA uses domain parameters commonly represented as:

```text
p, q, g
```

where:

- `p` — a large prime
- `q` — a prime divisor related to `p - 1`
- `g` — a generator
- `x` — private key
- `y` — public key

The public key is derived from the private key using the domain parameters.

Conceptually:

```text
Private key x
     |
     v
DSA mathematical operation
     |
     v
Public key y
```

The private key must remain secret.

---

# 16. DSA Signature Generation — Exam-Level View

Given message `M`:

### Step 1: Hash the message

```text
H(M)
```

### Step 2: Choose a fresh secret per-signature value

A secret value `k` is selected for that particular signature.

### Step 3: Compute signature values

DSA mathematically derives:

```text
r = function(p, q, g, k)

s = function(k, H(M), x, r, q)
```

The final signature is:

```text
(r, s)
```

The exact equations are less important than understanding the role of the values.

### Critical security requirement

The per-signature secret `k` must be unpredictable and **must never be reused**.

If `k` is reused or exposed, the private signing key can potentially be recovered.

---

# 17. DSA Signature Verification — Exam-Level View

The verifier has:

```text
M
(r, s)
Public key y
p, q, g
```

The verifier:

1. Checks that `r` and `s` are in the valid range.
2. Computes the hash of the message.
3. Performs DSA verification calculations.
4. Obtains a verification value.
5. Accepts the signature if the calculated value matches `r`.

Conceptually:

```text
Message + Signature + Public Key
              |
              v
       DSA Verification
              |
        +-----+-----+
        |           |
      Valid       Invalid
```

---

# 18. RSA Digital Signatures

RSA can also be used for digital signatures.

Conceptually:

```text
Message
   |
   v
Hash + signature encoding
   |
   v
RSA signing with private key
   |
   v
Signature
```

Verification uses the corresponding public key.

Modern RSA signatures should use standardized secure padding/encoding such as **RSA-PSS**, rather than obsolete textbook RSA signing.

---

# 19. ECDSA

**ECDSA (Elliptic Curve Digital Signature Algorithm)** is a digital signature scheme based on elliptic-curve cryptography.

Advantages include:

- Smaller public keys than traditional RSA for comparable security levels
- Smaller signatures in many configurations
- Efficient use in constrained environments

ECDSA uses:

- Private key
- Public key
- Elliptic-curve domain parameters
- Cryptographic hash
- Per-signature secret value

As with DSA, safe generation and handling of the per-signature secret is critical.

---

# 20. Modern Perspective: EdDSA and Post-Quantum Signatures

Modern systems also use signature schemes such as **EdDSA**, including Ed25519.

Post-quantum cryptography introduces signature schemes designed to resist attacks from sufficiently powerful quantum computers. One important standardized family is **ML-DSA**, based on lattice cryptography.

For most traditional exams, however, the key concepts remain:

```text
Hash → Sign with private key → Verify with public key
```

---

# 21. Important Terms for the Exam

### Integrity

Ensures that data has not been modified without detection.

### Authentication

Provides evidence about the origin or signer of data.

### Non-repudiation

A broader property in which a signer cannot successfully deny an action/signature under the applicable technical, organizational, and legal framework.

### Private key

Secret key used for signing.

### Public key

Key distributed to others for signature verification.

### Message digest

Fixed-size output of a cryptographic hash function.

### Collision

When two different messages produce the same hash:

```text
M1 ≠ M2
but
H(M1) = H(M2)
```

### Certificate

A digitally signed data structure that binds an identity or subject to a public key. In common PKI systems, certificates are issued and signed by a Certificate Authority (CA).

---

# 22. Complete Digital Signature Workflow

```text
                    SENDER
                      |
                Original Message
                      |
                      v
                Hash Function
                      |
                      v
                 Message Hash
                      |
                      v
                Private Key
                      |
                      v
              Digital Signature
                      |
                      |
       +--------------+--------------+
       |                             |
       v                             v
Original Message              Digital Signature
       |                             |
       +--------------+--------------+
                      |
                      v
                    SEND
                      |
                      v
                   RECEIVER
                      |
            +---------+---------+
            |                   |
            v                   v
       Hash Message        Verify Signature
            |                   |
            v                   v
          Hash 1              Result
            |                   |
            +---------+---------+
                      |
                      v
               Valid / Invalid
```

---

# 23. Key Differences: Hashing and Digital Signature

| Feature | Hash | Digital Signature |
|---|---|---|
| Main purpose | Integrity / digest generation | Authentication + integrity + signature evidence |
| Keys required | No key for a plain hash | Private and public keys |
| Output | Digest | Signature |
| Verification | Recalculate/compare hash | Verify using public key |
| Example | SHA-256 | RSA-PSS, ECDSA, Ed25519, DSA |
| Public verification | Not applicable to plain hashing | Yes |

---

# 24. Security Status to Remember

For an exam, memorize this classification:

```text
MD4   → Broken / obsolete
MD5   → Broken for collision resistance / obsolete
SHA-1 → Broken for collision resistance / deprecated
SHA-2 → Widely used and considered secure for current general-purpose applications
```

Do not confuse **collision resistance** with every other security property. An algorithm can have a known collision attack without having an equally practical preimage attack.

---

# 25. Frequently Asked Exam Questions

## Q1. What is a hash algorithm?

A hash algorithm is a mathematical function that maps an arbitrary-length input to a fixed-length output called a hash value or message digest. Cryptographic hash functions are designed to provide properties such as preimage resistance, second-preimage resistance, and collision resistance.

## Q2. What are the properties of a good cryptographic hash function?

- Deterministic
- Efficient
- Fixed-length output
- Preimage resistant
- Second-preimage resistant
- Collision resistant
- Strong avalanche effect

## Q3. What is MD5?

MD5 is a 128-bit message-digest algorithm that processes 512-bit blocks using four rounds of operations. It is historically important but is no longer considered secure because practical collision attacks exist.

## Q4. What is SHA-1?

SHA-1 is a cryptographic hash algorithm producing a 160-bit digest from 512-bit blocks. It uses 80 rounds. SHA-1 is deprecated for security-sensitive applications because practical collision attacks have been demonstrated.

## Q5. What is SHA-2?

SHA-2 is a family of secure hash algorithms including SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256. SHA-256 is one of the most widely used members.

## Q6. What is a digital signature?

A digital signature is a cryptographic mechanism in which a signer uses a private key to create a signature over data, normally using a secure hash and a signature scheme. A verifier uses the corresponding public key to verify the signature.

## Q7. What security services does a digital signature provide?

Primarily:

- Integrity
- Signer authentication/key-based attribution
- Support for non-repudiation in appropriate systems

It does **not** by itself provide confidentiality.

## Q8. Why is hashing used before digital signing?

Hashing reduces an arbitrarily large message to a fixed-size digest, making public-key signature operations more efficient while preserving strong integrity protection when secure algorithms are used.

## Q9. What is DSS?

DSS stands for **Digital Signature Standard**, a NIST standard for digital signatures. Historically, DSS specified DSA and has evolved to recognize additional approved signature algorithms.

## Q10. What is the difference between a digital signature and encryption?

Encryption primarily provides confidentiality. A digital signature primarily provides integrity and authentication/signature evidence. Encryption uses keys to transform plaintext into ciphertext, while a signature uses a private key to produce a value that can be verified using a public key.

---

# 26. Short Revision Sheet

### Hash Algorithm

```text
Input → Hash Function → Fixed-size Digest
```

Purpose:

```text
Integrity
```

### MD4

```text
128-bit digest
512-bit block
3 rounds
Broken
```

### MD5

```text
128-bit digest
512-bit block
4 rounds
Collision attacks
Obsolete
```

### SHA-1

```text
160-bit digest
512-bit block
80 rounds
Collision attacks
Deprecated
```

### SHA-256

```text
256-bit digest
512-bit block
64 rounds
Part of SHA-2
Widely used
```

### Digital Signature

```text
Message
   ↓
Hash
   ↓
Sign hash using Private Key
   ↓
Digital Signature
```

Verification:

```text
Message + Signature
       ↓
Hash + Public-key Verification
       ↓
Valid / Invalid
```

### DSS / DSA

```text
DSS = Digital Signature Standard
DSA = Digital Signature Algorithm
```

DSA signature:

```text
(r, s)
```

DSA uses:

```text
Private key → Signing
Public key  → Verification
```

---

# 27. One-Minute Exam Memory Trick

Remember:

```text
MD4 → MD5 → SHA-1 → SHA-2
  ↓      ↓      ↓       ↓
old    old    broken   current family
128    128    160      224/256/384/512
```

And:

```text
HASH:
Message → Digest
       ↓
   Integrity

SIGNATURE:
Message → Hash → Private Key → Signature
                                  ↓
                         Public Key verifies
```

The most important conceptual distinction is:

> **A hash proves consistency of data; a digital signature additionally ties that data to a private signing key that can be verified using the corresponding public key.**

---

# 28. Final Exam Summary

A cryptographic hash algorithm converts data of arbitrary length into a fixed-length digest and is primarily used to detect changes and support other cryptographic mechanisms. MD4 and MD5 are older message-digest algorithms and are no longer suitable for modern security because of serious weaknesses, especially collision attacks. SHA-1 produces a 160-bit digest but is also deprecated for collision-resistant security applications. SHA-2, including SHA-256 and SHA-512, provides the modern hash family covered in this topic and remains widely deployed.

A digital signature uses asymmetric cryptography. The signer uses a **private key** to create a signature, normally over a cryptographic hash of the message. The receiver uses the corresponding **public key** to verify it. Digital signatures provide integrity and signer authentication/key-based attribution and can support non-repudiation within an appropriate legal and operational framework, but they do not provide confidentiality.

The Digital Signature Standard (DSS) is a NIST standard covering approved digital-signature mechanisms. DSA is the classic algorithm associated with the original DSS. Other signature technologies include RSA signatures, ECDSA, EdDSA, and modern post-quantum schemes.

For an exam, be able to explain:

1. Definition and properties of cryptographic hash functions.
2. MD4 architecture and weaknesses.
3. MD5 characteristics and collision weakness.
4. SHA-1 characteristics and deprecation.
5. SHA-2 family and SHA-256 characteristics.
6. Hashing versus encryption.
7. Digital-signature generation and verification.
8. Security services provided by digital signatures.
9. Digital signatures versus MACs.
10. DSS and DSA, including the role of private/public keys.
11. The importance of secure per-signature randomness in DSA/ECDSA.
12. The difference between collision resistance, preimage resistance, and second-preimage resistance.
