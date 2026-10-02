# Domain 8: Software Development Security

## Contents

- [The SDLC](#the-sdlc)
- [Development models](#development-models)
- [SW-CMM (Capability Maturity Model for Software)](#sw-cmm-capability-maturity-model-for-software)
- [Requirements](#requirements)
- [Secure development practices](#secure-development-practices)
- [Databases](#databases)
- [Malware](#malware)
- [Application vulnerabilities](#application-vulnerabilities)
  - [Cross-site scripting (XSS)](#cross-site-scripting-xss)
  - [XSS vs CSRF](#xss-vs-csrf)
  - [Stored procedures](#stored-procedures)
- [Application vulnerabilities](#application-vulnerabilities)
  - [Buffer overflow](#buffer-overflow)
  - [Race conditions and TOCTOU](#race-conditions-and-toctou)
  - [Injection and the rest](#injection-and-the-rest)
  - [Sandboxing and isolation](#sandboxing-and-isolation)
  - [OWASP](#owasp)

---

## The SDLC

**Initiation, Requirements, Architecture, Design, Code, Test, Integrate, Deploy, Maintain, Dispose**

> **NIST SP 800-64 five-phase version:** Initiation, Development/Acquisition, Implementation/Assessment, Operations/Maintenance, Disposal.

**Sybex expanded version:** conceptual definition, functional requirements determination, control specifications development, design review, coding, code review walkthrough, system test review, maintenance and change management.

> **Requirements gathering is the most important step for secure software development.** Note that "most important" and "first" are different questions. Project initiation and scope definition come before requirements gathering.
>
> **Phrase-to-phase map.** The exam disguises the names:

| **Stem phrase**                                              | **Phase**    |
|--------------------------------------------------------------|--------------|
| define scope, **parameters**, boundaries, feasibility        | Initiation   |
| gather needs, stakeholder **input**                          | Requirements |
| security requirements, "MFA is vital"                        | Architecture |
| how we deliver functionality                                 | Design       |
| writing code, static analysis                                | Coding       |
| **systematic discovery**, debug, verify against requirements | Testing      |
| combine modules, assemble components                         | Integration  |
| patches, updates, support                                    | Maintenance  |
| decommission, data remanence                                 | Disposal     |

#### Distinguish these two:

- Sponsor authorises the project (a charter, permission to start) is **Initiation**

- "Acquire stakeholder input" is requirements elicitation, which is **Requirements**

Input is not approval.

## Development models

- **Waterfall:** sequential development. Each phase completes before the next. Has seven steps and follows them exactly. Identifiers: linear, sequential, structured, phases do not overlap

- **Agile:** places emphasis on customer needs. Working software over documentation, customer first, responding to change. Identifiers: iterative, customer feedback

- **Spiral:** several iterations of waterfall, which makes it more flexible. Works in a circle with risk analysis each loop

- **Prototyping:** build a working model early and refine

**IDEAL model** (SEI software process improvement): Initiating, Diagnosing, Establishing, Acting, Learning.

## SW-CMM (Capability Maturity Model for Software)

**Level Name Characteristic**

|  |  |  |
| --- | --- | --- |
| 1 | Initial | Ad hoc, chaotic, heroics. No process |
| 2 | Repeatable | Basic project management, can repeat past successes. Tracks cost and schedule |
| 3 | Defined | Documented, standardised process across the organisation |
| 4 | Managed | Quantitative metrics, measured process, statistical control |
| 5 | Optimizing | Continuous improvement, feeds measurements back into the process |

Mnemonic: **I R D M O**, "Initially, Repeat Definitely, Measure, Optimize."

#### The tested pair, Defined vs Managed:

- Defined means the process is written down

#### Managed means we have numbers about the process

- The word **quantitative** in a stem means level 4, every time

**Managed vs Optimizing:** Managed measures. Optimizing *acts* on the measurements. Cue words "continuous improvement" or "feedback loop" mean level 5.

CMMI is the modern successor. IDEAL is the related SEI implementation model.

Book's exact wording for level 4: **the organization uses quantitative measures to gain a detailed understanding of the development process.** The word quantitative is the entire tell.

**Cue words:** "quantitative", "metrics", "statistical" → 4. "Continuous improvement", "feedback loop" → 5. "Documented", "standardised across the organisation" → 3.

- **Inheritance**: methods from a parent class are inherited by a subclass

- **Polymorphism**: an object responds with **different behaviors to the same message** because of changed external conditions

- **Polyinstantiation**: **multiple versions of the same data item at different classification levels**, so a low-clearance user can't infer that higher-level data exists. **The inference countermeasure**

- **Cohesion**: strength of relationship between methods **within** a class. **High cohesion is good**

- **Coupling**: level of interaction **between** objects. **Low coupling is good** (more independent, easier to maintain)

- **Class**: collection of methods defining object behavior. **Instance**: a specific object created from a class

## Requirements

**Functional** = what the system **does**. A feature you build, test and demo. Test: can you write it as "the system shall \[verb\]"?

Examples: authentication, authorization, encryption of a specific field, audit logging, password reset, reporting, data validation, session timeout.

> **Non-functional** = how **well** it does it. A property or constraint on the whole system.

Examples: scalability, availability, reliability, capacity, performance, usability, maintainability, portability, recoverability, interoperability.

Shortcut: if it ends in **-ility**, it is usually non-functional.

Security requirements can land in both categories. "Users must authenticate with MFA" is functional (a feature). "The system must be available 99.9%" is non-functional. The category depends on whether it is a discrete buildable thing or a system-wide property.

## Secure development practices

- **CI/CD:** continuous integration, continuous delivery. Release versioning, automated vulnerability scanning, MFA. Staying secure continuously

- **SCM (Software Configuration Management):** snapshots and versioning help

- **DAST:** dynamic application security testing, testing the software during runtime

- **SAST:** static analysis on source code before release

- **Security must be built in throughout, not bolted on at one stage.** Perpetual analysis throughout the project beats any single-phase control

## Databases

> **RDBMS:** tables with relationships. A **foreign key** always connects to a **primary key**. **ACID properties:**

- **Atomicity:** all or nothing. A transaction fully completes or fully rolls back

- **Consistency:** the database moves from one valid state to another, rules never violated

- **Isolation:** concurrent transactions do not interfere with each other

- **Durability:** once committed, it survives a crash or power loss. **Transaction logs are how durability is implemented**

> **Concurrency control is NOT an ACID property.** It is *how* **isolation** is implemented, via **locking**.

- **Pessimistic locking:** lock the record before editing, others wait

- **Optimistic locking:** allow edits, detect conflict at commit time, reject the loser

Problems it solves: **lost update** (two writes, one overwrites the other), **dirty read** (reading uncommitted data), **deadlock** (two transactions each waiting on the other's lock).

#### Integrity types:

- **Entity integrity:** the primary key is unique and not null. Two records returned for one unique identifier means an entity integrity violation

- **Referential integrity:** a foreign key must point to a valid primary key

- **Semantic integrity:** data conforms to the defined type and rules

**Normalization** is the process of organizing database tables to remove redundancy and dependency issues.

> **NF level = how strictly attributes depend only on the primary key. Higher means stricter dependency rules and less redundancy, not raw speed.**

#### SQL sub-languages:

**Acronym Full name Does**

| DDL | Data Definition Language | Defines structure: CREATE, ALTER, DROP |
| --- | --- | --- |
| DML | Data Manipulation Language | Manipulates data: INSERT, UPDATE, DELETE, SELECT |
| DCL | Data Control Language | Permissions: GRANT, REVOKE |
| TCL | Transaction Control Language | COMMIT, ROLLBACK |

#### Database attacks:

- **Aggregation attack:** creating sensitive information by combining non-sensitive pieces

- **Inference attack:** deducing sensitive information from non-sensitive data by mapping pieces to each other

## Malware

#### Virus types:

- **File infector:** attaches to an executable, runs when the program starts

- **Service injection:** injected into a trusted Windows service process

- **Boot sector:** loads into memory at boot

- **Macro infection:** code inside small application scripts, for example in documents

> **Antivirus:** signature-based plus behaviour-based.
>
> **Shrink-wrap code attack:** exploits holes in unpatched or poorly configured off-the-shelf software.

## Application vulnerabilities

> **TOCTOU (Time-of-check-to-time-of-use):** a program checks access permissions too far in advance of using them, so state changes in between.

### Cross-site scripting (XSS)

An injection attack where script is injected into a web page and runs in the victim's browser.

**Type Where the payload lives How it fires**

| Stored (persistent) | Saved on the server: database, comment field, profile | Every visitor to that page is hit, no link needed |
| --- | --- | --- |
| Reflected (non-persistent) | In the request itself, usually a URL parameter | Victim must click a crafted link, fires once |
| DOM-based | Never reaches the server, client-side JavaScript only | The page's own JS writes attacker-controlled data into the DOM |

Variants sometimes mentioned: blind XSS (stored, fires in a backend panel), self-XSS (social engineering the victim into pasting it), mutation XSS.

### XSS vs CSRF

- **XSS:** the attacker runs **script** in the victim's browser. Goal is usually stealing session data or defacing

- **CSRF:** the attacker makes the victim's browser **send a request** the user did not intend. No script needed on the target site. Works precisely *because* the victim is already authenticated and the browser attaches cookies automatically

**Exam cue for CSRF:** an authenticated user, an action they did not authorise (transfer, password change, purchase), and their credentials or session were valid throughout.

#### Defences split cleanly:

- **XSS:** output encoding and input validation

- **CSRF:** anti-CSRF tokens and SameSite cookies

**The discriminators:**

- Payload **saved server-side**, affects all visitors → **stored**

- Payload **in the URL**, victim must click → **reflected**

- **Never touches the server**, client-side JS only → **DOM-based**

Stored is the most severe because it needs no social engineering and hits everyone.

**Variants worth recognising:** blind XSS (stored, fires in a backend admin panel), self-XSS (tricking the victim into pasting it themselves), mutation XSS.

**Input validation vs output encoding:** input validation rejects bad input on the way in. Output encoding neutralises it on the way out. Output encoding is the more reliable of the two for XSS specifically, because the same data may be safe in one context and dangerous in another.

**Root cause beats compensating control.** Secure coding (input validation, output encoding) removes the flaw. A WAF filters attacks against a flaw still present in the code. The code fix wins unless the stem says you cannot change the code (legacy, third party, need protection immediately while a fix is developed).

Ranking for a web application:

1.  Secure coding standard, preventive, removes the flaw at root

2.  WAF, preventive but compensating

3.  Penetration testing, detective, finds flaws

4.  SIEM correlation rule, detective, alerts after the fact

### Stored procedures

#### What they are

A stored procedure is a SQL statement (or set of statements) that lives on the database server and is called by name from the application. Instead of the app sending SQL text, it sends a procedure name plus parameters.

**Without:** app sends → "SELECT \* FROM users WHERE id = " + userInput

**With:** app sends → CALL getUser(userInput)

#### Why this is a security control

The SQL statement resides on the database server and may only be modified by database administrators.

That's the whole point. Two separate protections:

1\. It limits the application's ability to execute arbitrary code. The app can't invent new SQL. It can only call procedures that already exist. Even if an attacker fully controls the application layer, the set of possible database operations is fixed in advance.

2\. Separation of duties. Only DBAs modify stored procedures. A developer, or an attacker who has compromised the application, cannot change what SQL runs. Control over the query moves from the application tier to the database tier, and to a different set of people.

---

## Application vulnerabilities

### Buffer overflow

Writing more data into a buffer than it was allocated, overwriting adjacent memory. Classic stack overflow overwrites the return address and redirects execution to attacker-controlled code.

**Root cause:** missing bounds checking, typically in C or C++.

**Defences:**

- Input validation and bounds checking, the actual fix
- **ASLR** (Address Space Layout Randomisation): randomise memory locations
- **DEP/NX** (Data Execution Prevention): mark data pages non-executable
- **Stack canaries**: a known value before the return address, checked before return
- Memory-safe languages (Rust, Go, Java, Python)

### Race conditions and TOCTOU

A **race condition** occurs when the result depends on the timing of events that should be independent.

**TOCTOU** (Time of Check to Time of Use) is the security-relevant form: a resource is validated, then used, and the attacker changes it in the gap.

**Defence:** make check and use atomic, or use locking. The cue is "between the check and the use".

### Injection and the rest

| Vulnerability | Mechanism | Primary defence |
| --- | --- | --- |
| **SQL injection** | Untrusted input becomes part of a query | **Parameterised queries / prepared statements** |
| **Command injection** | Input passed to a system shell | Avoid shell calls, allowlist input |
| **XSS** | Attacker script runs in the victim's browser | Output encoding, CSP |
| **CSRF** | Victim's browser is tricked into sending an authenticated request | Anti-CSRF tokens, SameSite cookies |
| **XXE** | XML parser processes external entities | Disable external entity resolution |
| **Insecure deserialization** | Untrusted serialized data is reconstructed | Do not deserialize untrusted input |
| **SSRF** | Server is tricked into requesting an internal resource | Allowlist outbound destinations |

**Input validation is the universal answer for injection. Parameterised queries is the specific answer for SQL injection.** If both appear, parameterised queries is more precise and usually correct.

### Sandboxing and isolation

A sandbox restricts what code can do: no filesystem access outside its area, no arbitrary network calls, constrained system calls.

Used for untrusted code (browser tabs, mobile apps, malware analysis). The cue is running something you do not trust without letting it affect the host.

**Isolation ladder, weakest to strongest:** process isolation, container, virtual machine, physical separation (air gap).

### OWASP

The OWASP Top 10 is the reference list of web application risks. **OWASP Top 10 for LLM Applications** exists separately and covers prompt injection, insecure output handling, training data poisoning, and model theft. Know both by name.
