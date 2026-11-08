# Public Key Encryption Systems

Public-key encryption is also called **asymmetric cryptography**. It was introduced to solve some of the major problems of secret-key systems, especially **key distribution**.

The two major ideas you need to understand are:

1. **Concept and characteristics of public-key encryption**
    
2. **RSA (Rivest–Shamir–Adleman)**
    

---

# 1. Why Do We Need Public-Key Encryption?

Recall the problem with symmetric encryption.

Alice and Bob need the same secret key:

```text
Alice ─────── Secret Key K ─────── Bob
```

But how do they securely share `K` in the first place?

If Alice sends the key over an insecure network:

```text
Alice ── Secret Key ──→ Bob
             ↑
          Attacker
```

the attacker can steal the key.

Public-key cryptography provides a different approach.

Instead of one shared key, each user has **two related keys**:

```text
             Bob
              │
       ┌──────┴──────┐
       ↓             ↓
  Public Key     Private Key
```

The public key can be distributed openly.

The private key must remain secret.

---

# 2. Concept of Public-Key Encryption

Public-key encryption uses a pair of mathematically related keys:

- **Public key** — can be made available to everyone.
    
- **Private key** — must be kept secret by its owner.
    

For encryption:

```text
                  Bob's Public Key
                         ↓
Alice: Plaintext → Encryption → Ciphertext
                                │
                                ↓
                              Bob
                                │
                         Bob's Private Key
                                ↓
                           Decryption
                                ↓
                             Plaintext
```

The crucial property is:

> A message encrypted with Bob's public key can be decrypted using Bob's corresponding private key.

Mathematically:

$$C = E_{K_{pub}}(P)$$

and

$$P = D_{K_{priv}}(C)$$

where:

- `P` = plaintext
    
- `C` = ciphertext
    
- $K_{pub}$ = public key
    
- $K_{priv}$ = private key
    

---

# 3. Example

Suppose Bob has:

```text
Public Key  → Anyone can know this
Private Key → Only Bob knows this
```

Alice wants to send Bob a confidential message:

```text
"Meet at 5 PM"
```

Alice obtains Bob's public key:

```text
Meet at 5 PM
      ↓
Bob's Public Key
      ↓
Encryption
      ↓
7xA9#pQ...     ← Ciphertext
```

Alice sends the ciphertext to Bob.

Even if an attacker intercepts it:

```text
Attacker → 7xA9#pQ...
```

the attacker should not be able to recover the plaintext without Bob's private key.

Bob uses his private key:

```text
Ciphertext
     ↓
Bob's Private Key
     ↓
Decryption
     ↓
"Meet at 5 PM"
```

---

# 4. Public Key vs Private Key

|Public Key|Private Key|
|---|---|
|Can be shared|Must be kept secret|
|Used by others to encrypt to you|Used by you to decrypt|
|Often distributed through certificates/directories|Stored securely by its owner|
|Does not need to be secret|Must remain secret|

The exact operations depend on the cryptosystem. In RSA, the same mathematical key pair can also be used in a different way for **digital signatures**, which is distinct from encryption.

---

# 5. Characteristics of Public-Key Encryption

A good public-key cryptosystem has several important characteristics.

## 1. Two different keys are used

There is a key pair:

$$(K_{pub},K_{priv})$$

rather than one shared secret key.

---

## 2. Public key can be openly distributed

Bob can publish his public key.

```text
Bob's Public Key → Internet
```

Anyone can obtain it.

This eliminates the need to secretly distribute Bob's encryption key to every person who wants to communicate with him.

---

## 3. Private key must remain secret

The security of the system depends on protecting the private key.

If an attacker obtains Bob's private key:

```text
Private Key stolen
       ↓
Attacker may decrypt protected data
       ↓
Security compromised
```

---

## 4. Computationally difficult to derive private key

A fundamental requirement is that:

> Knowing the public key should not make it computationally feasible to determine the corresponding private key.

This is based on mathematical problems that are considered computationally difficult.

For RSA, the security is historically associated with the difficulty of **factoring large integers**.

---

## 5. Encryption and decryption are related

The keys are mathematically related such that:

$$D_{K_{priv}}(E_{K_{pub}}(P))=P$$

The relationship is carefully designed so that the legitimate receiver can efficiently decrypt, while an attacker cannot feasibly do so without the private key.

---

# 6. Public-Key Encryption vs Secret-Key Encryption

This is an important exam comparison.

|Feature|Symmetric/Secret Key|Public Key|
|---|---|---|
|Number of keys|One shared secret|Two related keys|
|Keys|Same key for encryption/decryption|Public + private|
|Key distribution|Difficult|Easier for public encryption key|
|Speed|Fast|Generally slower|
|Large data|Very suitable|Usually inefficient|
|Examples|AES, ChaCha20|RSA, ECC-based systems|
|Main use|Bulk data encryption|Key establishment, signatures, identity mechanisms|

In real systems, they are usually **combined** rather than treated as competitors.

For example:

```text
Public-key cryptography
          ↓
Establish/protect session key
          ↓
Symmetric cryptography
          ↓
Encrypt large amount of data
```

---

# 7. Applications of Public-Key Cryptography

Public-key cryptography is used for several purposes.

### 1. Confidentiality

Encrypt a message using the receiver's public key.

### 2. Digital signatures

A sender can use their private key to create a signature that others verify using the corresponding public key.

### 3. Authentication

Public-key mechanisms can help establish that a party possesses the corresponding private key.

### 4. Key exchange/establishment

Public-key techniques can help two parties establish a symmetric session key.

This is particularly important in protocols such as TLS.

---

# 8. RSA

RSA stands for:

> **Rivest–Shamir–Adleman**

It was developed by:

- **Ronald Rivest**
    
- **Adi Shamir**
    
- **Leonard Adleman**
    

RSA was published in **1977** and became one of the most famous public-key cryptosystems.

RSA can be used for:

- Public-key encryption
    
- Digital signatures
    

The encryption part is based on modular arithmetic and the properties of large prime numbers.

---

# 9. Basic Idea Behind RSA

RSA is based on choosing two large prime numbers.

Let:

$$p = \text{first prime} $$$$q = \text{second prime}$$

Then calculate:

$$n=pq$$

The number `n` becomes part of both the public and private keys.

The security intuition is:

> Multiplying two large primes is easy, but factoring their product back into the original primes is computationally difficult when the primes are sufficiently large.

For example:

```text
p = 61
q = 53

n = 61 × 53
  = 3233
```

Finding `3233` from `61 × 53` is easy.

But if you were only given a sufficiently large `n`, finding its two huge prime factors is computationally difficult.

The example above uses tiny numbers purely for learning.

---

# 10. RSA Key Generation

RSA key generation can be broken into several steps.

---

## Step 1: Choose Two Prime Numbers

Choose two distinct prime numbers:

$$p,q$$

For our small educational example:

$$p=61$$ $$q=53$$

---

# 11. Step 2: Calculate n

Calculate:

$$n=pq$$

Therefore:

$$n=61\times53$$ $$n=3233$$

The value `n` is called the **modulus**.

---

# 12. Step 3: Calculate Euler's Totient

For RSA:

$$\phi(n)=(p-1)(q-1)$$

Therefore:

$$\phi(3233) =(61-1)(53-1) =60\times52 =3120$$

So:

$$\phi(n)=3120$$

---

# 13. Step 4: Choose the Public Exponent

Choose an integer `e` such that:

$$1<e<\phi(n)$$

and:

$$gcd(e,\phi(n))=1$$

In our example, choose:

$$e=17$$

Check:

$$gcd(17,3120)=1$$

So `17` is valid.

---

# 14. Step 5: Calculate the Private Exponent

Now calculate `d` such that:

$$ed\equiv1\pmod{\phi(n)}$$

In other words, `d` is the **multiplicative inverse of `e` modulo $\phi(n)$**.

We need:

$$17d\equiv1\pmod{3120}$$

The solution is:

$$d=2753$$

because:

$$17\times2753=46801$$
and:

$$46801\bmod3120=1$$

---

# 15. RSA Keys

We now have:

