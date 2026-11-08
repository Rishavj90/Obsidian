# Secret Key Systems: DES, Strengthening DES, and AES

These topics are about **symmetric-key (secret-key) cryptography**. The central idea is that the communicating parties share a secret key.

```text
                SAME SECRET KEY
                      │
                      ▼
Plaintext ──→ Encryption ──→ Ciphertext
                              │
                              ▼
                         Decryption
                              │
                              ▼
                         Plaintext
```

The major algorithms you need to know here are:

- **DES — Data Encryption Standard**
    
- **3DES — strengthened version of DES**
    
- **AES — Advanced Encryption Standard**
    

---

# 1. Secret Key Systems

A **secret key system** is a cryptographic system where the **same secret key** is used by both parties for encryption and decryption.

It is also called:

- **Symmetric-key cryptography**
    
- **Secret-key cryptography**
    
- **Conventional cryptography**
    

### Basic model

Suppose Alice wants to send a message to Bob.

```text
                 Secret Key K
                      │
                      ▼
Alice: Plaintext ──→ Encryption
                         │
                         ▼
                    Ciphertext
                         │
                         ↓
                       Network
                         │
                         ▼
Bob: Ciphertext ──→ Decryption
                      ↑
                      │
                 Secret Key K
                      │
                      ▼
                   Plaintext
```

The security depends heavily on keeping `K` secret.

### Mathematical representation

Encryption:
$$
C = E_K(P)
$$
Decryption:
$$
P = D_K(C)
$$
Where:

- `P` = plaintext
- `C` = ciphertext
- `K` = secret key
- `E` = encryption algorithm
- `D` = decryption algorithm

The important property is:
$$
D_K(E_K(P)) = P
$$

---

# 2. Advantages of Secret-Key Systems

### 1. Fast

Symmetric algorithms are generally much faster than public-key algorithms.

This makes them suitable for:

- Large files
    
- Disk encryption
    
- Network traffic
    
- Databases
    
- Video/audio
    
- Secure communications
    

### 2. Efficient

They require relatively less computational power.

### 3. Strong security

With a sufficiently strong modern algorithm and properly generated key, symmetric encryption can provide very strong confidentiality.

---

# 3. Disadvantage: Key Distribution

The biggest traditional problem is:

> **How do Alice and Bob securely obtain the same secret key?**

Suppose Alice creates:

```text
K = 7F3A91...
```

She cannot simply send the key over an insecure network:

```text
Alice ── "Here is our secret key" ──→ Bob
                  ↑
               Attacker
```

The attacker could steal the key and decrypt the communication.

Modern protocols solve this problem using mechanisms such as public-key cryptography or authenticated key-exchange protocols.

---

# 4. Data Encryption Standard (DES)

**DES (Data Encryption Standard)** is a symmetric block cipher that was historically one of the most important encryption standards.

It was based on a design developed at IBM and was standardized by the U.S. government in **1977**.

DES operates on:

> **64-bit blocks of plaintext**

and uses a nominal:

> **64-bit key**

However, 8 bits of the key are used for parity, leaving an **effective key length of 56 bits**.

So remember:

```text
DES
│
├── Block size = 64 bits
├── Key size = 64 bits nominally
├── Effective key = 56 bits
└── Structure = Feistel network
```

---

# 5. DES Structure

DES is based on a **Feistel network**.

The plaintext block first undergoes an:

### Initial Permutation (IP)

Then it is divided into two 32-bit halves:

```text
64-bit plaintext
       │
       ▼
Initial Permutation
       │
       ▼
┌─────────────┐
│ Left 32-bit │
│ Right 32-bit│
└─────────────┘
       │
       ▼
   16 rounds
       │
       ▼
Final Permutation
       │
       ▼
64-bit ciphertext
```

The 16 rounds are the core of DES.

---

# 6. DES Feistel Network

Let:

- $L_i$ = left half during round `i`
    
- $R_i$ = right half during round `i`
    
- $K_i$ = round key
    

Each round follows:

$$L_{i+1}=R_i$$

and

$$R_{i+1}=L_i\oplus F(R_i,K_i)$$

where `F` is the DES round function.

This is extremely important for exams.

---

# 7. DES Round Function

