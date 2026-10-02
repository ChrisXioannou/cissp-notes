# Domain 1: Security and Risk Management

## Contents

- [The five pillars](#the-five-pillars)
- [Risk terminology](#risk-terminology)
- [Three types of risk](#three-types-of-risk)
- [Risk responses](#risk-responses)
- [Quantitative vs qualitative](#quantitative-vs-qualitative)
- [The formula chain](#the-formula-chain)
  - [EF vs ARO: severity vs frequency](#ef-vs-aro-severity-vs-frequency)
- [Risk assessment process (NIST SP 800-30)](#risk-assessment-process-nist-sp-800-30)
- [Threat modeling](#threat-modeling)
  - [STRIDE](#stride)
  - [The others](#the-others)
- [Framework categories](#framework-categories)
- [TOGAF: the four architecture domains (BDAT)](#togaf-the-four-architecture-domains-bdat)
- [Legal, regulatory and governance](#legal-regulatory-and-governance)
- [Due care vs due diligence](#due-care-vs-due-diligence)
- [SOC reports](#soc-reports)
- [Supply chain and hardware trust](#supply-chain-and-hardware-trust)
- [Personnel security](#personnel-security)
  - [Exit interviews](#exit-interviews)
- [US laws and regulations](#us-laws-and-regulations)
  - [HIPAA roles](#hipaa-roles)
  - [Liability under SOX](#liability-under-sox)
- [International privacy and data transfer](#international-privacy-and-data-transfer)
  - [GDPR essentials](#gdpr-essentials)
  - [EU-US transfer mechanisms, in order](#eu-us-transfer-mechanisms-in-order)
  - [Other regimes worth recognising](#other-regimes-worth-recognising)
- [Investigation types](#investigation-types)

---

## The five pillars

Classic triad plus two recent additions.

- **Confidentiality**

- **Integrity**

- **Availability**

- **Non-repudiation:** a user cannot deny performing an action

- **Authenticity:** a user is genuinely who they claim to be

## Risk terminology

> Memory chain: a **threat agent** exploits a **vulnerability**, creating a **threat event**. If it succeeds, it is a **breach**. **Risk** is what you calculated beforehand about how likely that was.

**When is it a breach?** Only when security was actually penetrated AND data was compromised. Not attempted, not exploited-but-contained.

**The word "accidental" is a signal.** A breach implies compromise. A threat event can be an employee emailing a spreadsheet to the wrong address. "Accidental or intentional" wants the broader term.

> **"Vulnerability" is broader on this exam than in daily SOC work.** Any weakness, gap or missing safeguard, not just a published CVE. No antivirus, no fence, untrained staff, weak process are all vulnerabilities.

Vulnerability is the flaw. Exposure is the condition of being susceptible to it. When a safeguard is missing, what remains is the **hole**.

## Three types of risk

- **Inherent risk:** the risk that exists in the absence of controls. Before implementing anything.

- **Total risk:** overall risk exposure **before** controls are applied.

- **Residual risk:** what remains **after** controls are applied. Must be accepted.

> **Total Risk = threats x vulnerabilities x asset value**

**Control gap:** the amount of risk that is reduced by implementing the security controls.

> **Residual risk = total risk - control gap**

## Risk responses

**Avoid, transfer, mitigate, accept**

- **Avoidance:** we do not want the risk at all

- **Mitigation:** reduce it

- **Transference:** have someone else deal with it (insurance, outsourcing)

- **Acceptance:** live with it

> Deterrence is a **control type**, not a risk response.

Risk management is **cyclical**, never one-and-done. After treatment, always reassess.

## Quantitative vs qualitative

> **Quantitative does not mean money. It means numbers.** Money is the most common output, not the definition. Any hard data (historical incident counts, probabilities, frequencies, measured rates) is quantitative.

- **Quantitative** = objective, measurable, calculable. Money, percentages, frequencies, probabilities. ALE/SLE/ARO are quantitative *tools*, not the whole category.

- **Qualitative** = subjective ratings. High/medium/low, expert judgment, opinions.

**Cue: does the stem hand you data? Yes means quantitative**

- "How much will this cost us annually" means quantitative

- "Rank these risks" means qualitative

**Qualitative methods:** Delphi (anonymous rounds until consensus), brainstorming, interviews, surveys, focus groups, scenario analysis, risk matrices.

> **FMEA (Failure Modes and Effects Analysis):** systematic analysis of how each component could fail and the consequences.

## The formula chain

> Computed in this order: **AV, EF, SLE, ARO, ALE**

**Term Meaning Formula**

| AV | Asset value | given |
| --- | --- | --- |
| EF | Exposure factor, percentage of the asset lost per event | given |
| SLE | Single loss expectancy, cost of ONE event | AV x EF |
| ARO | Annualised rate of occurrence, frequency per year | given |
| ALE | Annualised loss expectancy, cost PER YEAR | SLE x ARO = AV x EF x ARO |

ALE appears in both equivalent forms on the exam. Both are correct.

> **Safeguard evaluation** (cost effective and hard to bypass):
>
> **Safeguard value = ALE before safeguard - ALE after safeguard - annual cost of safeguard**

ARO examples: once every 10 years = 0.1. Twice a year = 2. Once a year = 1.

### EF vs ARO: severity vs frequency

> **EF is NOT "how exposed we are."** It is: *if this event occurs, what percentage of the asset's value is lost.* Severity per occurrence, not likelihood.

Example: server worth 100,000. A fire destroys 70% of it. EF = 0.7, SLE = 70,000. That 70% holds whether fires happen yearly or once a century.

| **Term Measures Unit** |                     |            |
|------------------------|---------------------|------------|
| **EF**                 | How BAD, per event  | percentage |
| **ARO**                | How OFTEN, per year | count      |
| **SLE**                | Cost of ONE event   | AV x EF    |
| **ALE**                | Cost PER YEAR       | SLE x ARO  |

EF sounds like a likelihood word. It is a **damage** word.

## Risk assessment process (NIST SP 800-30)

#### Prepare, Conduct, Communicate, Maintain

Conduct breaks into:

1.  Identify **threat sources and events** (threat actors)

2.  Identify **vulnerabilities**

3.  Determine **likelihood** (this is ARO)

4.  Determine **impact**

5.  Determine **risk**

So: vulnerability assessment finished means the next step is **likelihood / ARO**. Reporting to management is **Communicate**, the end of the process, never the next step mid-assessment.

A vulnerability scan identifies threat sources. The next step is identifying potential threat vectors.

**Risk Management Framework (NIST 800-37)**

Seven steps:

1.  **Prepare** to execute a risk management framework

2.  **Categorize** the information systems

3.  **Select** security controls

4.  **Implement** the controls

5.  **Assess** them based on the outcome we need

6.  **Authorize** the system (this is accreditation)

7.  **Monitor** the security controls

Monitor loops back. The cycle never ends.

## Threat modeling

Systematic process to identify security threats in a system before building or deploying it.

| **Cue in the stem Answer**                                                              |            |
|-----------------------------------------------------------------------------------------|------------|
| Categorize or enumerate threat **types**                                                | **STRIDE** |
| Rate or score the severity of a threat                                                  | **DREAD**  |
| Agile, DevOps, scalable across enterprise                                               | **VAST**   |
| Risk acceptable to **stakeholders**, open source                                        | **TRIKE**  |
| Align **business objectives** with technical requirements, **compliance**, risk-centric | **PASTA**  |

### STRIDE

A taxonomy. Walk through each component and ask which of the six applies.

**Letter Threat Violates**

| S | Spoofing | Authentication |
| --- | --- | --- |
| T | Tampering (unauthorised modification of data) | Integrity |
| R | Repudiation (deny doing something, blame someone else) | Non-repudiation |
| I | Information Disclosure (leak of sensitive info) | Confidentiality |
| D | Denial of Service | Availability |
| E | Elevation of Privilege | Authorisation |

**Note that STRIDE and DREAD are used together: STRIDE to identify the threats, DREAD to prioritize them**

### The others

- **DREAD:** a method for rating and severity-scoring threats. Damage potential, Reproducibility, Exploitability, Affected users, Discoverability.

- **VAST:** Visual, Agile, Simple, Threat. Based on Agile PM principles, integrates continuously into agile development.

- **TRIKE:** threat modeling plus risk management. Open source. Identify assets and threats, analyse the risks, assign risk levels, decide whether those risks are acceptable to **stakeholders**.

- **PASTA (Process for Attack Simulation and Threat Analysis):** seven stages, risk-centric. Only stage 1 matters for the exam:

**Definition of Objectives (business objectives plus compliance)**

TRIKE and PASTA extend into risk territory at the back end, but both are still threat modeling methods, not risk management frameworks.

Find the many points where an attack could occur, then use threat modeling techniques to assess them. Evaluate how controls reduce risk.

## Framework categories

**Zachman Framework**: enterprise architecture. Matrix of **six perspectives** crossed with **six questions** (what, how, where, who, when, why). Maps technology to business change.

| **Category**                   | **Examples**                                                  |
|--------------------------------|---------------------------------------------------------------|
| **Threat modeling**            | STRIDE, DREAD, PASTA, VAST, TRIKE                             |
| **Risk management framework**  | NIST RMF (800-37), ISO 27005, FAIR                            |
| **Security control framework** | ISO 27001 (what/why), ISO 27002 (how), NIST CSF, CIS Controls |
| **IT governance**              | COBIT                                                         |
| **Enterprise architecture**    | TOGAF, SABSA, Zachman                                         |
| **Process maturity**           | CMMI, IDEAL, SW-CMM                                           |
| **Product evaluation**         | Common Criteria (ISO 15408), replaced TCSEC and ITSEC         |

#### Standards mapping:

- Product evaluation means **Common Criteria**

- Payment cards means **PCI-DSS**

- Security management means **ISO 27001**

- Financial reporting means **SOX**

**SABSA:** security architecture framework and methodology.

**FedRAMP:** a US government program providing a standardized approach to cloud security assessment and authorization.

> **COBIT:** IT governance framework.

## TOGAF: the four architecture domains (BDAT)

| **Domain**      | **Owns**                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------|
| **B**usiness    | Strategy, governance, key business processes                                                    |
| **D**ata        | Structure of logical and physical information assets, and management of their resources         |
| **A**pplication | Application systems and their interactions                                                      |
| **T**echnology  | Hardware and software required to support deployment of business, data and application services |

## Legal, regulatory and governance

**Laws and regulations outrank standards and frameworks**

Different laws apply based on geography, especially in the cloud. Laws can collide and legal counsel is needed. Always get the expert in the room.

- **GDPR:** applies to every company that has customers in the EU. Breaches must be reported to the Supervisory Authority within 72 hours **of becoming aware of the breach**, not of the breach occurring. Detection delay adds to the total elapsed time.

- **PIA (Privacy Impact Assessment):** may be required to demonstrate GDPR compliance.

- **BCP (Business Continuity Plan):** the overall organizational plan for how to continue the business.

## Due care vs due diligence

- **Due diligence** = investigating, researching, understanding risk. **Before**. DO AND DETECT.

- **Due care** = acting reasonably on what you found. Ongoing, the prudent person standard. DO CORRECT.

Both require thinking before you act.

Failure to check dependencies before a change is a failure of **due diligence**, not of change control. A perfect change process still lets a bad change through if nobody did the analysis.

**These two answer a lot of "why did this go wrong" questions**

As a manager you must conduct a cost-benefit analysis. The last step always involves money.

## SOC reports

An independent examination of controls at a service organization. Helps customers assess whether a third-party provider has appropriate controls.

The point of this is that a service organization gets audited **once** and shares the report with all customers, instead of undergoing separate assessments from every customer.

Think: service provider, auditor, SOC report, customer assurance.

| **Report Focus** |                                                                                                                          |
|------------------|--------------------------------------------------------------------------------------------------------------------------|
| **SOC 1**        | Controls relevant to **financial reporting**                                                                             |
| **SOC 2**        | Controls against the **Trust Services Criteria**: security, availability, processing integrity, confidentiality, privacy |
| **SOC 3**        | Same criteria as SOC 2, but a **public summary**. Effectively a marketing tool                                           |

> **SOC 2 Trust Services Criteria:** Security, Availability, Processing Integrity, Confidentiality, Privacy.

- **Type I:** controls assessed at a point in time. A snapshot.

- **Type II:** controls assessed for operating effectiveness over a period of time. A video.

## Supply chain and hardware trust

- **Hardware root of trust:** a line of defense against executing unauthorized firmware

- **TPM:** a hardware root of trust. Works with certificates

- **Silicon root of trust:** an unchangeable signature burned into the chip

- **SBOM (Software Bill of Materials):** a list of all software components a particular piece of software depends on. Now required in many contexts

## Personnel security

- **Training must be specific to each role.** Generic training is the wrong answer.

- **Policies come before training.** You train people on the policy.

- **KPIs (key performance indicators)** represent all the active controls. Backward-looking. **How did we do? Measures what already happened.**

- KRI = forward-looking. **How exposed are we? Signals risk building up. Percentage of systems on unsupported software, vendors without a current security assessment.**

### Exit interviews

**Primary security purpose: review the NDA and continuing obligations**

Confidentiality survives employment. Reminding the departing employee in person, on the record, removes "I didn't know" as a defence later.

Checklist:

- Review NDA and continuing obligations

- Collect company property (badge, laptop, tokens, keys)

- Escort out

- **Access termination happens in parallel**, coordinated with the interview timing

Deprovisioning is a technical task performed by IT, usually without the employee present. It is not an interview activity, which is why "cancel network access" is the wrong answer despite being necessary.

> **Classic exam scenario:** disable accounts **before or during** the termination meeting, never after, so the employee cannot retaliate. HR/IT coordination is the tested point.

---

## US laws and regulations

The exam is vendor-neutral but US-law heavy. You are not asked to practise law. You are asked to recognise which statute governs a scenario from one or two cue words.

| Law | Year | Covers | Cue words in the stem |
| --- | --- | --- | --- |
| **HIPAA** | 1996 | Protected health information (PHI) | Hospital, clinic, patient records, insurer, medical |
| **HITECH** | 2009 | Extends HIPAA, adds breach notification, makes business associates directly liable | Business associate, breach notification, health |
| **GLBA** | 1999 | Financial institutions, customer financial privacy, safeguards rule | Bank, lender, insurer, financial product, NPI |
| **SOX** | 2002 | Financial reporting integrity for public companies, executive accountability | Public company, financial statements, CEO/CFO sign-off, internal controls |
| **FERPA** | 1974 | Student education records | School, university, student records, parent access |
| **COPPA** | 1998 | Online collection of data from children under 13 | Children, under 13, parental consent, website |
| **CFAA** | 1986 | Unauthorized access to protected computers | Hacking, exceeding authorized access, prosecution |
| **ECPA** | 1986 | Interception of electronic communications, stored communications | Wiretap, email interception, monitoring |
| **FISMA** | 2002 | Security for US federal agencies and their contractors | Federal agency, government system, authorisation to operate |
| **PCI DSS** | - | Payment card data. **Contractual, not law** | Cardholder data, merchant, acquirer, card brands |

**PCI DSS is the trap.** It is a contractual standard enforced by the card brands, not legislation. A question asking which of four items is "not a law" is usually pointing at PCI DSS.

### HIPAA roles

- **Covered entity:** providers, health plans, clearinghouses
- **Business associate:** a vendor handling PHI for a covered entity. Needs a **Business Associate Agreement (BAA)**
- Under HITECH, business associates are directly liable, not only through contract

### Liability under SOX

SOX made executives personally accountable. If a stem has a CEO or CFO certifying financial statements, or asks who is ultimately responsible, SOX reasoning applies: **senior management**, never the security team.

## International privacy and data transfer

### GDPR essentials

- Applies to personal data of EU data subjects, **regardless of where the processor sits**. Extraterritorial by design
- **Controller** decides purpose and means. **Processor** acts on the controller's instructions
- **Data Protection Officer (DPO)** required for public authorities, large-scale monitoring, or large-scale special-category processing
- Breach notification: **72 hours** to the supervisory authority
- Fines: up to **4% of global annual turnover or 20 million euro**, whichever is higher
- Lawful bases: consent, contract, legal obligation, vital interests, public task, legitimate interests

**Data subject rights:** access, rectification, erasure (right to be forgotten), restriction, portability, objection, rights relating to automated decision-making.

### EU-US transfer mechanisms, in order

| Mechanism | Status | What happened |
| --- | --- | --- |
| **Safe Harbor** | Invalidated 2015 | Struck down by *Schrems I* |
| **Privacy Shield** | Invalidated 2020 | Struck down by *Schrems II* |
| **EU-US Data Privacy Framework (DPF)** | Adopted 2023 | Current mechanism |
| **Standard Contractual Clauses (SCCs)** | Valid | Contractual, survives the invalidations, needs a transfer impact assessment |
| **Binding Corporate Rules (BCRs)** | Valid | For transfers inside a single corporate group |

If a stem asks how to transfer personal data from the EU to a country without an adequacy decision, **SCCs** is the safe answer.

### Other regimes worth recognising

- **PIPEDA** (Canada), **LGPD** (Brazil), **APPI** (Japan), **POPIA** (South Africa), **CCPA/CPRA** (California)
- **Wassenaar Arrangement:** multilateral export control on dual-use goods, including some cryptography. The cue is export restrictions on security technology

## Investigation types

Which standard of proof applies is a recurring question.

| Type | Standard of proof | Who runs it | Outcome |
| --- | --- | --- | --- |
| **Administrative** | Lowest, often just cause | Internal | Internal discipline, process change |
| **Civil** | Preponderance of the evidence | Private parties | Damages |
| **Criminal** | Beyond a reasonable doubt | Government | Fine, imprisonment |
| **Regulatory** | Varies by regulator | Regulator | Sanction, fine, licence action |

**Beyond a reasonable doubt is the highest bar and applies only to criminal.** Preponderance means more likely than not, roughly 51%.