$$p=61$$$$q=53$$$$n=3233$$ $$e=17$$ $$d=2753$$

Therefore:

### Public key

$$(e,n)$$ $$\boxed{(17,3233)}$$

### Private key

$$(d,n)$$ $$\boxed{(2753,3233)}$$

---

# 16. RSA Encryption

Suppose the plaintext is represented by an integer `M`.

RSA encryption is:

$$C=M^e\bmod n$$

where:

- `M` = plaintext integer
    
- `e` = public exponent
    
- `n` = modulus
    
- `C` = ciphertext
    

Using our example, suppose:

$$M=65$$

Then:

$$C=65^{17}\bmod3233$$

which gives:

$$C=2790$$

So:

```text
Plaintext M = 65

       ↓
RSA encryption
       ↓
Ciphertext C = 2790
```

---

# 17. RSA Decryption

The receiver uses the private key.

The decryption equation is:

$$M=C^d\bmod n$$

Therefore:

$$M=2790^{2753}\bmod3233$$

which gives:

$$M=65$$

So:

```text
Ciphertext = 2790

      ↓
Private key (2753,3233)

      ↓

Plaintext = 65
```

The original message is recovered.

---

# 18. RSA Key Generation Summary

This is one of the most important sequences to memorize:

```text
Choose p and q
      ↓
n = p × q
      ↓
φ(n) = (p−1)(q−1)
      ↓
Choose e such that gcd(e,φ(n)) = 1
      ↓
Find d such that
ed ≡ 1 mod φ(n)
      ↓
Public Key = (e,n)
Private Key = (d,n)
```

---

# 19. Why Does RSA Work?

The mathematical foundation is the relationship:

$$ed\equiv1\pmod{\phi(n)}$$

This means there exists some integer `k` such that:

$$ed=1+k\phi(n)
$$
When RSA performs:

$$(M^e)^d=M^{ed}$$

the mathematical properties of modular arithmetic allow the original message to be recovered modulo `n` under the RSA conditions.

Therefore:

$$M^{ed}\equiv M\pmod n$$

and:

$$C^d\bmod n=M$$

This is the mathematical heart of RSA.

---

# 20. Why Is RSA Secure?

The security of textbook RSA is associated with the difficulty of factoring:

$$n=pq$$

where `p` and `q` are very large primes.

The attacker knows:

```text
Public key:
(e,n)
```

but does not know:

```text
p
q
d
```

If an attacker could efficiently factor `n` into `p` and `q`, they could compute:

$$\phi(n)=(p-1)(q-1)$$

and then derive the private exponent `d`.

Thus:

```text
Public key
   │
   ├── e
   └── n = p × q
             │
             │ difficult to factor
             ↓
        p and q hidden
             ↓
        φ(n) hidden
             ↓
        d protected
```

---

# 21. Important: Textbook RSA Is Not Used Directly

A common mistake is thinking that real applications simply perform:

$$C=M^e\bmod n$$

on raw messages.

They do **not** use bare textbook RSA this way.

Practical RSA encryption requires a secure padding/encoding scheme such as **RSA-OAEP**.

Similarly, RSA signatures use appropriate signature encoding schemes such as **RSA-PSS**.

This prevents important attacks against naïve textbook RSA.

---

# 22. RSA and Digital Signatures

RSA has another major application: **digital signatures**.

The basic conceptual idea is reversed compared with confidentiality.

For confidentiality:

```text
Sender
   ↓
Receiver's PUBLIC key
   ↓
Encryption
   ↓
Ciphertext
```

For a digital signature:

```text
Sender
   ↓
Sender's PRIVATE key
   ↓
Signature
   ↓
Anyone can verify using
sender's PUBLIC key
```

Conceptually:

```text
              Alice
                │
          Private Key
                │
                ▼
          Digital Signature
                │
                ↓
             Bob
                │
          Alice's Public Key
                │
                ▼
             Verify
```

This provides evidence that the signature corresponds to the holder of the private key and that the signed data has not been altered, assuming the surrounding system is properly designed.

---

# 23. RSA Encryption vs RSA Signature

Do not confuse these.