The DES round function operates on the 32-bit right half.

```text
       R (32 bits)
            │
            ▼
     Expansion Permutation
            │
            ▼
       48 bits
            │
            │ XOR
            │
       48-bit key
            │
            ▼
       S-Boxes
            │
            ▼
       32 bits
            │
            ▼
       P-Permutation
            │
            ▼
          F(R,K)
```

There are several important stages.

---

## Step 1: Expansion

The right half initially contains:

```text
32 bits
```

DES expands it to:

```text
48 bits
```

Why?

Because the round key is:

```text
48 bits
```

So the two can be XORed.

```text
R = 32 bits
       ↓
Expansion
       ↓
48 bits
```

Some bits are repeated during this expansion.

---

# 8. Step 2: XOR with Round Key

The expanded 48-bit value is XORed with the 48-bit round key.

$$X = E(R) \oplus K_i$$

where:

- `E(R)` = expanded right half
    
- `Kᵢ` = round key
    

Result:

```text
48 bits
```

---

# 9. Step 3: S-Boxes

This is one of the most important parts of DES.

DES contains:

> **8 S-boxes**

Each S-box takes:

```text
6 bits
```

and produces:

```text
4 bits
```

Therefore:

```text
48 bits
   │
   ├── 6 → S-box 1 → 4
   ├── 6 → S-box 2 → 4
   ├── 6 → S-box 3 → 4
   ├── ...
   └── 6 → S-box 8 → 4
                  ↓
               32 bits
```

S-boxes introduce **non-linearity** into DES.

This makes cryptanalysis significantly more difficult than if the cipher consisted only of simple permutations and XOR operations.

---

# 10. Step 4: P-Permutation

The resulting 32 bits are rearranged using a fixed permutation called the **P-box** or P-permutation.

```text
S-box output
     ↓
P-Permutation
     ↓
32-bit output
```

This helps distribute the influence of the S-box outputs.

---

# 11. One DES Round

Putting it together:

```text
                    Right half R
                         │
                         ▼
                  Expansion (32→48)
                         │
                         ▼
                     XOR Kᵢ
                         │
                         ▼
                      S-boxes
                         │
                         ▼
                  P-Permutation
                         │
                         ▼
                     F(R,Kᵢ)
                         │
                         │
Left half L ─────────── XOR
                         │
                         ▼
                    New Right
                         
New Left = Old Right
```

Mathematically:

$L_{i+1}=R_i$ $R_{i+1}=L_i\oplus F(R_i,K_i)$

This happens **16 times**.

---

# 12. DES Key Generation

DES doesn't use the entire 56-bit effective key directly in every round.

It generates:

> **16 round keys**

Each round key is:

> **48 bits**

The process is roughly:

```text
64-bit key
    │
    ▼
Permuted Choice 1
    │
    ▼
56 bits
    │
    ▼
Split
 ┌───────┐
 │28 | 28│
 └───────┘
    │
    ▼
Left shifts
    │
    ▼
Permuted Choice 2
    │
    ▼
48-bit round key
```

This produces:

```text
K1
K2
K3
...
K16
```

---

# 13. Why Does DES Use a Feistel Structure?

One major benefit of the Feistel structure is that **the same basic structure can be used for encryption and decryption**.

For decryption, the round keys are used in reverse:

```text
Encryption:

K1 → K2 → K3 → ... → K16


Decryption:

K16 → K15 → K14 → ... → K1
```

This simplifies implementation.

---

# 14. Why Was DES Considered Secure?

When DES was introduced, its 56-bit key and 16-round structure were considered strong enough for many applications.

Its design provided resistance against several forms of cryptanalysis, particularly through:

- Multiple rounds
    
- S-boxes
    
- Permutations
    
- Key mixing
    
- Confusion
    
- Diffusion
    

However, the major problem eventually became its **56-bit key size**.

---

# 15. Analyzing the Strength of DES

When analyzing a cryptographic algorithm, we need to consider things such as:

1. Key length
    
2. Brute-force resistance
    
3. Cryptanalytic attacks
    
4. Algorithm structure
    
5. Known weaknesses
    
6. Computational cost
    

---

# 16. DES and Brute-Force Attacks

