# Domain 2: Asset Security

## Contents

- [Data classification](#data-classification)
  - [Military / government (highest to lowest)](#military-government-highest-to-lowest)
  - [Private sector / commercial](#private-sector-commercial)
  - [The discriminator that gets tested](#the-discriminator-that-gets-tested)
- [Data roles and responsibilities](#data-roles-and-responsibilities)
- [Step 1: which list am I in?](#step-1-which-list-am-i-in)
- [Step 2: the four you must know roles](#step-2-the-four-you-must-know-roles)
- [Step 3: the six extra roles](#step-3-the-six-extra-roles)
- [Step 4: recognize the name](#step-4-recognize-the-name)
- [Step 5: the three questions that decide the answer](#step-5-the-three-questions-that-decide-the-answer)
- [Worked examples](#worked-examples)
- [Data lifecycle](#data-lifecycle)
- [Media sanitisation](#media-sanitisation)
- [The ladder, weakest to strongest](#the-ladder-weakest-to-strongest)
  - [Two axes, reconciled](#two-axes-reconciled)
- [Detail per method](#detail-per-method)
- [Decision tree](#decision-tree)
- [Trigger words](#trigger-words)
- [Sanitisation vs declassification](#sanitisation-vs-declassification)
  - [Source conflict to expect](#source-conflict-to-expect)
- [Storage issues](#storage-issues)
- [Privacy by Design: the seven principles](#privacy-by-design-the-seven-principles)
- [Intellectual property](#intellectual-property)
- [Content controls](#content-controls)

---

## Data classification

> The two common schemes are **military/government** and **private sector/commercial**.

"Classified vs unclassified" is NOT one of the two schemes. It is a split *within* the military scheme, a level distinction rather than a scheme distinction.

### Military / government (highest to lowest)

| **Level Meaning**                    |                                      |
|--------------------------------------|--------------------------------------|
| **Top Secret**                       | Grave damage to national security    |
| **Secret**                           | Serious damage                       |
| **Confidential**                     | Damage                               |
| **Sensitive but Unclassified (SBU)** | Minor damage, not for public release |
| **Unclassified**                     | No damage, releasable                |

### Private sector / commercial

| **Level Meaning**              |                                                                       |
|--------------------------------|-----------------------------------------------------------------------|
| **Confidential / Proprietary** | Highest. Severe damage if disclosed. Trade secrets, source code       |
| **Private**                    | **Data about individuals.** HR records, salary, medical, home address |
| **Sensitive**                  | Above public, internal use only                                       |
| **Public**                     | No damage if disclosed                                                |

> Sybex sometimes swaps the order of Private and Sensitive. **Know the top and bottom for certain: Confidential/Proprietary highest, Public lowest.**

### The discriminator that gets tested

**Is the data about a human being, or about the company?**

- **Human** means **Private** (employees, individuals, personnel, HR, customers as people). Maps to PII, GDPR personal data, PHI

- **Company** means Confidential, Proprietary or Sensitive

Memory hook: private life, personal data.

## Data roles and responsibilities

## Step 1: which list am I in?

| **Ask this first**                                                       | **Then you are in**                                               |
|--------------------------------------------------------------------------|-------------------------------------------------------------------|
| **Does the stem mention GDPR, the EU, or personal data of individuals?** | **The GDPR list. Only two roles exist: Controller and Processor** |
| **No GDPR, no EU, no personal data? Just a company and its data.**       | **The general list. Owner, Custodian, Administrator, Auditor**    |

## Step 2: the four you must know roles

| **Role**       | **List**    | **One line**                                                                                                                                                                              | **Trigger words**                                                                        |
|----------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| **Data Owner** | **General** | Decides and is accountable. Classifies the data, decides who gets access, ensures controls exist. Delegates the day-to-day work to the custodian. May be personally liable for negligence | classify, decide access, ultimately responsible, liable, senior manager, department head |
| **Custodian**  | **General** | Protects and stores. Executes what the owner decided: backups, storage, audit logs, integrity validation. Makes no decisions                                                              | stores, safeguards, backups, day-to-day, delegated, safe custody, transport              |
| **Controller** | **GDPR**    | Decides why and how. Determines the purposes and means. Accountable for overall GDPR compliance and must be able to demonstrate it. Accountability cannot be delegated                    | decides purposes and means, accountable for compliance, demonstrates compliance          |
| **Processor**  | **GDPR**    | Executes on instruction. Processes personal data on behalf of the controller, only on documented instructions. Has its own duties but not overall accountability                          | on behalf of, at the direction of, documented instructions, third party doing the work   |

## Step 3: the six extra roles

| **Role**                          | **List**    | **One line**                                                                                                                                                                        |
|-----------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Administrator**                 | **General** | Grants access, following the owner's guidelines. Least privilege, need to know, RBAC groups. Does not decide who should have access, only implements it                             |
| **Auditor**                       | **General** | Reviews and verifies that the policy is implemented and the controls are adequate. Reports to senior management. Independent of the people they audit                               |
| **Asset / System Owner**          | **General** | Owns the system that processes the data, not the data itself. Develops and maintains the system security plan. Often the same person as the data owner, but not always              |
| **Data Subject**                  | **GDPR**    | The individual the personal data is about. Holds the rights: access, rectification, erasure, portability, objection                                                                 |
| **DPO (Data Protection Officer)** | **GDPR**    | Informs, advises, monitors compliance. Advisory only. Does not decide purposes and does not implement. Reports to the highest management level and cannot be told how to do the job |
| **Supervisory Authority**         | **GDPR**    | The national regulator. Monitors and enforces, investigates, fines. This is who receives the 72-hour breach notification. Does not process data for anyone                          |

## Step 4: recognize the name

| **Role**                     | **One line**                                                                                                                                |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **Steward**                  | Quality of the data: accuracy, fitness for use, metadata standards. Not in the Sybex book at all. Appears in ISC2 material and banks        |
| **Business / Mission Owner** | Owns the business process, not the systems. Ensures systems deliver value to the organisation                                               |
| **Security Professional**    | Designs and implements security solutions per the approved policy. Implementer, never decision maker. All decisions go to senior management |
| **User / Operator**          | Accesses data to do their job. Bound by least privilege. Responsible for following policy and procedures                                    |

## Step 5: the three questions that decide the answer

| **Ask**                     | **Decides / owns**                                            | **Executes / implements**                                      |
|-----------------------------|---------------------------------------------------------------|----------------------------------------------------------------|
| **1. Decides or executes?** | **Data Owner, Business Owner, Controller, senior management** | **Custodian, Administrator, Processor, Security Professional** |

| **Ask**                             | **Answer**                                                                                        |
|-------------------------------------|---------------------------------------------------------------------------------------------------|
| **2. Protects, grants, or checks?** | **Protects and stores → Custodian. Grants access → Administrator. Verifies afterwards → Auditor** |
| **3. Safe or correct?**             | **Is the data safe → Custodian. Is the data accurate → Steward**                                  |

## Worked examples

| **Stem**                                                                                                | **Reasoning**                                                                                                                             | **Answer**     |
|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|----------------|
| "You are tasked with ensuring access to newly acquired data stored on the central server is authorised" | No GDPR mentioned → general list, Controller and Processor gone. Tasked with → executing, not deciding. Stored and safeguarding → storage | **Custodian**  |
| "Who is responsible for classifying the data and determining who may access it?"                        | General list. Classify and determine access are decisions                                                                                 | **Data Owner** |
| "A company passes employee data to an external payroll firm. What is the payroll firm?"                 | Personal data, external party acting for someone else → GDPR list. On behalf of                                                           | **Processor**  |
| "Which role is accountable for demonstrating GDPR compliance?"                                          | GDPR list. Accountable and demonstrate are Art. 5(2) and Art. 24                                                                          | **Controller** |
| "Who verifies that the security policy has been properly implemented?"                                  | General list. Verifies after the fact, independent                                                                                        | **Auditor**    |

## Data lifecycle

Data should be properly managed throughout its entire lifecycle. Only the data that is necessary should be collected and retained. Data should be appropriately classified, and transparency maintained regarding how data is collected, stored, used and processed.

## Media sanitisation 

> Data remanence = data remaining on media after it was supposedly erased. Deleting leaves data intact for undelete tools, even after overwriting, traces may survive as less perceptible magnetic fields.

## The ladder, weakest to strongest

| **\#** | **Method**                 | **NIST category**          | **Prepares media for**                                                                                                                             | **Physical access?** |
|--------|----------------------------|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1      | **Erasing**                | None. Not sanitisation     | Nothing. Removes the directory link only, data stays on the drive. Recoverable by anyone with undelete tools. Never acceptable for sensitive media | No                   |
| 2      | **Clearing (overwriting)** | Clear                      | Reuse in the same environment, same security level. Writes unclassified data over all addressable locations                                        | No                   |
| 3      | **Purging**                | Purge                      | Reuse in a less secure environment. Declassification. More intense clearing, repeated, may combine with another method                             | Depends on method    |
| 4      | **Degaussing**             | Purge (a method inside it) | Magnetic media only. Returns tape to original state                                                                                                | Yes                  |
| 5      | **Destruction**            | Destroy                    | Nothing. End of media life. Most secure                                                                                                            | Yes                  |
| n/a    | **Crypto-erase**           | Purge                      | Any environment including cloud. Destroy the keys. Requires data already encrypted                                                                 | No                   |

### Two axes, reconciled

Sybex ranks methods 1 to 5 by strength. NIST defines three categories: Clear \< Purge \< Destroy. Degaussing and crypto-erase are methods that sit inside Purge, not rungs above it. Both framings are correct, they measure different things.

## Detail per method

**Clearing, thorough method (3 passes):** write a character (1010 0001), write its complement (0101 1110), write random bits (1101 0100). Weakness: lab forensics may still recover data. Spare sectors, sectors marked "bad", and wear-levelled SSD blocks are not necessarily cleared.

**Purging:** provides assurance data is not recoverable by any known method. Not always trusted. The US government accepts no purging method for top secret data. Top secret media stays top secret until destroyed.

**Degaussing:** works on magnetic tape, traditional HDDs, floppies. Does not work on SSD, CD, DVD (no magnetic flux). Book does not recommend it for hard disks: it normally destroys the access electronics while giving no assurance the data is gone, and platters can be mounted in another drive in a clean room.

**Destruction:** incineration, crushing, shredding, disintegration, dissolving in caustic or acidic chemicals. Some orgs remove platters from highly classified drives and destroy them separately. Orgs donating or selling equipment often destroy storage rather than purge, to eliminate the risk of an incomplete purge.

**SSD special case:** degaussing does nothing (integrated circuitry, not flux). Research found no traditional single-file method effective, and built-in erase commands failed on some models. Best method is destruction, NSA requires an approved disintegrator shredding to 2mm or smaller. Alternative: encrypt everything stored so remnants are unreadable.

## Decision tree

1\. Do you control the physical media? No (cloud, SaaS, third-party hosted) means cryptographic erase. Stop. Degaussing and shredding are impossible and you cannot verify an overwrite reached the right physical blocks.

**2. If** yes: same level means Clear. Lower level means Purge. Leaving the org, SSD, or top secret means Destroy.

## Trigger words

| **Stem says**                                         | **Answer**                                |
|-------------------------------------------------------|-------------------------------------------|
| Reuse in a less secure environment, declassification  | **Purging**                               |
| Reuse in the same environment                         | **Clearing / overwriting**                |
| Cloud, no physical control of hardware                | **Cryptographic erase**                   |
| Does not erase data, residual data left behind        | **Remanence (the residue, not a method)** |
| Most secure, highest assurance, media leaving the org | **Destruction**                           |
| SSD sanitisation                                      | **Destruction**                           |
| Magnetic tape returned to original state              | **Degaussing**                            |
| Top secret media                                      | **Destruction only**                      |

## Sanitisation vs declassification

- Sanitisation is the umbrella term: clearing, purging, destroying. Can mean destroying media, or purging classified data without destroying the media. For computer disposal: nonvolatile memory removed or destroyed, no CDs/DVDs left in drives, internal HDDs and SSDs sanitised, removed or destroyed.

- Declassification is any process that purges media or a system for reuse in an unclassified environment. The cost of secure declassification often exceeds the cost of new media, and purged data might be recoverable by some future method, so many organisations refuse to declassify and destroy instead.

- Always verify the result. Sanitisation is unreliable when performed by people or flawed tools. Software can be buggy, magnets faulty, either can be misused.

### Source conflict to expect

NIST puts crypto-erase inside Purge. Some question banks list "Purging" and "Encryption" as separate options and key encryption for cloud scenarios. If both appear and the context is cloud, pick the crypto option. Everywhere else purging still wins for reuse at a lower classification.

## Storage issues

- Removable media such as USB drives can easily be used to steal data

- Access controls and encryption must always be applied to the data

- **Data can remain even after deletion** (data remanence)

## Privacy by Design: the seven principles

> *Privacy comes first and is built in. It is on by default and costs you nothing. It is protected end to end, done transparently, and respects the user.*

1.  **Proactive not reactive; preventative not remedial**

2.  **Privacy as the default setting**

3.  **Privacy embedded into design**

4.  **Full functionality: positive-sum, not zero-sum** (privacy AND functionality, not a trade-off)

5.  **End-to-end security: full lifecycle protection**

6.  **Visibility and transparency: keep it open**

7.  **Respect for user privacy: keep it user-centric**

**Trap:** distractors swap a posture word for a promise word. "*Respect* for user privacy" becomes "*assurance* of user privacy". Any option promising a guaranteed outcome is the fake. Privacy by Design describes posture, it never promises outcomes.

## Intellectual property

**Type Protects Duration Exam cue**

| Copyright | Creative works: books, music, film, software source code , art | Life + 70 years, or 95 years corporate | "creative work", "original expression" |
| --- | --- | --- | --- |
| Patent | Inventions, processes, designs | 20 years, then public | "invention", "novel", "must be disclosed publicly" |
| Trademark | Brand names, logos, slogans on goods | Indefinite while in use | "brand", "logo" |
| Service mark | Same, but for services | Indefinite while in use | "services" rather than products |
| Trade secret | Confidential business info (formula, algorithm, client list) | Indefinite while secret | "must remain undisclosed", "no registration", Coca-Cola formula |

> **The five rights of copyright.** All are verbs, and that is the tell:

1.  **Reproduce** the work in any form, language or medium

2.  **Adapt** or derive more works from it

3.  **Make and distribute** copies

4.  **Perform** it in public

5.  **Display or exhibit** it in public

Duration is **not** one of the five rights. If an option describes a term of years, it is the odd one out.

#### Key points:

- Copyright is **automatic on creation.** Registration helps you sue, it does not create the right

- Copyright protects the **expression**, not the idea

- **Software is copyrighted, not patented.** The process the software implements may be patentable

- **Patent vs trade secret:** a patent requires public disclosure in exchange for 20 years of monopoly. A trade secret requires eternal secrecy and offers no protection if independently discovered

- **DMCA** criminalises circumventing copy protection and gives ISPs safe harbour

- **Fair use** is a defense, not a right. Define it generally, do not judge specific cases

> **Legal is not technical.** Copyright gives recourse after infringement. It prevents nothing

## Content controls

| **Control Does what**     |                                                                                                                                                                                      |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DRM**                   | Technical enforcement of usage rules on content **after distribution**. No edit, copy, print, forward, screen capture. Adds expiry, revocation, device and geo locking, watermarking |
| **DLP**                   | Stops sensitive data **leaving** the organisation                                                                                                                                    |
| **Constrained interface** | Restricts what a user can **see or request** in an application. Greyed-out menus, limited views                                                                                      |
| **Watermarking**          | Embeds identifying info to trace leaks. Often a DRM feature                                                                                                                          |

**DLP keeps data in. DRM controls data you deliberately sent out**

| **Stem says Answer type**                                        |                           |
|------------------------------------------------------------------|---------------------------|
| prevent, stop, block, control what they can do                   | **Technical**             |
| protect our rights, pursue infringement, legal remedy, ownership | **Legal**                 |
| data leaving the company                                         | **DLP**                   |
| data we deliberately sent out                                    | **DRM**                   |
| what the user can see in the app                                 | **Constrained interface** |

Other:

- **Whitelist / allow list:** only approved software runs. Stronger, harder to maintain

- **Blacklist / deny list:** block known bad. Weaker, endless list

- **COTS:** commercial off-the-shelf software
