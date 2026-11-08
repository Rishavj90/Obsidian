# Basic Encryption and Decryption
---

# 1. Encryption and Decryption

### Encryption

**Encryption** is the process of converting readable information (**plaintext**) into an unreadable form (**ciphertext**) using an algorithm and usually a key.

```text
Plaintext + Key + Encryption Algorithm
                    ↓
                Ciphertext
```

Example:

```text
Plaintext:   HELLO
Key:         3
             ↓
Ciphertext:  KHOOR
```

Someone who intercepts `KHOOR` should not be able to understand the original message without the necessary key or ability to break the encryption.

### Decryption

**Decryption** is the reverse process: converting ciphertext back into plaintext.

```text
Ciphertext + Key + Decryption Algorithm
                    ↓
                 Plaintext
```

For example:

```text
KHOOR → HELLO
```

### Important terminology

|Term|Meaning|
|---|---|
|Plaintext|Original readable message|
|Ciphertext|Encrypted/unreadable message|
|Encryption|Plaintext → Ciphertext|
|Decryption|Ciphertext → Plaintext|
|Cipher|Algorithm used for encryption/decryption|
|Key|Secret or relevant value controlling the cipher|
|Cryptography|Science of securing information using mathematical techniques|
|Cryptanalysis|Study of breaking or analyzing cryptographic systems|

---

# 2. Attackers and Types of Threats

An **attacker** is an individual or organization that attempts to compromise a system, network, application, or information.

## Types of attackers

### 1. Passive attacker

A passive attacker **observes information but does not modify it**.

Example:

```text
Alice ───────────────→ Bob
          ↑
       Attacker
       listening
```

The attacker may capture network traffic.

Examples:

- Eavesdropping
    
- Traffic analysis
    
- Packet sniffing
    

The main goal is usually to obtain confidential information.

---

### 2. Active attacker

An active attacker **modifies, injects, deletes, or disrupts information**.

Example:

```text
Alice ───→ Attacker ───→ Bob
             |
          modifies
          message
```

Examples:

- Modifying a transaction
    
- Injecting malicious packets
    
- Denial-of-service attacks
    
- Replay attacks
    

---

## Common security threats

### 1. Interception

An unauthorized person obtains information.

Example:

> Attacker captures someone's network communication.

Primarily threatens **confidentiality**.

---

### 2. Modification

An attacker changes information.

Example:

```text
Amount = ₹1000
       ↓
Attacker changes it
       ↓
Amount = ₹9000
```

Threatens **integrity**.

---

### 3. Fabrication

An attacker creates fake information or messages.

Example:

> Attacker sends a fake transaction pretending to be a legitimate user.

Threatens **authenticity/integrity**.

---

### 4. Interruption

An attacker prevents legitimate users from accessing a service.

Example:

> DDoS attack makes a website unavailable.

Threatens **availability**.

---

# 3. Information Security Challenges

Information security is generally concerned with protecting information from unauthorized access, modification, destruction, or disruption.

The major security objectives are commonly represented by the **CIA Triad**.

## C — Confidentiality

Only authorized people should be able to access information.

Example:

> Only you should be able to read your encrypted messages.

Methods include:

- Encryption
    
- Access control
    
- Authentication
    

---

## I — Integrity

Information should not be improperly modified.

Example:

> A bank transaction of ₹5,000 should not become ₹50,000.

Methods include:

- Hash functions
    
- Digital signatures
    
- Message authentication codes
    

---

## A — Availability

Information and services should be available when required.

Example:

> A banking website should remain accessible to legitimate customers.

Methods include:

- Backups
    
- Redundant servers
    
- DDoS protection
    
- Disaster recovery
    

---

## Other important security challenges

### Authentication

Determining **who someone is**.

Example:

```text
Username + Password
        ↓
   Authentication
```

---

### Authorization

Determining **what an authenticated user is allowed to do**.

For example:

```text
Student → View marks
Teacher → Modify marks
Admin   → Manage users
```

---

### Non-repudiation

Ensuring that someone cannot reasonably deny performing an action.

Digital signatures are commonly used for this purpose.

---

### Key management

Encryption is only as secure as the way keys are generated, stored, distributed, and protected.

For example, even a strong encryption algorithm becomes useless if:

```text
Encryption: Strong
Key:        "password123"
Key storage: Publicly accessible
```

---

# 4. Encryption Techniques

Encryption techniques can be broadly divided into:

```text
                    Cryptography
                         |
          ┌──────────────┴──────────────┐
          ↓                             ↓
    Symmetric                       Asymmetric
    Encryption                      Encryption
          |                             |
     One shared key              Public + Private key
          |
    ┌─────┴─────┐
    ↓           ↓
Stream       Block
Cipher       Cipher
```

---

## Symmetric Encryption

The **same secret key** is used for encryption and decryption.

```text
             Secret Key
                 ↓
Plaintext → Encryption → Ciphertext
                           |
                           ↓
                     Decryption
                           ↑
                       Same Key
                           |
                           ↓
                       Plaintext
```

Examples:

- AES
    
- DES
    
- 3DES
    
- ChaCha20
    

### Advantages

- Fast
    
- Efficient for large amounts of data
    
- Relatively low computational cost
    

### Problem

The sender and receiver must somehow securely obtain the same secret key.

This is called the **key distribution problem**.

---

# 5. Asymmetric Encryption

Asymmetric cryptography uses a **pair of keys**:

- Public key
    
- Private key
    

The public key can be shared openly, while the private key must be kept secret.

For encryption:

```text
Message
   ↓
Encrypt with Bob's Public Key
   ↓
Ciphertext
   ↓
Decrypt with Bob's Private Key
   ↓
Message
```

Examples:

- RSA
    
- ECC-based cryptography
    

### Main advantage

It solves much of the key-distribution problem associated with symmetric encryption.

### Disadvantage

It is generally computationally more expensive than symmetric encryption.

In real systems, both are often used together.

For example, HTTPS/TLS can use public-key cryptography to establish/protect a session key, and symmetric cryptography to efficiently encrypt the actual data.

---

# 6. Classical Cryptographic Algorithms

Before modern algorithms such as AES and RSA, cryptography relied heavily on **classical ciphers**.

Two major categories are:

1. Substitution ciphers
    
2. Transposition ciphers
    

---

# 7. Mono-alphabetic Substitution Cipher

A **substitution cipher** replaces each plaintext character with another character.

In a **mono-alphabetic substitution cipher**, each plaintext letter is consistently mapped to one ciphertext letter.

For example:

```text
Plain:       A B C D E F G H I J ...
Cipher:      D E F G H I J K L M ...
```

So:

```text
A → D
B → E
C → F
...
```

The mapping remains the same throughout the message.

## playfair cipher