DES has an effective key size of:

$$2^{56}$$

possible keys.

That's approximately:

$$72\text{ quadrillion}$$

possible keys.

That sounds huge, but modern computing made exhaustive search increasingly practical.

An attacker can theoretically:

```text
Ciphertext
    ↓
Try key 1
Try key 2
Try key 3
...
Try every possible 56-bit key
    ↓
Find valid plaintext
```

This is a **brute-force attack**.

The critical issue is that DES's key space is no longer large enough for modern security requirements.

---

# 17. DES Cryptanalysis

DES has also been extensively studied against forms of cryptanalysis including:

### Differential cryptanalysis

Studies how differences in plaintext pairs affect differences in ciphertext.

### Linear cryptanalysis

Attempts to find statistical relationships between plaintext, ciphertext, and key bits.

These attacks are important academically because they demonstrate how the internal structure of a cipher can be analyzed.

However, for practical modern attacks on ordinary DES, the **short 56-bit key** is the major problem.

---

# 18. Strengthening DES

Once DES became too weak, one obvious question was:

> Can we use DES multiple times to make it stronger?

Yes.

The major historical solution was:

# Triple DES (3DES)

Instead of applying DES once, apply it three times.

The commonly used form is:

$$C = E_{K_3}(D_{K_2}(E_{K_1}(P)))$$

This is called **Encrypt-Decrypt-Encrypt (EDE)**.

---

# 19. Triple DES

Conceptually:

```text
Plaintext
    │
    ▼
DES Encryption with K1
    │
    ▼
DES Decryption with K2
    │
    ▼
DES Encryption with K3
    │
    ▼
Ciphertext
```

For decryption:

```text
Ciphertext
    │
    ▼
DES Decryption with K3
    │
    ▼
DES Encryption with K2
    │
    ▼
DES Decryption with K1
    │
    ▼
Plaintext
```

---

# 20. Why Encrypt-Decrypt-Encrypt?

You might wonder:

> Why is there a decryption operation in the middle?

One important historical reason is **backward compatibility**.

If:

$$K_1=K_2=K_3$$

then:

$$E_K(D_K(E_K(P))) = E_K(P)$$

because:

$$D_K(E_K(P))=P$$

So 3DES effectively becomes ordinary DES.

This allowed implementations to transition more easily from DES.

---

# 21. 3DES Security

3DES substantially increased resistance to brute-force attacks compared with single DES, but it remained based on DES's:

- 64-bit block size
    
- older design
    
- relatively slow performance
    

Therefore, it was eventually superseded by AES.

For modern systems, **AES is preferred over 3DES**.

---

# 22. DES vs 3DES

|Feature|DES|3DES|
|---|--:|--:|
|Block size|64 bits|64 bits|
|Effective DES key|56 bits|Larger effective keying options|
|DES operations|1|3|
|Security|Weak by modern standards|Stronger than DES but obsolete for new systems|
|Speed|Faster|Much slower|
|Modern recommendation|Do not use|Do not use for new designs|

---

# 23. Introduction to AES

Because DES and 3DES had limitations, a new standard was required.

This resulted in:

# AES — Advanced Encryption Standard

AES was selected by **NIST** as the successor to DES.

AES is based on the **Rijndael** cipher, designed by **Joan Daemen and Vincent Rijmen**.

---

# 24. AES Basic Specifications

AES is a:

> **Symmetric block cipher**

It has:

### Block size

$$128\text{ bits}$$

### Key sizes

AES supports:

```text
AES-128 → 128-bit key
AES-192 → 192-bit key
AES-256 → 256-bit key
```

### Number of rounds

|Version|Key size|Rounds|
|---|--:|--:|
|AES-128|128 bits|10|
|AES-192|192 bits|12|
|AES-256|256 bits|14|

Remember:

> **AES always uses a 128-bit block size, but the key size can be 128, 192, or 256 bits.**

---

# 25. AES vs DES

This distinction is frequently asked in exams.

|Feature|DES|AES|
|---|--:|--:|
|Type|Symmetric block cipher|Symmetric block cipher|
|Block size|64 bits|128 bits|
|Key size|56 effective bits|128/192/256 bits|
|Structure|Feistel network|Substitution-permutation network|
|Rounds|16|10/12/14|
|Security today|Insecure|Widely used|
|Performance|Relatively slow|Efficient|
|Status|Obsolete|Current standard|

