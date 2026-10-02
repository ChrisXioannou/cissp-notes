# Domain 3: Security Architecture and Engineering

## Contents

- [Zero Trust and SASE](#zero-trust-and-sase)
- [Systems, applications and infrastructure](#systems-applications-and-infrastructure)
- [Cloud service models and responsibility](#cloud-service-models-and-responsibility)
  - [Essential characteristics of cloud computing](#essential-characteristics-of-cloud-computing)
- [Cloud security tooling](#cloud-security-tooling)
- [Virtualization](#virtualization)
- [Cryptography fundamentals](#cryptography-fundamentals)
  - [Choosing an algorithm: three questions](#choosing-an-algorithm-three-questions)
  - [Case-mapping checklist](#case-mapping-checklist)
  - [Post-quantum](#post-quantum)
- [Hashing and signatures](#hashing-and-signatures)
- [PKI](#pki)
- [Security models](#security-models)
- [The foundational three](#the-foundational-three)
  - [Harrison-Ruzzo-Ullman (HRU)](#harrison-ruzzo-ullman-hru)
  - [Model quick-reference additions](#model-quick-reference-additions)
- [Quick discriminators](#quick-discriminators)
- [TCB, security kernel and reference monitor](#tcb-security-kernel-and-reference-monitor)
- [Protection rings](#protection-rings)
- [Evaluation criteria](#evaluation-criteria)
- [Certification and accreditation](#certification-and-accreditation)
- [Covert channels](#covert-channels)
- [Access control models](#access-control-models)
  - [DAC in detail](#dac-in-detail)
  - [Nondiscretionary (the contrast that gets tested)](#nondiscretionary-the-contrast-that-gets-tested)
  - [RBAC extra worth holding](#rbac-extra-worth-holding)
  - [MAC extra](#mac-extra)
- [Trusted Platform Module](#trusted-platform-module)
- [Physical security](#physical-security)
  - [Server room and data centre design](#server-room-and-data-centre-design)
  - [Fire](#fire)
- [Mobile and BYOD](#mobile-and-byod)
- [Block cipher modes of operation](#block-cipher-modes-of-operation)
  - [The three facts that get tested](#the-three-facts-that-get-tested)
  - [IV, nonce, salt, pepper](#iv-nonce-salt-pepper)
- [Cryptographic attacks](#cryptographic-attacks)
- [Emerging cryptography](#emerging-cryptography)
- [Industrial and embedded systems](#industrial-and-embedded-systems)

---

## Zero Trust and SASE

**Zero Trust** is identity-based. No one is trusted by default. It evaluates whether a user should access something, every time.

> **Least privilege** means a user has the minimal permissions needed for their job.

**SASE** is an architecture for delivering networking plus security from the cloud. Similar in spirit to Zero Trust but a different thing.

- **SASE** = an architecture for delivering networking + security from the cloud

- **Zero Trust** = a security philosophy or model for deciding who or what gets access to what

**SASE** = cloud-delivered networking + security in one platform. Components: **SD-WAN** (networking), **SWG** (secure web gateway), **CASB** (cloud app visibility and policy), **FWaaS** (firewall as a service), **ZTNA** (zero trust network access).

**The shift it represents:** traditional model backhauls remote traffic to a central perimeter for inspection. SASE connects users directly to services via the nearest cloud edge. Lower latency, no hairpinning.

**SASE vs Zero Trust:** Zero Trust is the philosophy (never trust, always verify). SASE is the delivery architecture that implements it. ZTNA is the SASE component that does the access control.

## Systems, applications and infrastructure

- **IoT devices need security too.** Do not forget them

- **Embedded systems** are small computer systems built into devices to perform specific tasks. Many IoT devices (printers, drones) use embedded systems. Authentication and security practices apply to these too

- **Microservices** contain code. Always do static code analysis so you do not release code with vulnerabilities

- **Docker / containers** share one operating system while applications keep their isolation

#### APIs need to be secured

- **REST** is the concept of communication from an HTTP request to a URL with a JSON response

- **Firmware**, for example in a printer, is the software that makes it able to start, stop and operate

- **Process isolation** makes an application see only its own data

- **Layering** limits communication between components

- **ICS (Industrial Control System):** computers controlling physical industrial processes. SCADA (Supervisory Control and Data Acquisition) is the class that monitors and controls geographically distributed sites. Also DCS (Distributed Control System, single facility) and PLC (Programmable Logic Controller, the individual device). Security concerns: legacy protocols with no authentication, systems that cannot be patched or taken offline, and a breach causing physical harm to people and property. Primary countermeasure is network segmentation, up to full air gap

## Cloud service models and responsibility

**Model Your responsibility**

| SaaS | Just configure the features of the application. Provider handles everything else. Least security work on your part |
| --- | --- |
| PaaS | Platform as a service. You secure more of the platform as well |
| IaaS | Provider gives only the infrastructure. You secure the rest. Most security work |

- **Serverless:** you are not responsible for the backend. The provider handles it, including allocating more infrastructure

- **Private cloud:** virtualizing machines yourself, for example with Hyper-V

- Cloud nowadays can be more secure than on-premises hosting

> **SECaaS (Security as a Service):** security provided to an organization by an online entity.

### Essential characteristics of cloud computing

- **On-demand self-service:** you provision resources yourself, no ticket, no human

- **Broad network access:** reachable from anywhere, any device

- **Resource pooling:** provider's resources shared across tenants, multi-tenancy

- **Rapid elasticity:** scale up and down with demand

- **Measured service:** metered usage, pay for what you use

Signals:

- "provision without contacting anyone" means on-demand self-service

- "scale up during peaks, down after" means rapid elasticity

- "multi-tenant, shared hardware" means resource pooling

- "billing, metering, chargeback" means measured service

- "access from any device anywhere" means broad network access

## Cloud security tooling

**Tool Watches Question it answers**

| CASB | Users accessing cloud services | Who is using what, and may they do that with this data? |
| --- | --- | --- |
| CSPM | Configuration of your cloud tenant | Is my cloud set up correctly and consistently? |
| CWPP | Workloads : VMs, containers, serverless | Is what is running inside secure? |
| CNAPP | All of the above in one platform |  |

- **CASB** sits between users and cloud services to provide security controls. Helps organizations enforce security policies when employees use cloud applications. It does **not** replace the cloud provider's security; it enforces *your* policies on *your* usage

- **CSPM** flags a public storage bucket, an over-permissive IAM role, unencrypted storage, a security group open to 0.0.0.0/0. Continuous configuration scanning against benchmarks like CIS. **Google SCC is a CSPM**

- **CWPP** flags a vulnerable container image or malware running in a VM

| **Stem says Tool**                                                             |          |
|--------------------------------------------------------------------------------|----------|
| Cloud + policy enforcement + visibility + shadow IT + DLP                      | **CASB** |
| misconfiguration, configuration drift, compliance posture, consistent settings | **CSPM** |
| container, VM, workload, runtime protection                                    | **CWPP** |

## Virtualization

> **Hypervisor (Virtual Machine Monitor, VMM)** splits into two categories:

1.  **Bare-metal (Type 1):** installs directly onto the hardware of the host. No operating system between

2.  **Hosted (Type 2):** installs on top of the host's operating system

## Cryptography fundamentals

- **Cipher** = the algorithm or method used to transform data

- **Encryption** = the process of using a cipher with a key to turn plaintext into ciphertext

> **Cryptography does not always provide confidentiality** (hashing does not). **Ciphers always do**, because they are meant to hide data.

- **Stream cipher:** one character at a time

- **Block cipher:** blocks ciphered at a time

- **Work factor:** the time required to break the chosen security control

- **Key clustering:** two different keys produce the same ciphertext. This is bad and should not happen

> **IV (initialization vector):** used in encryption, sent along with every ciphertext. Not used for hashing

#### Symmetric vs asymmetric:

- **Symmetric** is faster and stronger per bit, but has a key distribution problem: n parties need n(n-1)/2 keys. It **cannot** provide non-repudiation, because a shared key means either party could have done it

- **Asymmetric** solves key distribution and enables signing and non-repudiation, but is slow and not for bulk data

> **Non-repudiation comes from asymmetric cryptography only**, specifically digital signatures. It is not a property of cryptography in general.

#### One-way and two-way:

- **One-way** = hash = integrity, password storage, irreversible

- **Two-way** = encryption = confidentiality, reversible with a key

> **Modern ciphers (AES, RSA, ECC) are computationally secure, not theoretically unbreakable.** Do not compare them to classical ciphers. Classical ciphers are all breakable except the Vernam cipher (one-time pad).

### Choosing an algorithm: three questions

#### Confidentiality of bulk data means symmetric

- Fast, low overhead, good for large volumes: disk encryption, VPN bulk data, database fields

- Cue words: "encrypt the file/database/large volume", "performance", "bulk data", "speed"

- Algorithms: AES (default answer), 3DES (legacy, avoid on new deployments)

**Key exchange, digital signatures, non-repudiation, small data means asymmetric**

- Cue words: "non-repudiation", "digital signature", "key exchange", "identity verification", "small amount of data", "authentication of sender"

- Algorithms: RSA, ECC (smaller keys, less compute, mobile and IoT), Diffie-Hellman (key exchange only, no encryption or signing)

#### Both needed simultaneously means hybrid

- Asymmetric encrypts a symmetric session key; symmetric encrypts the actual data

- The real-world default: TLS/SSL, PGP, S/MIME, SSH

- Cue words: "secure channel", "TLS", "email encryption", "need speed AND secure key exchange"

### Case-mapping checklist

| **Requirement in the scenario**                   | **Answer**                               |
|---------------------------------------------------|------------------------------------------|
| Encrypt large file or database, speed matters     | Symmetric (AES)                          |
| Prove sender identity, non-repudiation            | Asymmetric (digital signature)           |
| Exchange keys over an insecure channel            | Diffie-Hellman or asymmetric wrapping    |
| Secure a whole communication session              | Hybrid                                   |
| Verify data was not altered, no encryption needed | Hashing (SHA-2/3), not encryption at all |
| Verify integrity and authenticity together        | HMAC or digital signature                |

**Common trap:** questions naming a hash function (MD5, SHA) as if it were encryption. Hashing is one-way, not encryption. If the scenario says "integrity" rather than "confidentiality", you are in hashing/HMAC territory.

### Post-quantum

- **Symmetric cryptography holds up better against quantum** (Grover's algorithm only halves effective key length)

- **Asymmetric such as RSA is weak against quantum** (Shor's algorithm)

- **Lattice-based cryptography** is the family of algorithms designed to stay secure against quantum computers. It is a family, not a single algorithm. The NIST standards are **ML-KEM (Kyber)** for key encapsulation and **ML-DSA (Dilithium)** for signatures

## Hashing and signatures

> **A hash function must take an input of any length and produce an output of the same fixed size.** It must be collision resistant and one-way.
>
> **Salting happens before hashing** to enhance security.

**Hashing detects changes, so it provides accuracy as well as integrity**

> **Digital signatures require both hashing and public key cryptography.** They use SHA-2 with RSA, DSA or elliptic curves (ECDSA).

- **DSS (Digital Signature Standard)** works with DSA, RSA or elliptic curve algorithms

- **Zero-knowledge proof:** proving you know a secret without revealing the secret itself. This is **not** what digital signatures do. Keep the two separate

#### Attacks on hashes and passwords:

- **Rainbow tables** accelerate brute-force attacks against password hashes by precomputing hash values

- **Birthday attacks** take advantage of two hashes colliding

## PKI

**PKI is a system**, not a single application: policies, roles, hardware, software and procedures that bind an identity to a public key using certificates.

> **PKI does not generate your public key.** You generate your own keypair. PKI's job is to **vouch** that a given public key belongs to a given identity, by signing a certificate.

PKI provides a way to secure sensitive data during transmission, using digital certificates and keys.

| **Component Role**                            |                                                                                           |
|-----------------------------------------------|-------------------------------------------------------------------------------------------|
| **Certificate Authority (CA)**                | Issues and signs certificates. The trust anchor                                           |
| **Registration Authority (RA)**               | Verifies your identity **before** the CA issues. Registration, not signing                |
| **Repository / directory**                    | Where certificates are published, often LDAP                                              |
| **Certificate Revocation List (CRL)**         | Certificate Revocation List. A periodically published list of revoked certs. Can be stale |
| **Online Certificate Status Protocol (OCSP)** | Online Certificate Status Protocol. Real-time revocation check, adds a lookup             |
| **Certificate Policy (CP)**                   | Certificate Policy and Certification Practice Statement, the governing documents          |
| **Key escrow**                                | Copies of private keys held for recovery                                                  |

> **Certificate lifecycle:** generate keypair, create **CSR**, RA verifies identity, CA signs, publish, use, renew or **revoke**, expire.

**X.509** is the certificate format: subject, public key, validity dates, issuer, CA signature.

**Deployment types:** internal/private PKI (AD Certificate Services, EJBCA, Vault), public PKI (DigiCert, Sectigo, Let's Encrypt, roots pre-installed in browsers), cloud managed (AWS Private CA, Google CA Service, Azure Key Vault).

> **Standard architecture:** an **offline root CA**, physically disconnected, powered on only to sign subordinates. Then online **issuing CAs** for daily work. Compromise an issuing CA and you revoke it. Compromise the root and the whole PKI is dead.

**Trust models:** hierarchical (root, subordinates, end entities: the standard), mesh (CAs cross-certify peer to peer), bridge (a bridge CA links separate hierarchies), web of trust (no CA, users vouch for each other: PGP).

> **PGP (Pretty Good Privacy):** standard for encrypting and signing email and files.

## Security models

## The foundational three

| **Model**        | **Built on**      | **What it says**                                                                                                                                                                                                                                                                         |
|------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| State machine    | Nothing. The base | A state is a snapshot of the system. If every aspect satisfies policy, the state is secure. A transition is what happens on input or output. Must boot secure, stay secure across every transition, and allow access only in policy-compliant ways. Next state = F(input, current state) |
| Information flow | State machine     | Secure state defined by permitted direction of information flow. Also handles covert channels by excluding all undefined flow pathways                                                                                                                                                   |
| Noninterference  | Information flow  | Actions of a higher-level subject must not affect the system state or actions of a lower-level subject. Isolation                                                                                                                                                                        |

A **security model** is a way to formalize a security policy. It determines how security will be implemented and who accesses what. It moves from abstract policy to actual written implementation.

- **State machine model:** whatever state a system is in, it must be secure. Every snapshot is secure at any time

- **Information flow model:** based on the state machine model, focuses on the flow of information

- **Non-interference model:** based on the information flow model. Actions of different subjects and objects must not interfere with one another

<!-- -->

- The Bell-LaPadula and Biba models are built on the state machine model.

**Model Focus Rules and trigger words**

| Bell-LaPadula | Confidentiality | No read up, no write down. Military and classification. Trigger words: secret, classified, clearance, leakage, disclosure |
| --- | --- | --- |
| Biba | Integrity | No read down, no write up. Trigger words: integrity, data corruption, trusted vs untrusted source, low-integrity input |
| Clark-Wilson | Integrity , commercial and financial | The access control triple : subject, program (transformation procedure), object. Only through a transformation procedure can you change a trusted item. Where Biba is theoretical, Clark-Wilson is practical |
| Brewer-Nash (Chinese Wall) | Conflict of interest | Access permissions change dynamically based on what you have already accessed. Touch Client A's data and Client B's becomes off-limits. Trigger words: consultant, contractor, competitors, unbiased |
| Graham-Denning | Administration of rights | Eight primitive operations. How subjects, objects and rights get created, deleted and transferred. Trigger words: create subject, delete object, grant/transfer rights, ownership |
| Lattice-based | Access limits | Manages what a subject or object should have access to |
| Non-interference | Isolation | Actions of one subject do not affect another |
| Take-Grant | How rights propagate | Uses a directed graph. Four rules: take (subject takes a right from another subject), grant (subject grants any right it possesses), create (generate new rights), remove (delete rights it has). The point is to work out when rights can change and where leakage (unintentional distribution of permissions) can occur |
| Sutherland | Integrity | Prevents interference in support of integrity. Formally based on the state machine and information flow models. Does not specify protection mechanisms; defines a set of system states, initial states and state transitions, and permits only those. Common example: preventing a covert channel from influencing the outcome of a process |
| Goguen-Meseguer | Integrity | The foundation of noninterference theory. When someone says "the noninterference model" they often mean this. Based on automation theory and domain separation : subjects may perform only predetermined actions against predetermined objects. Members of one subject domain cannot interfere with another |

#### Bell-LaPadula row:

**Simple Security Property = no read up Star (\*) Property = no write down (also called the confinement property) Strong Star Property = read and write only at your own level Discretionary Security Property = uses an access matrix Does not address covert channels**

#### Biba row:

**Simple Integrity Property = no read down Star (\*) Integrity Property = no write up Invocation Property = a subject at one integrity level cannot invoke a subject at a higher integrity level**

#### Terminology:

- **Subjects** are persons, groups, individuals, organizations

- **Objects** are resources, computers, applications

> **Quality of data means integrity.** If a stem asks for models focused on data quality, it wants **Biba and Clark-Wilson**.

**Composition theories:** two systems that are each secure can be insecure when combined. Security properties do not automatically survive integration.

| **Theory Shape** |                                                                             |
|------------------|-----------------------------------------------------------------------------|
| **Cascading**    | A's output becomes B's input. One direction                                 |
| **Feedback**     | A feeds B, and B feeds back to A                                            |
| **Hookup**       | A talks to B **and** to an external entity. The outside channel is the risk |

"Iterative" is **not** a composition theory. That is the usual distractor.

### Harrison-Ruzzo-Ullman (HRU)

Extends the access matrix by governing **how rights themselves are changed**, not just who holds them. Defines primitive operations: create subject, create object, enter right, delete right, destroy subject, destroy object.

Its famous result is that the **safety question** (can this subject ever obtain this right?) is **undecidable** in the general case.

**Cue:** the stem is about changing or propagating access rights, or about proving that a right can never leak. Compare with Take-Grant, which also models rights propagation but is decidable.

### Model quick-reference additions

| Model | One-line purpose |
| --- | --- |
| **Harrison-Ruzzo-Ullman** | Rules for changing access rights, safety is undecidable |
| **Take-Grant** | Rights propagation using take, grant, create, remove |
| **Graham-Denning** | Eight primitive protection rights for creating and deleting subjects and objects |
| **Noninterference** | Actions at a high level must not be observable at a low level |
| **Information flow** | Tracks how data moves between security levels, the basis of covert-channel analysis |

**Noninterference is the model behind covert-channel concerns.** If a high-level action changes anything a low-level subject can observe, information has flowed.

## Quick discriminators

| Stem trigger | Model |
| --- | --- |
| No read up, no write down, clearance, disclosure | Bell-LaPadula |
| No read down, no write up, low-integrity input | Biba |
| Access control triple, transformation procedure, CDI/UDI/IVP/TP, commercial integrity | Clark-Wilson |
| Data quality (asks for models, plural) | Biba and Clark-Wilson |
| Consultant, competitors, conflict of interest, access changes based on what you already touched | Brewer-Nash |
| Create subject, delete object, transfer rights, ownership | Graham-Denning |
| How rights are passed from one subject to another, leakage | Take-Grant |
| Foundation of noninterference, domain separation, automation theory | Goguen-Meseguer |
| Prevents interference, defined set of states, stops a covert channel influencing a process | Sutherland |
| Actions of one subject do not affect another | Noninterference (or Goguen-Meseguer) |
| Every snapshot of the system must be secure | State machine |
| Table of subjects and objects; column vs row | Access control matrix (ACL vs capability list) |

## TCB, security kernel and reference monitor

- **TCB (Trusted Computing Base):** the combination of controls and hardware that form the trusted base

- **Security kernel:** creates and implements the security controls

- **Reference monitor:** the logical component that actually enforces, checking whether an access is allowed

#### Three properties the reference monitor must have

- **Complete mediation:** cannot be bypassed, every access checked

- **Tamperproof:** cannot be modified

- **Verifiable:** small and simple enough to prove correct

**Security kernel's job:** launch and run the components that deliver reference monitor functionality, resist all known attacks, mediate all resource access, use trusted paths to talk to subjects.

## Protection rings

**Ring Contains Used in practice?**

|  |  |  |
| --- | --- | --- |
| 0 | OS kernel, most privileged | Yes |
| 1 | Other OS components, device drivers | Rarely. This is the exam answer for "not normally implemented" |
| 2 | Drivers, protocols, I/O | Rarely |
| 3 | Applications, user mode, least privileged | Yes |

- Lower number means more privileged

- Real systems collapse to two: **kernel mode (0)** and **user mode (3)**

> **Ring 0 aliases:** privileged mode, supervisory mode, system mode, kernel mode.
>
> **User mode is Ring 3**, never Ring 0. Classic distractor.
>
> Crossing rings requires a system call. **TCB, security kernel and reference monitor all live at ring 0.**

## Evaluation criteria

> **Common Criteria (ISO/IEC 15408)** is the gold standard. **TCSEC** and **ITSEC** have been largely replaced by it.

**TCSEC trusted system ratings, four divisions, low to high:**

**Level Name Key feature**

| D | Minimal protection | No security requirements |
| --- | --- | --- |
| C1 | Discretionary security | Basic access control, users separate their own data |
| C2 | Controlled access | Individual accountability, audit logs, object reuse protection |
| B1 | Labeled security | Mandatory access control via labels |
| B2 | Structured protection | MAC on all objects, covert channel analysis starts here, trusted path for login |
| B3 | Security domains | Tamperproof reference monitor, real-time monitoring |
| A1 | Verified protection | Formally verified design matches implementation |

**Common Criteria (ISO/IEC 15408)**

Structure: Part 1 Introduction and General Model, Part 2 Security Functional Requirements, Part 3 Security Assurance.

Two key elements of the process:

- Protection Profile (PP): the customer's security requirements. The "I want"

- Security Target (ST): the vendor's claims of security built into the product. The "I will provide"

- TOE (Target of Evaluation): the product being evaluated

- Package: an intermediate grouping of security requirements that can be added to or removed from a TOE

Process: the PP is compared against various STs, and the client buys the closest match.

**Three parts: Part 1 Introduction and General Model, Part 2 Security Functional Requirements, Part 3 Security Assurance**

- **TOE (Target of Evaluation):** the product being evaluated

- **PP (Protection Profile):** the customer's requirements. The "I want"

- **ST (Security Target):** the vendor's claims. The "I will provide"

- **Process:** the org's PP is compared against vendor STs, closest match purchased

| EAL | Assurance |
| --- | --- |
| 1 | Functionally tested |
| 2 | Structurally tested |
| 3 | Methodically tested and checked |
| 4 | Methodically designed, tested and reviewed ← commercial ceiling |
| 5 | Semi-formally designed and tested |
| 6 | Semi-formally verified design and tested |
| 7 | Formally verified design and tested |

**Higher EAL = more rigorously evaluated, not more secure. CC evaluates functionality and assurance separately, unlike TCSEC which combined them into one rating**

**Two traps worth noting, since you now have both tables side by side:**

**A1 in TCSEC and EAL7 in CC both mean formally verified. Different scales, same concept**

**If a stem gives an EAL number and asks whether a requirement is met, check whether it needs formal verification. That's 7 only**

## Certification and accreditation

- **Certification:** technical evaluation confirming a product meets documented requirements. Done by engineers

- **Accreditation:** management formally accepts the residual risk and authorizes use. Formal acceptance, not a technical test

**C before A**, alphabetically and chronologically. Accreditation is time-bound; significant system changes require reaccreditation. In NIST RMF this is the "Authorize" step.

- **QA** is process-focused, prevents defects, happens throughout development

- **QC** is product-focused, inspects and tests the finished item

#### Accreditation types (three)

- **System =** a major application or general support system evaluated

- **Site =** applications and systems at a specific, self-contained location

- **Type =** an application or system distributed to a number of different locations

**Trigger words:** "self-contained location" → Site. "distributed to many locations" → Type. Single app/system → System.

## Covert channels

A method of passing information along a path not normally used for that purpose, so that the security controls do not apply. Example: steganography.

A cascading composition is a common way a covert channel appears, because the connection creates a path neither system designed for.

## Access control models

**Model Who controls access? Example**

| MAC (Mandatory) | The system, via labels | Military "Top Secret" files |
| --- | --- | --- |
| DAC (Discretionary) | The owner or creator | Sharing your own Drive file with friends |
| Non-discretionary | Organization or system rules | HR staff can only access the HR database |
| Rule-based | Conditions and rules | Firewall allows access only from company IPs |
| RBAC (Role-based) | Role assignment | Job function determines access |
| ABAC (Attribute-based) | Attributes of subject, object, environment | Flexible policy expressions |

In the **MAC** model every object and subject has a label attached. A central authority assigns security labels (Secret, Top Secret) to objects and clearance levels to subjects. Access is granted only if the subject's clearance matches or exceeds the object's classification. Used in military and government systems.

> One-liner: **ABAC means flexible rules with attributes. MAC means strict labels controlled by the system. Separation of privilege:** I cannot be the one granting rules to myself. Duties must be separated.

**Casing quirk the book flags:** ISC2's outline writes rule-based access control in lowercase with no acronym, while DAC, RBAC, ABAC, MAC are capitalized with acronyms. Minor, but it's how they distinguish rule-based from the rest.

### DAC in detail

The owner, creator, or data custodian of an object controls and defines access to that object. Access is based on the discretion of the owner.

- Create a spreadsheet, you are creator and owner, you modify permissions to grant or deny other users.

- Data owners can delegate to data custodians, giving custodians the ability to modify permissions.

- Identity-based access control is a subset of DAC, because systems identify users by identity and assign resource ownership to identities.

**Implementation:** ACLs on objects. Each ACL defines the types of access granted or denied to subjects.

**Its defining weakness:** no centrally controlled management system, because owners can alter ACLs on their objects at will.

**Its defining strength:** flexibility. Access is easy to change, especially compared to the static nature of MAC. Administrators can easily suspend privileges when users go on vacation, and easily disable accounts when users leave.

### Nondiscretionary (the contrast that gets tested)

The major difference is how they are controlled and managed.

|                       | **DAC**                   | **Nondiscretionary**                                |
|-----------------------|---------------------------|-----------------------------------------------------|
| **Who administers**   | Owners, individually      | Administrators, centrally                           |
| **Scope of a change** | Only that owner's objects | Can affect the entire environment                   |
| **Focus**             | User identity             | Static set of rules governing the whole environment |
| **Flexibility**       | High                      | Lower                                               |
| **Manageability**     | Harder                    | Easier                                              |

**Book's catch-all:** any model that isn't discretionary is nondiscretionary.

### RBAC extra worth holding

RBAC enforces least privilege by preventing privilege creep, the tendency for users to accrue privileges over time as their roles change. When privileges are assigned to users directly, it's hard to identify and revoke everything they no longer need. With groups, you remove the account from the group and the privileges vanish immediately.

Also called task-based access control in the book.

### MAC extra

When documented in a table, MAC sometimes resembles a lattice, hence lattice-based model. "Lattice-based" is therefore not a sixth model, it is a description of how MAC looks on paper.

#### Trigger words

- "owner decides", "creator", "grant to whoever I choose", "NTFS", "share my file" → **DAC**

- "job function", "groups", "new hire gets the same access as peers", "privilege creep" → **RBAC**

- "applies to everyone equally", "firewall", "restrictions", "filters" → **rule-based**

- "multiple attributes", "plain language policy", "software-defined network" → **ABAC**

- "labels", "clearance", "classification", "lattice" → **MAC**

- "centrally administered", "changes affect the whole environment" → **nondiscretionary (any non-DAC)**

## Trusted Platform Module

**TPM (Trusted Platform Module):** used for full disk encryption, communicates directly with the motherboard. It is a hardware root of trust and works with certificates.

## Physical security

Physical controls are really important and break into categories:

1.  **Logical / technical controls:** access controls, intrusion detection, alarms

2.  **Administrative controls:** facility construction, personnel controls

3.  **Physical controls:** fencing, guards, dogs

Good temperatures, wiring and fire safety all matter.

Restricted areas and visitor areas should be defined, and the places that need heavy protection should get much more than the rest.

**CPTED (Crime Prevention Through Environmental Design)**

Designing the physical environment so it discourages crime and reduces fear of crime. Design shapes behaviour before any guard or camera is involved.

Three elements:

- **Natural surveillance**: make people and activity visible. Low shrubs, open sightlines, lighting, windows overlooking entrances, glass stairwells. Offenders avoid places where they can be seen.

- **Natural access control**: guide movement using layout rather than barriers. Single obvious entrance, pathways, landscaping, low fences, signage. Makes unauthorised routes feel wrong.

- **Territorial reinforcement**: make ownership obvious. Boundary markers, paving changes, company logos, maintained landscaping. Signals that the space is watched and cared for.

Exam cue: if a stem describes **design, layout, landscaping, sightlines, or discouraging crime**, it's CPTED. If it describes a device that stops someone, it's a barrier control.

CPTED is a **deterrent** approach, not a preventive one. It reduces the likelihood people attempt something, it does not stop anyone.

Collect some form of logging. Even CCTV is logging if you can review an intrusion afterwards.

> **UPS is a security measure.** It cleans the power before it arrives.

| **Term Purpose**                       |                                                                         |
|----------------------------------------|-------------------------------------------------------------------------|
| **Turnstile**                          | One person, **one direction**. Restricts flow into or out of a facility |
| **Mantrap / access control vestibule** | Two doors, first closes before the second opens. Stops **tailgating**   |
| **Gate**                               | Opening in a fence. Wide entry point, often vehicles. No flow control   |
| **Fence**                              | Perimeter barrier. **Deterrent only.** Does not stop anyone             |
| **Bollard**                            | Post stopping **vehicles**, not people on foot                          |
| **Sally port**                         | Controlled two-gate passage, often for vehicles                         |
| **Crash bar / panic bar**              | Exit device, safety over security                                       |
| **Cipher lock**                        | Keypad, code-based entry. Preventive access control                     |

#### Discriminators:

- "Restrict access into or out of" (flow, direction) means **turnstile**

- "Prevent someone following an authorised person through" means **mantrap**

- "Stop a vehicle" means **bollard**

- Fence is always a **deterrent**. If the stem says stop, prevent or restrict, fence loses

- Cipher lock is the only one that **authenticates**. Fences and bollards deflect, they do not distinguish authorised from unauthorised

### Server room and data centre design

**Same category:** server rooms, data centers, communications rooms, wiring closets, server vaults, IT closets. All enclosed, restricted, protected rooms housing mission-critical servers and network devices.

**Core principle:** human incompatibility is a feature. The more human incompatible, the more protection against casual and determined attacks. Achieved via:

- Halotron, PyroGen, or other halon-substitute oxygen-displacement fire systems

- Low temperatures

- Little or no lighting

- Equipment stacked with little room to maneuver

**Design goal:** support optimal IT operation and block unauthorized human access or intervention.

#### Placement

- At the core of the building

- Avoid ground floor, top floor, and basement

- Away from water, gas, and sewage lines (leak/flood risk)

- Walls need a one-hour minimum fire rating

**Vaults:** the book's example is storing machines inside an actual bank vault. Short of that, the substitute is an interior room with limited access, no windows, one entry/exit point. Add CCTV on the door and motion detectors inside.

#### Environment

| **Factor**      | **Range**               |
|-----------------|-------------------------|
| **Temperature** | 60 to 75°F (15 to 23°C) |
| **Humidity**    | 40 to 60 percent        |

- Too much humidity → **corrosion**

- Too little humidity → **static electricity**

- Even on antistatic carpeting, low humidity can generate 20,000-volt discharges

**Note the trap:** 40 volts is the lowest value and destroys components. Low numbers do serious damage.

#### Water issues

- Locate server rooms away from water sources and transport pipes

- Install water detection circuits on the floor around mission-critical systems

- Know shutoff valves and drainage locations

- Assess flooding history, drainage, hill vs valley, basement vs first floor

### Fire

#### Fire triangle

**Three corners:** heat, oxygen, fuel. Center = the chemical reaction. Remove any one and the fire is extinguished.

| **Medium**                                 | **Removes**                           |
|--------------------------------------------|---------------------------------------|
| **Water**                                  | Temperature (heat)                    |
| **Soda acid / dry powders**                | Fuel supply                           |
| **CO2**                                    | Oxygen                                |
| **Halon substitutes / nonflammable gases** | Chemistry of combustion and/or oxygen |

#### Four stages of fire

- Incipient - air ionization only, no smoke

- Smoke - smoke visible from point of ignition

- Flame - flame visible to the naked eye

- Heat - intense heat buildup, everything in the area burns

**Earlier detection =** easier extinguishing = less damage. Fire extinguishers are only used during the incipient stage.

#### Fire extinguisher classes

| **Class** | **Type**            | **Suppression material** |
|-----------|---------------------|--------------------------|
| **A**     | Common combustibles | Water, soda acid         |
| **B**     | Liquids             | CO2, halon\*, soda acid  |
| **C**     | Electrical          | CO2, halon\*             |
| **D**     | Metal               | Dry powder               |

- Halon or an EPA-approved halon substitute

**Why the exclusions (this is where questions come from)**

- **No water on Class B:** splashes the burning liquid, and liquids float on water. Spreads the fire.

- **No water on Class C:** electrocution risk.

- **No oxygen suppression on Class D:** burning metal produces its own oxygen.

**Mnemonic:** Ashes, Boiling liquids, Current, Dense metal.

#### Fire detection systems

| **Type**                                 | **Trigger**                                                                                  |
|------------------------------------------|----------------------------------------------------------------------------------------------|
| **Fixed-temperature**                    | Specific temperature reached. Metal/plastic melts in sprinkler head, or glass vial vaporizes |
| **Rate-of-rise**                         | Speed of temperature change hits a threshold                                                 |
| **Flame-actuated**                       | Infrared energy of flames                                                                    |
| **Smoke-actuated**                       | Photoelectric or radioactive ionization sensors                                              |
| **Incipient smoke (aspirating sensors)** | Chemicals of very early combustion, before anything else can detect                          |

**Placement:** inside dropped ceilings and raised floors, server rooms, private offices, public areas, HVAC vents, elevator shafts, basement.

#### Water suppression systems (four types)

| **System**                 | **Behavior**                                                                                                                                                        |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Wet pipe (closed head)** | Always full of water. Immediate discharge                                                                                                                           |
| **Dry pipe**               | Contains compressed air. Air escapes, valve opens, pipes fill, then discharge                                                                                       |
| **Deluge**                 | Dry pipe with larger pipes, much more water. Inappropriate for electronics/computers                                                                                |
| **Preaction**              | Combination dry/wet. Dry until early fire signs detected, then pipes fill. Water releases only when sprinkler head trigger melts. Can be manually emptied and reset |

Preaction is the most appropriate water-based system for environments housing both computers and humans. Common exam answer.

**Most common cause of water system failure:** human error. Turning off the water source during a fire, or triggering release when there's no fire.

#### Why the others fail

- **Wet pipe:** always full, immediate discharge. One false trigger and everything is soaked. Zero intervention window. Also freeze risk in cold areas.

- **Dry pipe:** compressed air delays discharge slightly, but once triggered it fills and dumps. Single trigger condition. The delay is a side effect, not a safety feature.

- **Deluge:** dry pipe with larger pipes, delivers significantly more water. Book explicitly calls it inappropriate for environments containing electronics and computers. Worst possible choice here.

#### Watch the question wording

"Best type of water-based system" → **preaction.**

Drop "water-based" and ask for the best system generally → gas discharge, since gas is more effective than water and causes no water damage. But gas requires the room to be unoccupied, because it removes oxygen.

That qualifier is the whole question. If a stem doesn't say water-based and people are present, preaction is still your answer, since gas would asphyxiate them.

## Mobile and BYOD

> Mobile devices need securing as well. **Bring Your Own Device (BYOD)** may be useful but adds many risks to the company.

**Obfuscation:** deliberately making code hard to read while keeping it functional. Rename variables and methods to meaningless strings, flatten control flow, strip debug symbols, insert junk code. Raises the **work factor**. This is the answer for protecting mobile app IP from reverse engineering.

Obfuscation does not *prevent* reverse engineering, it slows it down. For IP protection, raising cost is the realistic goal.

> **Code signing** proves origin and integrity, not confidentiality. A signed app can still be decompiled. **Encryption of app code fails on hostile hardware** because the decryption key ships with the app. Related: anti-tampering and anti-debugging, root and jailbreak detection.

---

## Block cipher modes of operation

A block cipher encrypts one fixed-size block. The mode decides how multiple blocks chain together. Choosing the mode is a tested decision.

| Mode | Full name | Parallel | Needs IV/nonce | Integrity | Use it when |
| --- | --- | --- | --- | --- | --- |
| **ECB** | Electronic Code Book | Yes | No | No | Never, in practice |
| **CBC** | Cipher Block Chaining | Decrypt only | Yes (IV) | No | Legacy bulk encryption |
| **CFB** | Cipher Feedback | Decrypt only | Yes (IV) | No | Stream-like, legacy |
| **OFB** | Output Feedback | No | Yes (IV) | No | Noisy channels, errors do not propagate |
| **CTR** | Counter | Yes | Yes (nonce) | No | High-speed bulk encryption |
| **GCM** | Galois/Counter Mode | Yes | Yes (nonce) | **Yes** | The modern default |
| **CCM** | Counter with CBC-MAC | No | Yes (nonce) | **Yes** | Constrained devices, WPA2 |

### The three facts that get tested

**ECB is broken and you must be able to say why.** Identical plaintext blocks produce identical ciphertext blocks, so structure leaks. The classic demonstration is an encrypted bitmap where the original image is still visible. If a stem offers ECB, it is wrong.

**GCM and CCM are authenticated encryption (AEAD).** They give confidentiality *and* integrity *and* authenticity in one pass. Every other mode in the table gives confidentiality only and needs a separate MAC. If a stem wants both properties from one mechanism, the answer is GCM.

**Never reuse a nonce or IV with the same key in CTR or GCM.** Reuse in CTR exposes the XOR of two plaintexts. Reuse in GCM additionally leaks the authentication key, which is catastrophic.

### IV, nonce, salt, pepper

These four are routinely confused.

| Term | Secret? | Unique per? | Purpose |
| --- | --- | --- | --- |
| **IV** (initialisation vector) | No | Message | Randomise the first block so identical plaintexts differ |
| **Nonce** (number used once) | No | Message | Guarantee uniqueness, never reused with the same key |
| **Salt** | No | Password | Defeat rainbow tables, make identical passwords hash differently |
| **Pepper** | **Yes** | System | A secret added to passwords, stored separately from the database |

A salt is stored alongside the hash and is not secret. A pepper is secret and deliberately stored somewhere else, so a database dump alone is not enough to crack.

## Cryptographic attacks

| Attack | What the attacker has | Goal |
| --- | --- | --- |
| **Ciphertext-only** | Ciphertext | Hardest case for the attacker |
| **Known-plaintext** | Some plaintext/ciphertext pairs | Recover the key |
| **Chosen-plaintext** | Can encrypt text of their choosing | Recover the key |
| **Chosen-ciphertext** | Can decrypt text of their choosing | Recover the key |
| **Meet-in-the-middle** | - | Why 2DES gives almost no gain over DES |
| **Birthday attack** | - | Find any hash collision, not a specific one |
| **Rainbow table** | Hash database | Precomputed hash lookup. Defeated by salting |
| **Side channel** | Physical access to the device | Timing, power draw, EM emission, acoustic |
| **Fault injection** | Physical access | Induce an error (voltage glitch, laser) and read the faulty output |
| **Replay** | Captured traffic | Resend a valid message. Defeated by nonces and timestamps |
| **Downgrade** | Position in the handshake | Force a weaker protocol or cipher |

**Meet-in-the-middle is why 3DES exists.** Double DES gives roughly 57 bits of effective strength instead of 112, so the industry skipped to triple.

**Birthday attack is about collisions, not preimages.** The cue is "two different inputs producing the same hash". Mitigation is a longer hash output.

## Emerging cryptography

- **Homomorphic encryption:** compute on ciphertext without decrypting it. The cue is processing data while it stays encrypted, typically in an untrusted cloud
- **Quantum key distribution (QKD):** uses physics, not maths. Eavesdropping disturbs the quantum state and is therefore detectable
- **Blockchain:** a distributed, append-only ledger using hash chaining. Provides **integrity and non-repudiation**, not confidentiality. A question implying blockchain gives confidentiality is wrong
- **Steganography:** hiding the existence of a message inside a carrier file. Cryptography hides content, steganography hides the fact that there is content at all
- **Digital watermarking:** embedding ownership information, often used for DRM and leak attribution

---

## Industrial and embedded systems

| Term | Meaning |
| --- | --- |
| **ICS** | Industrial Control System, the umbrella term |
| **SCADA** | Supervisory Control and Data Acquisition, geographically distributed |
| **DCS** | Distributed Control System, confined to a single plant |
| **PLC** | Programmable Logic Controller, the device doing the actual control |
| **HMI** | Human Machine Interface, the operator's screen |
| **Historian** | Database recording process values over time |

**Why ICS security is different.** Priorities invert: **availability and safety come first, confidentiality last**. The opposite of standard IT. Patching may be impossible because downtime is unacceptable and vendors certify specific configurations.

**Compensating controls for ICS:** network segmentation, unidirectional gateways (data diodes), strict change control, monitoring instead of patching.

The **Purdue Model** describes ICS network layers, Level 0 (physical process) up to Level 5 (enterprise network), with a DMZ between IT and OT.
