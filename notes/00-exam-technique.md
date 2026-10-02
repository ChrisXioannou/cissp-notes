# Exam Technique

## Contents

- [The tier ladder (BEST / MOST questions)](#the-tier-ladder-best-most-questions)
- [The FIRST ladder (sequence questions)](#the-first-ladder-sequence-questions)
- [Pick the right ladder](#pick-the-right-ladder)
- [Why policy beats technical controls](#why-policy-beats-technical-controls)
- [Instant eliminators](#instant-eliminators)
- [Answering mechanics](#answering-mechanics)
- [Three recurring errors](#three-recurring-errors)
- [ISC2 Code of Ethics](#isc2-code-of-ethics)

---

## The tier ladder (BEST / MOST questions)

**Tier Category Examples**

|  |  |  |
| --- | --- | --- |
| 1 | Human life and safety | Evacuate, get out, don't re-enter |
| 2 | Legal and regulatory | GDPR, jurisdiction, data residency, compliance, liability |
| 3 | Policy and governance | Policy, standards, classification, contracts, procedures |
| 4 | Technical controls | Encryption, firewall, IPS, access controls, scanning |

> **preventive \> detective \> corrective**

Human safety beats any other option

## The FIRST ladder (sequence questions)

1.  **Stop active harm** (only if damage is accumulating right now)

2.  **Assess, confirm, investigate**

3.  **Act on the findings**

4.  **Report and document**

## Pick the right ladder

| Qualifier | Ladder to run |
| --- | --- |
| FIRST, NEXT | Sequence |
| BEST, MOST EFFECTIVE | Tier |
| GREATEST RISK, PRIMARY CONCERN | Impact |
| MOST LIKELY | Probability, not severity |
| NOT, EXCEPT | Membership: find the leftover |

## Why policy beats technical controls

CISSP is written from the CISO's chair, not the engineer's.

- Policy defines what the control must enforce. A firewall rule with no policy behind it is one person's opinion.

- Policy scales. It covers every system, every employee, every future case.

- Policy establishes accountability and shifts liability.

- Classification precedes protection. You cannot protect data you have not classified.

Policies provide **guidance** for security efforts. High level, not technical.

## Instant eliminators

**Absolute language** is almost always wrong: eliminate, expunge, eradicate, guarantee, ensure completely, always, never, all, every, 100%, fully secure, zero risk, indefectible.

Security reduces risk to an acceptable level. It never removes it. Residual risk always remains

Other eliminators:

- **Two options saying the same thing** means neither is the answer.

- **Broad, generic, too-good-to-be-true** options are wrong.

- **Overly technical** options usually lose on scenario questions.

- **"Comply with standards"** is never the answer to an immediate-action question.

- When tasked with **stopping** something, policy-related answers lose. Look for an enforced control.

## Answering mechanics

- **Read the actor first.** PM, auditor, CISO, developer. The role sets the vocabulary.

- **Underline the qualifier** before reading the options.

- **Tag each option**: CIA leg, control type, tier.

- **Count the requirements in the stem**, then count coverage per option. Most coverage wins.

- **Read the governance option first**, not last, or you will anchor on a technical answer.

- **"And then what?"** for PRIMARY-reason questions. The endpoint of the chain is the answer.

- **Paired options:** tag the individual items first, then find the pair where both qualify.

- **Every option correct in isolation** means the question is testing sequence, not knowledge.

- **When two options belong to the same phase**, neither is the answer to a FIRST question.

- **Property vs mechanism:** if the stem describes a desired outcome, pick the property. If it asks how you achieve it, pick the mechanism.

- Most important skill: understanding **which part of CIA** you are protecting.

| **Written policies**   | **Intent.** What should happen                                                                      |
|------------------------|-----------------------------------------------------------------------------------------------------|
| **Config screenshots** | **State.** How it's set up right now, one moment in time                                            |
| **Auth logs**          | **Activity.** That something happened                                                               |
| **Access reviews**     | **Operating effectiveness.** That the procedure runs on a schedule and someone validates the output |

## Three recurring errors

1.  **Technical over governance.** The biggest single pattern.

2.  **Act before assess** on FIRST questions.

3.  **Missed qualifier.** Reading past FIRST, current, documented, interim, periodically.

> On exam day, ask on every scenario question: **what would a CISO who has never touched a terminal choose here?**

## ISC2 Code of Ethics

Preamble: safety of society, the common good, duty to principals and to each other requires adherence to the highest ethical standards. Strictly necessary to be a member.

**The four canons, in order. The order IS the priority**

1.  **Protect society**, the common good, necessary public trust and confidence, and the infrastructure

2.  **Act honorably, honestly, justly, responsibly and legally**

3.  **Provide diligent and competent service to principals**

4.  **Advance and protect the profession**

> **Society, Honour, Service, Profession**

Also know **RFC 1087 (Ethics and the Internet)** and the Internet Architecture Board ethics statement. Unethical acts: unauthorised access, wasting resources, destroying the integrity of information, compromising privacy.

Violating the code can cost the certification.