---

# 26. AES Internal Structure

AES does **not** use a Feistel network like DES.

It uses a:

> **Substitution-Permutation Network (SPN)**

AES represents the 128-bit block as a **4 × 4 matrix of bytes**, called the **state**.

```text
        4 × 4 State

     ┌────┬────┬────┬────┐
     │ b0 │ b4 │ b8 │b12 │
     ├────┼────┼────┼────┤
     │ b1 │ b5 │ b9 │b13 │
     ├────┼────┼────┼────┤
     │ b2 │ b6 │b10 │b14 │
     ├────┼────┼────┼────┤
     │ b3 │ b7 │b11 │b15 │
     └────┴────┴────┴────┘
```

Each cell represents one byte.

Since:

$$16 \text{ bytes} \times 8 = 128 \text{ bits}$$

---

# 27. AES Round Operations

The core AES operations are:

1. **SubBytes**
    
2. **ShiftRows**
    
3. **MixColumns**
    
4. **AddRoundKey**
    

Conceptually:

```text
State
  │
  ▼
SubBytes
  │
  ▼
ShiftRows
  │
  ▼
MixColumns
  │
  ▼
AddRoundKey
  │
  ▼
Next Round
```

There is one important exception:

> The **final AES round does not perform MixColumns**.

---

# 28. SubBytes

Each byte in the state is replaced using an AES **S-box**.

```text
Input byte
    │
    ▼
 AES S-box
    │
    ▼
Output byte
```

This provides **non-linearity** and contributes to resistance against cryptanalysis.

Conceptually:

```text
State:

A1 B2 C3 D4
E5 F6 G7 H8
I9 J0 K1 L2
M3 N4 O5 P6

        ↓ SubBytes

S(A1) S(B2) S(C3) S(D4)
S(E5) S(F6) S(G7) S(H8)
S(I9) S(J0) S(K1) S(L2)
S(M3) S(N4) S(O5) S(P6)
```

---

# 29. ShiftRows

The rows of the state are cyclically shifted.

Conceptually:

```text
Before:

A B C D
E F G H
I J K L
M N O P
```

After:

```text
A B C D
F G H E
K L I J
P M N O
```

The first row doesn't shift.

The second row shifts by 1.

The third row shifts by 2.

The fourth row shifts by 3.

This provides **diffusion**.

---

# 30. MixColumns

Each column is transformed using a mathematical operation over a finite field.

Conceptually:

```text
Column             New column
 A                      A'
 E                      E'
 I       ─────────→     I'
 M                      M'
```

The operation mixes the four bytes of each column.

The purpose is to ensure that a change in one byte spreads to multiple bytes.

This provides further **diffusion**.

---

# 31. AddRoundKey

The state is XORed with a round key.

$$State \oplus RoundKey$$

This is where the secret key directly influences the state.

```text
State
  +
Round Key
  │
  ▼
XOR
  │
  ▼
New State
```

---

# 32. AES-128 Encryption Process

AES-128 uses **10 rounds**.

There is an initial key addition before the normal rounds.

```text
128-bit Plaintext
       │
       ▼
   AddRoundKey
       │
       ▼
 ┌───────────────┐
 │ Round 1       │
 │ SubBytes      │
 │ ShiftRows     │
 │ MixColumns    │
 │ AddRoundKey   │
 └───────────────┘
       │
       ▼
      ...
       │
       ▼
 ┌───────────────┐
 │ Round 10      │
 │ SubBytes      │
 │ ShiftRows     │
 │ AddRoundKey   │
 │ NO MixColumns │
 └───────────────┘
       │
       ▼
128-bit Ciphertext
```

---

# 33. AES Key Expansion

AES does not simply use the original key for every round.

It generates **round keys** from the original key through a process called:

> **Key Expansion**

For AES-128:

```text
Original 128-bit key
        │
        ▼
    Key Expansion
        │
        ├── Round Key 0
        ├── Round Key 1
        ├── Round Key 2
        ├── ...
        └── Round Key 10
```

The key schedule uses operations including:

- RotWord
    
- SubWord
    
- XOR
    
- Round constants
    

---

# 34. Why AES Is Strong

AES provides a much larger key space than DES.

For AES-128:

$$2^{128}$$

possible keys.

For AES-256:

$$2^{256}$$

possible keys.

Compare:

```text
DES:
2^56

AES-128:
2^128

AES-256:
2^256
```

The difference is enormous.

---

# 35. DES vs AES — Key Idea

The historical progression is:

```text
DES
 │
 │ 56-bit key became too small
 ↓
3DES
 │
 │ stronger but slow + 64-bit blocks
 ↓
AES
 │
 ├── 128-bit blocks
 ├── 128/192/256-bit keys
 └── much more suitable for modern systems
```

---

# 36. Confusion and Diffusion

These are two important concepts in understanding both DES and AES.

## Confusion

Confusion makes the relationship between:

```text
Key ↔ Ciphertext
```

difficult to determine.

S-boxes are important for achieving confusion.

---

## Diffusion

Diffusion spreads the influence of individual plaintext bits throughout the ciphertext.

For example, ideally:

```text
Change 1 bit of plaintext
          ↓
Many ciphertext bits change
```

This is related to the **avalanche effect**.

### Avalanche effect

A small change in the input should cause a significant change in the output.

```text
Plaintext 1 → Encryption → Ciphertext A

Plaintext 2 → Encryption → Ciphertext B
     ↑
 only a tiny change
     
Yet:
Ciphertext A and B should differ significantly.
```

Good cryptographic algorithms aim for strong diffusion and confusion.

---

# 37. Important Exam Questions

You should be able to answer these without referring to notes:

### Short questions

**1. What is a secret-key system?**

A symmetric cryptographic system in which the same shared secret key is used for encryption and decryption.

**2. What is DES?**

DES is a symmetric block cipher that processes 64-bit blocks and has an effective key length of 56 bits.

**3. How many rounds does DES have?**

**16 rounds.**

**4. What is the effective key size of DES?**

**56 bits.**

**5. What is the block size of DES?**

**64 bits.**

**6. What is 3DES?**

A strengthened form of DES that applies DES three times, commonly using Encrypt-Decrypt-Encrypt.

**7. What is AES?**

AES is a modern symmetric block cipher standardized as the successor to DES, using a 128-bit block and 128-, 192-, or 256-bit keys.

**8. What are AES's round counts?**

```text
AES-128 → 10
AES-192 → 12
AES-256 → 14
```

**9. What are the four AES round operations?**

```text
SubBytes
ShiftRows
MixColumns
AddRoundKey
```

Except that the final round omits **MixColumns**.

---

# 38. The Most Important Things to Memorize

If this is for an exam, prioritize this table:

| Property        | DES                           | 3DES                               | AES                                            |
| --------------- | ----------------------------- | ---------------------------------- | ---------------------------------------------- |
| Type            | Symmetric block cipher        | Symmetric block cipher             | Symmetric block cipher                         |
| Block size      | 64 bits                       | 64 bits                            | 128 bits                                       |
| Key             | 56 effective bits             | 2/3 DES keys depending on variant  | 128/192/256 bits                               |
| Structure       | Feistel                       | Feistel-based                      | Substitution-Permutation Network               |
| Rounds          | 16                            | 48 DES operations                  | 10/12/14                                       |
| Main weakness   | Small key                     | Slow, 64-bit blocks, legacy design | No comparable practical weakness in normal use |
| Status          | Obsolete                      | Legacy/being phased out            | Modern standard                                |
| Main components | Expansion, S-box, permutation | DES ×3                             | SubBytes, ShiftRows, MixColumns, AddRoundKey   |

### One-line memory trick

```text
DES  → 64-bit block + 56-bit key + 16 Feistel rounds

3DES → DES three times

AES  → 128-bit block + 128/192/256-bit key
       + 10/12/14 rounds
       + SubBytes, ShiftRows, MixColumns, AddRoundKey
```

The conceptual distinction to keep clear is **DES = Feistel network**, whereas **AES = substitution-permutation network**. That difference, along with the block size, key sizes, rounds, and internal operations, covers a large portion of typical questions on this topic.