|Purpose|Key associated with operation|
|---|---|
|Encrypt message for Bob|Bob's **public key**|
|Decrypt message|Bob's **private key**|
|Create Alice's signature|Alice's **private key**|
|Verify Alice's signature|Alice's **public key**|

A useful memory rule:

> **Encryption → receiver's public key.**  
> **Signature → sender's private key.**

---

# 24. Advantages of RSA

### 1. Solves key distribution problem

The public key can be openly distributed.

### 2. Supports digital signatures

RSA can provide both encryption and signature mechanisms.

### 3. Well studied

RSA has been extensively analyzed over several decades.

### 4. Useful for establishing secure communications

It has historically been widely used in security protocols and applications.

---

# 25. Disadvantages of RSA

### 1. Slow compared with symmetric encryption

RSA is computationally expensive compared with AES.

Therefore, you normally don't encrypt a large file directly with RSA.

Instead:

```text
Random AES Session Key
          ↓
AES encrypts large data
          ↓
RSA protects/establishes the session key
```

This is an example of **hybrid cryptography**.

---

### 2. Large keys

RSA generally requires much larger key sizes than symmetric algorithms to achieve comparable security levels.

---

### 3. Requires secure padding

Raw/textbook RSA is insecure for practical use.

Proper schemes such as OAEP are necessary for encryption.

---

### 4. Private-key protection is critical

If the private key is compromised, the security of the system can be severely compromised.

---

# 26. RSA Example — Complete Flow

Let's put the entire small example together.

### Key generation

```text
p = 61
q = 53
```

Calculate:

```text
n = p × q
  = 3233
```

Calculate:

```text
φ(n) = (p−1)(q−1)
     = 3120
```

Choose:

```text
e = 17
```

Find:

```text
d = 2753
```

Therefore:

```text
Public Key:
(17, 3233)

Private Key:
(2753, 3233)
```

### Encryption

Suppose:

```text
M = 65
```

Then:

$$C=65^{17}\bmod3233$$ $$C=2790$$

### Decryption

$$M=2790^{2753}\bmod3233 $$
$$M=65$$

Thus:

```text
65
 ↓
RSA Encryption
 ↓
2790
 ↓
RSA Decryption
 ↓
65
```

---

# 27. Symmetric vs Public-Key Cryptography

You should understand **why both exist**.

Suppose Alice and Bob want to communicate.

Using only symmetric encryption:

```text
Alice ←──── Secret Key ────→ Bob
```

The problem is securely establishing that shared key.

Using public-key cryptography:

```text
Bob publishes:
Public Key

Alice → encrypts something for Bob
       using Bob's public key
```

This helps with key establishment.

But public-key operations are relatively expensive.

So modern systems typically combine them:

```text
              Public-Key Cryptography
                       │
                       ↓
             Establish session key
                       │
                       ↓
                Symmetric Key
                       │
                       ↓
                  AES/ChaCha20
                       │
                       ↓
                 Bulk Encryption
```

This is the fundamental idea behind **hybrid cryptography**.

---

# 28. What You Should Memorize

For this chapter, the core structure is:

```text
PUBLIC KEY CRYPTOGRAPHY
        │
        ├── Two keys
        │     ├── Public
        │     └── Private
        │
        ├── Public key can be shared
        │
        ├── Private key remains secret
        │
        ├── Slower than symmetric encryption
        │
        └── RSA
              │
              ├── Choose p, q
              ├── n = pq
              ├── φ(n) = (p−1)(q−1)
              ├── Choose e
              ├── Find d
              ├── Public = (e,n)
              └── Private = (d,n)
```

And for RSA specifically:
$$\boxed{n=pq}$$ $$\boxed{\phi(n)=(p-1)(q-1)}$$ $$\boxed{ed\equiv1\pmod{\phi(n)}}$$$$\boxed{C=M^e\bmod n}$$$$\boxed{M=C^d\bmod n}$$

The **most important conceptual distinction** from your previous DES/AES topic is:

> **DES/AES use one shared secret key. RSA uses a mathematically related public/private key pair.**

RSA then trades the speed of symmetric cryptography for easier public-key distribution and additional capabilities such as digital signatures.