![playfair cipher](https://www.youtube.com/watch?v=-KjFbTK1IIw)

---

# 8. Caesar Cipher

The **Caesar cipher** is one of the simplest substitution ciphers.

It shifts each letter by a fixed number of positions in the alphabet.

Suppose the key is:

```text
3
```

Then:

```text
A → D
B → E
C → F
D → G
...
X → A
Y → B
Z → C
```

Therefore:

```text
HELLO
```

becomes:

```text
KHOOR
```

### Encryption formula

If letters are represented by numbers:

```text
A = 0
B = 1
C = 2
...
Z = 25
```

Then:
$$
C=(P+K)\mod26
$$
Where:

- `P` = plaintext letter
    
- `C` = ciphertext letter
    
- `K` = key
    

### Example

For `H`:

```text
H = 7
K = 3

C = (7 + 3) mod 26
  = 10
```

`10 = K`

Therefore:

```text
H → K
```

### Decryption formula

$$
P = (C-K) \mod 26
$$
For:

```text
KHOOR
```

using key `3`:

```text
K → H
H → E
O → L
O → L
R → O
```

Result:

```text
HELLO
```

---

# 9. Why Caesar Cipher Is Weak

Caesar cipher has only **26 possible keys**.

An attacker can simply try every possibility.

For example:

```text
Ciphertext
    ↓
Try key 1
Try key 2
Try key 3
...
Try key 25
    ↓
Readable plaintext found
```

This is called a **brute-force attack**.

It is also vulnerable to **frequency analysis**, because the frequency pattern of letters is preserved.

For example, if `E` is the most common letter in English, the most frequently occurring ciphertext letter may give clues about which plaintext letter it represents.

---

# 10. Mono-alphabetic Substitution vs Caesar Cipher

Caesar cipher is actually a specific type of mono-alphabetic substitution cipher.

### Caesar cipher

Uses a fixed shift:

```text
A → D
B → E
C → F
...
```

### General mono-alphabetic substitution

Can use an arbitrary mapping:

```text
A → Q
B → W
C → E
D → R
...
```

There can be enormously more possible mappings, but the cipher is still vulnerable to **frequency analysis** because the same plaintext letter always maps to the same ciphertext letter.

---

# 11. Stream Ciphers

A **stream cipher** encrypts data one bit or one byte at a time using a generated **keystream**.

Conceptually:

```text
Plaintext:   10110110
Keystream:   01101001
             ↓ XOR
Ciphertext:  11011111
```

The encryption operation is often XOR:

$$
C = P \oplus K
$$
where:

- `P` = plaintext
- `K` = keystream
- `C` = ciphertext

Decryption:
$$
P = C \oplus K
$$
because:
$$
(P \oplus K) \oplus K = P
$$
---

## Example

Suppose:

```text
Plaintext  = 10110010
Keystream  = 01101101
```

XOR:

```text
10110010
01101101
────────
11011111
```

Ciphertext:

```text
11011111
```

For decryption:

```text
11011111
01101101
────────
10110010
```

Original plaintext is recovered.

---

## Examples of stream ciphers

Historically:

- RC4
    

Modern designs include:

- ChaCha20
    

Stream ciphers can be very efficient for data that arrives continuously.

---

# 12. Block Ciphers

A **block cipher** encrypts data in fixed-size blocks.

For example, suppose a cipher uses 128-bit blocks:

```text
Plaintext
    ↓
┌──────────────┐
│ 128-bit block│
└──────────────┘
    ↓
Encryption
    ↓
┌──────────────┐
│ 128-bit block│
└──────────────┘
    ↓
Ciphertext
```

Instead of encrypting individual bytes independently, the algorithm processes an entire block.

---

## AES

**AES (Advanced Encryption Standard)** is the most important modern example of a block cipher.

AES has:

- Block size: **128 bits**
    
- Key sizes: **128, 192, or 256 bits**
    

For example:

```text
Plaintext
    ↓
128-bit block
    ↓
AES + secret key
    ↓
128-bit ciphertext block
```

AES is widely used in:

- File encryption
    
- Disk encryption
    
- VPNs
    
- Network security
    
- TLS/HTTPS
    
- Secure applications
    

---

# 13. Stream Cipher vs Block Cipher

| Feature     | Stream Cipher            | Block Cipher                      |
| ----------- | ------------------------ | --------------------------------- |
| Processing  | Bit/byte-oriented stream | Fixed-size blocks                 |
| Data unit   | Individual bits/bytes    | Blocks                            |
| Example     | ChaCha20, RC4            | AES                               |
| Padding     | Usually unnecessary      | May be required depending on mode |
| Typical use | Continuous streams       | Files, storage, network data      |
| Basic idea  | Plaintext XOR keystream  | Transform entire blocks           |

One important qualification: modern **block-cipher modes** such as CTR can make a block cipher behave much like a stream cipher, while authenticated modes such as GCM provide both encryption and integrity protection.

---

# 14. Putting Everything Together

The overall progression is:

```text
                 INFORMATION SECURITY
                         │
                         ↓
                    Cryptography
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
       Classical                  Modern
       Cryptography              Cryptography
             │                       │
      ┌──────┴──────┐        ┌───────┴────────┐
      ↓             ↓        ↓                ↓
Substitution   Transposition Symmetric     Asymmetric
      │                         │                │
   Caesar                 ┌────┴────┐        RSA / ECC
                          ↓         ↓
                       Stream     Block
                          │         │
                       ChaCha20    AES
```

And the security goals are:

```text
             Information Security
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
 Confidentiality  Integrity  Availability
        │
    Encryption
        │
   ┌────┴─────┐
   ↓          ↓
Symmetric  Asymmetric
   │          │
   ↓          ↓
Stream/Block Public/Private
   │
   ├── Stream → RC4, ChaCha20
   └── Block  → AES
```

