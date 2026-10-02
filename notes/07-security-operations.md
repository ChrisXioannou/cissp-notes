# Domain 7: Security Operations

## Contents

- [Monitoring deployment](#monitoring-deployment)
- [Denial of service attacks](#denial-of-service-attacks)
- [Operations concepts](#operations-concepts)
- [Change management](#change-management)
- [Anti-fraud administrative controls](#anti-fraud-administrative-controls)
- [Incident response](#incident-response)
- [Evidence and forensics](#evidence-and-forensics)
- [eDiscovery (EDRM)](#ediscovery-edrm)
- [Business continuity and disaster recovery](#business-continuity-and-disaster-recovery)
  - [BIA](#bia)
  - [BCP: the four phases](#bcp-the-four-phases)
  - [BCP documentation components](#bcp-documentation-components)
  - [Recovery metrics](#recovery-metrics)
  - [Recovery sites](#recovery-sites)
  - [DRP testing](#drp-testing)
  - [Teams](#teams)
  - [Software escrow arrangements](#software-escrow-arrangements)
- [Availability and reliability metrics](#availability-and-reliability-metrics)
- [Availability terms](#availability-terms)
- [RAID](#raid)
- [Backup types](#backup-types)
- [DDoS mitigation](#ddos-mitigation)
- [DLP](#dlp)
- [Deception technology](#deception-technology)

---

## Monitoring deployment

|  | Sees | Can block? | Packet loss | If it fails | Latency |
| --- | --- | --- | --- | --- | --- |
| SPAN / mirror | Copy of traffic | No | Yes, drops under switch load | No network impact | None |
| TAP | Copy of traffic | No | No | No network impact | None |
| Inline | The actual traffic | Yes | No | Traffic stops or flows uninspected | Yes |

**IDS is out of band (SPAN or TAP), detective. IPS is inline, preventive**

**How SPAN works.** A switch only sends frames to the port where the destination MAC lives, so an IDS plugged into a random port sees almost nothing. You configure the switch to copy traffic from a set of ports to a monitoring port. The original traffic is untouched.

Consequences: the IDS cannot block (it holds a copy), the IDS failing has zero network effect, and the switch **drops mirrored frames first** under load, so monitoring gets lossy exactly when you need it most.

**TAP** is a physical device spliced into the cable, passively splitting the signal. No switch CPU involved, no dropped frames. Used for high-assurance monitoring.

**Inline** means the device sits in the path and nothing reaches the destination until it forwards. The only way to drop a malicious packet. Cost: single point of failure, added latency, and a fail-open vs fail-closed design decision.

**Encryption limit:** TLS traffic looks like noise. A SPAN-fed IDS sees headers, packet sizes, timing and destinations, not payload. This is why decryption points and TLS-inspecting proxies exist.

**Device Deployment Control type Purpose**

| IDS | Out of band | Detective | Alert on suspicious traffic |
| --- | --- | --- | --- |
| IPS | Inline | Preventive | Block malicious traffic |
| Firewall | Inline | Preventive | Allow or deny by rules |
| WAF | Inline (reverse proxy) | Preventive | HTTP/HTTPS, SQLi, XSS |
| NGFW | Inline | Preventive | DPI plus threat intelligence |
| Proxy | Inline | Preventive | Mediates client requests |

## Denial of service attacks

| **Attack**        | **Protocol**           | **Detail**                                                                                                                                                                                                                                                                                                                                              |
|-------------------|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Smurf**         | **ICMP**               | Spoofed broadcast ping using the victim's IP as the source. Every system on the segment replies to the victim. Uses an amplifying network (smurf amplifier) via a directed broadcast through a router. RFC 2644 (1999) changed router defaults so directed broadcasts are not forwarded, which limits smurf to a single network. Rarely a problem today |
| **Fraggle**       | **UDP ports 7 and 19** | Identical to smurf but UDP instead of ICMP. Broadcasts a UDP packet with the victim's spoofed IP. The ICMP vs UDP split is the whole question                                                                                                                                                                                                           |
| **Ping flood**    | ICMP                   | Floods the victim with ping requests. Effective as a botnet DDoS. Blocked by disabling ICMP                                                                                                                                                                                                                                                             |
| **Ping of death** | ICMP                   | Oversized ping packet, over 64 KB where normal is 32 or 64 bytes. Caused crashes or buffer overflows. Patched                                                                                                                                                                                                                                           |
| **Teardrop**      | IP fragmentation       | Mangles fragments so the receiver cannot reassemble them. Older systems crashed. IDS can check for malformed packets                                                                                                                                                                                                                                    |
| **SYN flood**     | TCP                    | Floods with TCP SYN packets, leaving half-open connections that exhaust the connection table. Defence: SYN cookies                                                                                                                                                                                                                                      |
| **LAND**          | TCP                    | Packet crafted with the source and destination both set to the victim's own address                                                                                                                                                                                                                                                                     |

Smurf vs Fraggle is the pair that gets tested. Everything about them is identical except the protocol. Smurf = ICMP. Fraggle = UDP 7 and 19.

If a question asks which OSI layer an attack sits at, identify the protocol it abuses and map that. Fraggle abuses UDP, so Transport. Smurf abuses ICMP, so Network. Reasoning from the protocol works; memorising a bank's layer table does not.

## Operations concepts

- **SLA (Service Level Agreement):** performance expectations

- **CSP:** cloud service provider

- **Secure provisioning:** resources deployed in a secure manner, for example starting VMs from a secure image

## Change management

**Scope:** every modification to the production environment. Systems, applications, configurations, security controls, infrastructure, processes. Not just code.

> **Purpose: preventing security compromises.** Uncontrolled change is a leading cause of incidents: a rushed firewall rule, a server rebuilt without hardening, a patch that reopens a closed port.

- **Request control**: framework for users to request changes, managers do cost/benefit, devs prioritise

- **Change control**: devs recreate the issue and test the fix before production

- **Release control**: approval for release, includes **acceptance testing**

#### Mechanisms:

> **Request, Impact assessment, Approval, Build and test, Notify stakeholders, Implement, Validate, Document**

- **RFC:** Request for Change

- **CAB:** Change Advisory Board, approves

- **Standard change:** pre-approved, low risk, no CAB needed

- **Emergency change:** expedited, approved retroactively

- **Rollback plan:** required **before** approval

- Approval always follows an impact assessment. The assessment is what management approves on

> **The test that matters:** documentation, communication and rollback are **mechanisms serving the purpose**. If a stem asks for the **goal**, it is preventing security compromises. If it asks what you do at a given stage, it is a mechanism.

Trap: "maintaining documentation" feels like the answer because documenting is the visible part of the job. It is step 8 of 8, not the point.

## Anti-fraud administrative controls

| **Control Purpose**                      |                                                               |
|------------------------------------------|---------------------------------------------------------------|
| **Separation of duties**                 | No one person completes a sensitive process alone             |
| **Dual control**                         | Two people simultaneously required for one action             |
| **Two-person integrity**                 | Two people physically present                                 |
| **Job rotation**                         | Forces exposure of ongoing fraud, someone else takes the role |
| **Mandatory vacation**                   | Detective. Schemes surface while the person is away           |
| **Least privilege**                      | Minimum access needed for the role                            |
| **Need to know**                         | Access only to the data required for the task                 |
| **Access recertification / attestation** | **Periodic** review by data owners. Fixes **privilege creep** |

> **Insider theft or fraud means administrative controls, not cameras and locks.** Cameras and locks stop outsiders. Rotation and separation catch insiders.
>
> **Collusion** is what defeats dual control.
>
> **Privilege attestation review** is the ideal way to remove unjustified elevated permissions based on job responsibilities.

**Exam cue:** "periodically", "regularly", "on a schedule", "ongoing", "validated" means the answer is a recurring review process, not a one-time setting.

## Incident response

#### NIST 800-61 four phases:

> **Preparation, Detection and Analysis, Containment / Eradication / Recovery, Post-Incident Activity**

Post-Incident Activity is also called **Lessons Learned**. Both names are correct.

**Sybex / ISC2 seven-step version** (describes handling an incident, so it starts at Detection): Detection, Response, Mitigation, Reporting, Recovery, Remediation, Lessons Learned

#### Which to use:

- Stem is about the **plan**, the programme, being prepared, means **Preparation**

- Stem is about an incident **already underway** means **Detection** first

- FIRST question with Preparation as an option means almost always Preparation

**Lessons Learned covers:** root cause analysis, updating the IR plan and playbooks, retraining, evidence retention decisions, feeding findings back into controls. Exam cue is anything about *preventing recurrence*. It loops back into Preparation, which is why IR is a cycle.

#### Event vs incident vs disaster:

- **Event:** any observable occurrence

- **Incident:** an event that harms, or threatens to harm, CIA

- **Disaster:** an incident severe enough that normal operations cannot be restored at the primary site Do not invoke DR for what might be scheduled maintenance. Confirm the event first.

## Evidence and forensics

#### Types of evidence:

- **Testimonial:** verbal

- **Real:** actual objects

- **Documentary:** written documents

**Evidence types**

- **Admissible** requires three things: **relevant, material, competent** (legally obtained). All three or it's excluded

- **Direct**: witness observation, proves a fact on its own

- **Circumstantial**: based on inference. **Supports** a conclusion, cannot prove it

- **Corroborative**: strengthens other evidence, **cannot stand alone**

- **Hearsay**: statement made outside court by someone not testifying. **Generally inadmissible.** Courts apply this so **system logs cannot be introduced unless authenticated by a system administrator**

- **Demonstrative**: objects, pictures, models used to illustrate facts at trial

> **Best evidence rule:** when possible, the original and not a copy must be used as evidence.

**Parol evidence rule:** when a written contract is formed, it is assumed all terms are in the agreement, and no verbal agreement can modify the written one.

**Probative value:** whether a piece of evidence is useful in proving something at trial.

> **Locard's exchange principle:** criminals leave something behind when they take something. Forensics concept, unrelated to contract law.
>
> **Data retention for evidence is critical.** Chain of custody must be documented.

## eDiscovery (EDRM)

**Information Governance, Identification, Preservation, Collection, Processing, Review, Analysis, Production, Presentation**

Front half is **getting** the data: govern, identify, preserve, collect. Back half is **using** it: process, review, analyse, produce, present.

- **Preservation:** legal hold, stop deletion

- **Review:** check relevance, filter out privileged and confidential material

- **Analysis:** deep inspection of content and context

- **Production:** hand over in the required format

## Business continuity and disaster recovery

> **BCP (Business Continuity Plan):** how to keep services running for clients. **COOP (Continuity of Operations Plan) DRP (Disaster Recovery Plan):** how to recover the systems to do that.

Continuity is about keeping services going. Disaster recovery is about recovering the systems.

> **BCP lifecycle:**
>
> **Project scope and planning, BIA, Recovery Strategy, Plan approval and implementation, Training and awareness, Documentation testing and maintenance**

### BIA

#### BIA vs risk assessment:

- **Risk assessment:** what could happen, how likely, how bad

- **BIA:** if this function stops, what does it cost us and how long can we survive without it

BIA is about **impact of disruption**, not threats or likelihood. A BIA assumes disruption has occurred and does not care *why*. Fire, flood, ransomware, supplier failure are all the same to a BIA.

> **Rule: if BIA appears in a stem, look for disaster, downtime or continuity.** No disruption in the scenario means it is not BIA.

#### BIA covers both tangible and intangible impact:

- **Tangible:** lost revenue per hour, replacement cost, contractual penalties, regulatory fines, overtime, temporary facilities

- **Intangible:** reputational damage, loss of customer confidence, brand harm, employee morale, competitive position, market share

#### BIA steps (Sybex, ordered):

1.  **Identify priorities:** critical business functions, ranked

2.  **Risk identification:** what could disrupt them

3.  **Likelihood assessment:** ARO for each threat

4.  **Impact assessment:** tangible and intangible. Produces MTD, RTO, RPO

5.  **Resource prioritisation:** rank where to spend, based on ALE

Step 5 is the output: a prioritised list telling you where the money goes.

**BIA outputs:** critical business functions ranked, MTD/RTO/RPO/WRT for each, financial impact of downtime, operational and reputational impact, dependency mapping, recovery priority order.

### BCP: the four phases

#### 1. Project Scope and Planning

- **Business organization analysis:** identify all departments/individuals with stake (operational depts, critical support like IT + facilities, corporate/physical security, senior execs). Done first by BCP leads, then re-validated by full team.

- **BCP team selection:** reps from core operational depts, business unit members from the org analysis, IT SMEs, cybersecurity staff, physical security/facilities, legal counsel, HR, PR, senior management.

- **Resource requirements:** estimate resources for 3 phases: BCP development, BCP testing/training/maintenance, BCP implementation. Labor is the biggest cost.

- **Legal and regulatory requirements:** due diligence obligations of officers/directors, industry regulation, contractual/SLA obligations. Keep attorneys involved for the whole plan lifetime, not just pre-implementation review.

**2. Business Impact Assessment (BIA) (5 steps, both quantitative and qualitative)**

- **Identify priorities:** criticality prioritization, ranked list of business processes. Assign asset value (AV). Set MTD (max tolerable downtime) and RTO. Goal: RTO \< MTD.

- **Risk identification:** natural vs man-made threats. Purely qualitative at this stage, no likelihood or damage yet.

- **Likelihood assessment:** ARO per risk, from corporate history, team experience, experts, published hazard maps.

- **Impact assessment:** EF (% of asset damaged), SLE = AV x EF, ALE = SLE x ARO. Qualitative side: goodwill loss, staff attrition, negative publicity, social responsibility.

- **Resource prioritization:** sort risks descending by ALE, then merge with qualitative list into single prioritized list with senior management.

#### 3. Continuity Planning (5 subtasks)

- **Strategy development:** decide which risks get mitigated vs accepted, referencing MTD figures.

- **Provisions and processes:** design actual controls. Three asset categories:

- **People:** safety first, always. Provisions for shelter, food, rotated stockpiles.

- **Buildings/facilities:** hardening provisions, or alternate sites.

- **Infrastructure:** physical hardening (fire suppression, UPS), or alternative/redundant systems.

- The goal of this process is to create a **continuity of operations plan (COOP)**, which focuses on how an org will carry out critical business functions starting shortly after a disruption occurs and extending up to one month of sustained operations

- Plan approval

- Plan implementation

- Training and education

#### 4. Approval and Implementation

- **Plan approval:** endorsement by top executive (CEO/chair/president) for weight and credibility.

- **Plan implementation:** BCP team builds implementation schedule, deploys resources, then runs ongoing maintenance program.

- **Training and education:** everyone gets an overview briefing; people with direct BCP duties trained and evaluated on their specific tasks; at least one trained backup per task.

### BCP documentation components

- Continuity planning goals

- Statement of importance (letter, ideally CEO-signed)

- Statement of priorities (from BIA identify-priorities step)

- Statement of organizational responsibility

- Statement of urgency and timing (with timetable)

- Risk assessment (recap of BIA; include AV, EF, ARO, SLE, ALE plus qualitative reasoning; point-in-time, update regularly)

- Risk acceptance/mitigation (why accepted risks were accepted + triggers to revisit; controls for unaccepted ones. Document acceptance formally, auditors look for it)

- Vital records program (which records are vital, where stored, backup procedures)

- Emergency-response guidelines (immediate response procedures, notification list, secondary procedures until BCP team assembles)

- Maintenance (living document, team stays intact, version control, destroy old copies, fold BCP duties into job descriptions)

- Testing and exercises (test types covered in Ch. 18)

**Two things worth memorizing for the exam:** the 4 phases in order, and the 5 BIA steps in order. Also the formulas: SLE = AV x EF, ALE = SLE x ARO.

### Recovery metrics

**Metric Meaning Calculated in Documented in**

| MTD | Maximum time down before the business fails | BIA | Recovery Strategy |
| --- | --- | --- | --- |
| RTO | Target time to restore | BIA | Recovery Strategy |
| RPO | Maximum acceptable data loss , measured backwards in time | BIA | Recovery Strategy |
| WRT | Work Recovery Time: verifying and catching up after restore | BIA | Recovery Strategy |

**RTO + WRT must be less than or equal to MTD. RTO must always be less than or equal to MTD.**

**MTD = MAD = MAO = MTO.** Maximum Tolerable Downtime, Maximum Allowable Downtime, Maximum Acceptable Outage, Maximum Tolerable Outage. Same concept, four names. MAD is the hard business limit; RTO is the target you design toward.

**Recovery Strategy contains:** the chosen approach (hot, warm or cold site, cloud, reciprocal agreement), the documented metrics, recovery team roles, and the timeline responders work to.

**The mechanic:** BIA is the **analysis** that produces the numbers. Recovery Strategy is the **plan document** people actually pick up during a disaster. If a stem asks where a responder finds the timeline, it is Recovery Strategy. If it asks where MTD is determined, it is BIA.

### Recovery sites

**Site State Cost and speed**

| Cold | Empty. Ready to move in, nothing running yet | Cheapest, slowest recovery |
| --- | --- | --- |
| Warm | Pre-installed hardware, pre-configured bandwidth | Middle. Faster, more expensive |
| Hot | Fully replicated | Most expensive, fastest |

The hotter the site, the more ready it is. Cold means nothing is running yet.

### DRP testing

Five tests, in increasing order of disruption:

1.  **Read-through** (checklist)

2.  **Structured walkthrough** (tabletop)

3.  **Simulation**

4.  **Parallel test**

5.  **Full interruption test**

### Teams

**Recovery team** gets critical business functions running, typically at the alternate site. **Salvage team** restores operations at the primary site.

### Software escrow arrangements

**What it protects against:** a software developer failing to provide adequate support for its products, or going out of business leaving no technical support available.

#### How it works

- Developer provides copies of the application source code to an independent third-party organization

- Third party maintains updated backup copies of the source code securely

- The agreement between end user and developer specifies trigger events

- When a trigger event occurs, the third party releases the source code to the end user

- End user can then analyze the source to resolve issues or implement updates

#### Trigger events the book names

- Failure of the developer to meet the terms of an SLA

- Liquidation of the developer's firm

**Where it belongs:** part of your disaster recovery plan. That placement gets tested. It's a DR/BC control, not a procurement or IP control.

**When to use it:** if your organization depends on custom-developed software or software from a small firm. The book's practical note: negotiate escrow with suppliers you fear may go out of business because of their size. You won't get an escrow agreement out of Microsoft, and you don't need one, since a firm that size isn't going to vanish.

**Three parties, always:** end user, developer, independent third party. If an option describes a two-party arrangement, it isn't escrow.

Don't confuse with key escrow, which is copies of private cryptographic keys held for recovery. Different thing entirely, same word.

## Availability and reliability metrics

- **MTTF (Mean Time To Failure):** expected lifespan of a **non-repairable** component. It dies once and you replace it

- **MTBF (Mean Time Between Failures):** average uptime between failures of a **repairable** system

- **MTTR (Mean Time To Repair):** average time to fix

## Availability terms

| **Term Means**        |                                                                              |
|-----------------------|------------------------------------------------------------------------------|
| **Redundancy**        | Duplicate components exist. The **mechanism**                                |
| **Fault tolerance**   | System keeps operating through a component failure. The **property**         |
| **High availability** | Meets an uptime target such as 99.9%. A **measurement**                      |
| **Resilience**        | Absorb disruption and recover. Broader, includes more than component failure |

RAID gives redundancy; the *result* is fault tolerance.

You can have redundancy without fault tolerance: a cold spare in a rack is redundant, but if failover requires a human it was not fault tolerant.

**The mechanic:** when a stem describes a desired outcome, pick the property. When it asks how you achieve it, pick the mechanism.

## RAID

> **RAID is for uptime, not backup.** The OS sees just one drive. Deleted files, ransomware and corruption replicate instantly to every disk in the array.

**Level Does Survives**

|  |  |  |
| --- | --- | --- |
| 0 | Striping, speed only | Nothing. One disk dies and everything is gone |
| 1 | Mirroring, exact copy | 1 disk |
| 5 | Striping plus distributed parity, minimum 3 disks | 1 disk |
| 6 | Striping plus double parity, minimum 4 disks | 2 disks |
| 10 | Mirrored pairs, then striped | 1 per pair, fastest recovery |

> **A higher number does not mean more secure or more expensive.** RAID 10 outperforms RAID 6 despite the lower number. RAID 0 has a RAID number and zero redundancy, which is the trap.

#### The clean split:

- **RAID** means availability. A disk fails, the service continues

- **Backup** means recovery. Data is lost or corrupted, you restore it

You need both. They solve different failures.

**Exam cues:** "disk failure without downtime" means RAID. "Recover deleted or corrupted data" means backup. "Site destroyed" means a DR site, neither.

## Backup types

**Type Backs up Speed Restore complexity**

| Full | Everything | Slowest | Simplest, one restore |
| --- | --- | --- | --- |
| Incremental | Only changes since the last backup, full or incremental | Fastest daily | Slowest restore. Need the full plus every incremental in order |
| Differential | Everything changed since the last full backup | Medium | Faster restore than incremental. Need the full plus the latest differential only |

**Method What it moves Currency Cost**

| Electronic vaulting | Batch transfer of backup data to an offsite location. Periodic, not continuous | Least current | Cheapest |
| --- | --- | --- | --- |
| Remote journaling | Transaction logs transferred offsite in near real time. Moves the logs, not the data itself | More current | Middle |
| Remote mirroring | Live identical copy maintained at another site | Most current | Most expensive |

**Order by currency and cost: vaulting \< journaling \< mirroring**

**Discriminator:** journaling moves logs, vaulting moves data. If a stem asks about preserving the data itself offsite, journaling is wrong.

## DDoS mitigation

> **CDNs are the best answer for DDoS mitigation**, because they also do rate limiting and blocking, and absorb traffic across a distributed edge.

## DLP

**DLP is the control that stops sensitive data leaving**

Note that DLP cannot work without classification. You cannot stop "sensitive" data leaving until you have defined which data is sensitive. This is why a classification programme beats DLP or IPS on "BEST way to protect against exfiltration" questions when DLP is not among the options.

**IDS and IPS are not the answer for exfiltration.** They can inspect outbound traffic, but they miss the largest categories: an insider with a USB drive, an employee emailing to personal webmail, uploads to personal cloud storage over TLS. All authorised, encrypted, and indistinguishable from normal work.

---

## Deception technology

| Term | What it is |
| --- | --- |
| **Honeypot** | A single decoy system with no production purpose |
| **Honeynet** | A network of honeypots |
| **Honeytoken** | A fake record or credential planted to detect misuse |
| **Padded cell** | Where an attacker is transferred *after* detection, rather than attracted in |
| **Darknet** | Unused address space, any traffic to it is suspicious by definition |

**Entrapment vs enticement.** Entrapment is persuading someone to commit a crime they would not otherwise have committed. It is illegal and a defence in court. Enticement is making an already-willing attacker choose your decoy over a real target. Enticement is legal.

Because a honeypot has no production purpose, **any** interaction with it is suspicious. That is why they generate so few false positives.
