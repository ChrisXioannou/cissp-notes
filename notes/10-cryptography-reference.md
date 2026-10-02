# Cryptography Reference

## Contents

- [Algorithm cheat sheet](#algorithm-cheat-sheet)
- [Hash output lengths](#hash-output-lengths)
- [Symmetric block and key sizes](#symmetric-block-and-key-sizes)
- [Asymmetric key sizes at equivalent strength](#asymmetric-key-sizes-at-equivalent-strength)
- [Password hashing and key derivation](#password-hashing-and-key-derivation)
- [Digital signature algorithms](#digital-signature-algorithms)
- [Classical and historical ciphers](#classical-and-historical-ciphers)
- [Key selection rules](#key-selection-rules)

---

## Algorithm cheat sheet

**Algorithm Type Key facts and tricks**

| Lucifer | Block cipher | IBM/Feistel, precursor to DES. "Lucifer to DES" is the pairing to know |
| --- | --- | --- |
| DES | Block, 56-bit key | Derived from Lucifer, adopted 1976, now broken and deprecated (key too short) |
| 3DES | Block | DES applied three times (encrypt-decrypt-encrypt). Stopgap before AES |
| AES (Rijndael) | Block | Won the NIST competition in 2001, replaced DES/3DES, current US government standard |
| Skipjack | Block | NSA, used in the Clipper chip, tied to key escrow. Exam trap alongside DES |
| Blowfish | Block | Schneier, free and unpatented DES alternative |
| Twofish | Block | Schneier, AES finalist (lost to Rijndael), successor to Blowfish |
| IDEA | Block | Used in early PGP versions, 128-bit key |
| RC4 | Stream | Used in WEP and early SSL. Broken and deprecated |
| RC5 / RC6 | Block | Ron Rivest. RC6 was an AES finalist |
| RSA | Asymmetric | Factoring large primes. Key exchange plus digital signatures |
| Diffie-Hellman | Asymmetric | Key exchange only, no encryption or signing. Vulnerable to MITM without authentication |
| ECC | Asymmetric | Elliptic curve. Smaller keys for the same strength as RSA. Efficient for mobile and IoT |
| El Gamal | Asymmetric | Based on the discrete logarithm problem. Used in some signature schemes |
| MD5 | Hash | 128-bit, broken (collisions found). Frequent wrong-answer bait |
| SHA-1 | Hash | 160-bit, deprecated, collisions demonstrated |
| SHA-2 | Hash | Current standard family (SHA-256, SHA-512) |

#### Trick patterns:

- "Same author as Blowfish" means Twofish

- "NSA plus key escrow or Clipper" means Skipjack

- "Derived from or based on X" means the Lucifer to DES chain

- "AES finalist but lost" means Twofish, RC6, Serpent or MARS (Rijndael won)

- "Key exchange only, no encryption" means Diffie-Hellman

- "Broken due to collisions" means MD5 or SHA-1

## Hash output lengths

| Algorithm | Output | Status |
| --- | --- | --- |
| MD5 | 128 bits | Broken, collisions |
| SHA-1 | 160 bits | Broken, deprecated |
| SHA-224 | 224 |  |
| SHA-256 | 256 bits | Standard |
| SHA-384 | 384 |  |
| SHA-512 | 512 bits |  |
| SHA-3 | 224/256/384/512 | Keccak, different internal design |
| RIPEMD-160 | 160 |  |
| HAVAL | 128/160/192/224/256 | Variable |

SHA-2 names tell you the output. SHA-256 gives 256 bits. Free marks.

## Symmetric block and key sizes

**Algorithm Block Key**

| DES | 64 | 56 effective (64 with parity) |
| --- | --- | --- |
| 3DES | 64 | 112 or 168 |
| AES | 128 always | 128, 192 or 256 |
| Blowfish | 64 | 32 to 448 |
| Twofish | 128 | 128, 192, 256 |
| IDEA | 64 | 128 |
| RC4 | stream | 40 to 2048 |
| Skipjack | 64 | 80 |

**AES block size is always 128 regardless of key size. Common trap**

## Asymmetric key sizes at equivalent strength

| Symmetric | RSA / DH | ECC |
| --- | --- | --- |
| 80 | 1024 | 160 |
| 112 | 2048 | 224 |
| 128 | 3072 | 256 |
| 192 | 7680 | 384 |
| 256 | 15360 | 512 |

> **ECC achieves equivalent strength with far smaller keys.** That is why it is used on mobile and IoT.

## Password hashing and key derivation

- **bcrypt:** based on Blowfish. Adds 128 additional bits as salt. Protects against rainbow table attacks. Used by many Unix/Linux systems

## Digital signature algorithms

**DSS =** Digital Signature Standard, FIPS 186-4. Requires SHA-3 hashing. Three approved encryption algorithms:

- DSA (FIPS 186-4)

- RSA (ANSI X9.31)

- ECDSA (ANSI X9.62)

**Also recognize by name:** Schnorr's signature algorithm, Nyberg-Rueppel's signature algorithm.

**Digital signature goals:** authentication (proves sender), integrity (proves unmodified), nonrepudiation. Does NOT provide confidentiality by itself.

## Classical and historical ciphers

| **Cipher**                       | **Notes**                                                                                |
|----------------------------------|------------------------------------------------------------------------------------------|
| **Caesar cipher (ROT3)**         | Shift each letter 3 right. Monoalphabetic substitution. Vulnerable to frequency analysis |
| **Vigenère**                     | Polyalphabetic substitution, uses a key and a 26-row shift chart                         |
| **One-time pad (Vernam cipher)** | Gilbert Vernam, AT&T Bell Labs. Only unbreakable cipher when implemented correctly       |
| **Enigma / Ultra**               | WWII German machine, broken by Allied Ultra effort                                       |

## Key selection rules

- **Encrypt a message:** recipient's public key

- **Decrypt a message sent to you:** your private key

- **Sign a message:** your private key

- **Verify a signature:** sender's public key

#### Mapped onto a modern TLS cipher suite

- **ECDHE =** key exchange (ephemeral, gives forward secrecy)

- **RSA or ECDSA =** signs the handshake, proves cert ownership

- **AES-GCM =** bulk encryption of the actual traffic

- **SHA-2 =** integrity/hashing underneath it all
