# Domain 6: Security Assessment and Testing

## Contents

- [Testing techniques](#testing-techniques)
- [Audits](#audits)
- [Vulnerability scan vs penetration test](#vulnerability-scan-vs-penetration-test)
- [Penetration test phases](#penetration-test-phases)
- [Penetration test knowledge levels](#penetration-test-knowledge-levels)
- [Network reconnaissance techniques](#network-reconnaissance-techniques)
- [Product evaluation](#product-evaluation)
- [Vulnerability scoring and threat frameworks](#vulnerability-scoring-and-threat-frameworks)
  - [CVSS](#cvss)
  - [The identifier alphabet soup](#the-identifier-alphabet-soup)
  - [Attack frameworks](#attack-frameworks)

---

## Testing techniques

**Fuzzing:** testing with unexpected circumstances by modifying inputs.

#### Code scanning:

- **Static (SAST):** analyses the source code without running it. The right choice when developers analyse code before it goes live

- **Dynamic (DAST):** executes the application and tests it at runtime, without needing the source code

> **Software testing** can be manual or automatic.

## Audits

> **A security audit is generally third party**, unless the question states otherwise.

An **audit** is a methodical examination of an environment. **Audit trails** are recorded evidence of what happened.

## Vulnerability scan vs penetration test

| **Stem asks Answer**                                           |                        |
|----------------------------------------------------------------|------------------------|
| Confirm fixes were applied, verify remediation                 | **Vulnerability scan** |
| Find vulnerabilities in the first place                        | **Vulnerability scan** |
| Prove a vulnerability is exploitable, test defences end to end | **Penetration test**   |
| Next step after a VA in a progressive assessment programme     | **Penetration test**   |

**The mechanic: what is the stem trying to learn?**

- "Is it still there?" means **scan**

- "Can it be used against me?" means **pentest**

**Why scan wins on remediation verification:** an audit produces a list of findings. A scan checks every item on that list systematically in an hour. A pentest is scoped and time-boxed and goes deep on attack paths, so it will not re-verify every finding.

Pentest answers are frequently distractors on verification questions because they sound heavier and more thorough. Risk management is cyclical. After treatment, always reassess: from a vulnerability scan to a vulnerability assessment.

## Penetration test phases

1.  **Reconnaissance:** gather info, passive or active

2.  **Scanning / enumeration:** port scan, service and version identification, banner grabbing. Cue: "identify services"

3.  **Vulnerability assessment:** match services to known weaknesses. Cue: "locate deficiencies"

4.  **Exploitation:** actually attack. Cue: "leverage findings"

#### Post-exploitation / pivoting

5.  **Reporting**

## Penetration test knowledge levels

Book pairs each colour with a knowledge level. Learn them as pairs, since questions use either term.

| **Box**       | **Team**          | **Knowledge**                                                    | **Resembles**            |
|---------------|-------------------|------------------------------------------------------------------|--------------------------|
| **Black box** | Zero-knowledge    | Nothing except public info (domain name, company address)        | A real external attack   |
| **White box** | Full-knowledge    | Everything: patches, upgrades, exact device configs, source code | An insider or auditor    |
| **Gray box**  | Partial-knowledge | Some info, e.g. network design and configuration details         | A focused, targeted test |

| **Term**          | **Meaning**                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------|
| Red team          | Offensive. Simulates a real adversary, usually covert and goal-based rather than comprehensive                         |
| Blue team         | Defensive. Detects and responds                                                                                        |
| Purple team       | Red and blue working together, sharing findings in real time to improve detection. Not a third team, a mode of working |
| White team / cell | Referees the exercise, sets rules of engagement, arbitrates                                                            |

## Network reconnaissance techniques

- **Port scans:** identify open ports and services

- **IP probes:** ping addresses across a range to find live hosts

- **Vulnerability scans:** match findings to known weaknesses

## Product evaluation

If a product is not working as expected, the failure was in some form of **evaluation**, not in requirements gathering.

---

## Vulnerability scoring and threat frameworks

### CVSS

The Common Vulnerability Scoring System rates severity on a 0 to 10 scale.

| Group | Contains | Changes over time? |
| --- | --- | --- |
| **Base** | Attack vector, complexity, privileges required, user interaction, scope, CIA impact | No |
| **Temporal** | Exploit maturity, remediation level, report confidence | Yes |
| **Environmental** | How much it matters in *your* environment | Per organisation |

| Score | Rating |
| --- | --- |
| 0.1-3.9 | Low |
| 4.0-6.9 | Medium |
| 7.0-8.9 | High |
| 9.0-10.0 | Critical |

**CVSS measures severity, not risk.** Risk needs asset value and likelihood in your context, which is what the environmental group is for. A stem asking how to prioritise remediation wants CVSS *plus* business context, not CVSS alone.

### The identifier alphabet soup

- **CVE:** a unique identifier for one specific vulnerability
- **CWE:** a category of weakness (for example CWE-89, SQL injection)
- **CPE:** a naming scheme for platforms and products
- **CCE:** configuration issues
- **SCAP:** the protocol that ties these together for automated scanning

CVE names an instance. CWE names the class. That distinction is tested.

### Attack frameworks

| Framework | Shape | Best for |
| --- | --- | --- |
| **Lockheed Martin Cyber Kill Chain** | Linear, 7 stages | Describing an intrusion end to end |
| **MITRE ATT&CK** | Matrix of tactics and techniques | Mapping observed adversary behaviour, detection coverage |
| **Diamond Model** | Four vertices | Analysing a single intrusion event |

**Cyber Kill Chain, seven stages:** Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control, Actions on Objectives.

**Diamond Model vertices:** Adversary, Capability, Infrastructure, Victim. Any two known vertices help you pivot to the others.

**The discriminator:** the Kill Chain is sequential and good for explaining a whole attack. ATT&CK is a catalogue and good for detection engineering and gap analysis. The Diamond Model analyses one event, not a campaign.
