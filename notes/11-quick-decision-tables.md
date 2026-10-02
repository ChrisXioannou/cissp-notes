# Quick Decision Tables

## Contents

- [Cryptography selection](#cryptography-selection)
- [CIA mapping](#cia-mapping)
- [Framework and tool selection](#framework-and-tool-selection)
- [Security model selection](#security-model-selection)

---

## Cryptography selection

| **Stem says Answer**                              |                                    |
|---------------------------------------------------|------------------------------------|
| Confidentiality of bulk data, speed matters       | **Symmetric (AES)**                |
| Non-repudiation, digital signature, key exchange  | **Asymmetric (RSA, ECC, DH)**      |
| Speed **and** secure key exchange                 | **Hybrid** (TLS, PGP, S/MIME, SSH) |
| Verify data was not altered, no encryption needed | **Hashing (SHA-2/3)**              |
| Integrity **and** authenticity together           | **HMAC or digital signature**      |
| WPA2-Enterprise authentication                    | **EAP / 802.1X**                   |
| Network device administration                     | **TACACS+**                        |
| Remote user network access                        | **RADIUS**                         |
| Site-to-site tunnel                               | **IPsec tunnel mode**              |
| Internal host-to-host encryption                  | **IPsec transport mode**           |

## CIA mapping

| **Stem language CIA leg**                                                                                      |                     |
|----------------------------------------------------------------------------------------------------------------|---------------------|
| accurate, correct, unaltered, trustworthy, **quality**, consistent, not corrupted, not tampered with, reliable | **Integrity**       |
| secret, private, disclosure, leakage, classified, need to know, exposure                                       | **Confidentiality** |
| uptime, accessible, downtime, redundancy, DR, outage, HA                                                       | **Availability**    |

**Quality of data means integrity**

Terms that also indicate integrity: non-repudiation (integrity of origin), authenticity, accuracy, entity and referential integrity.

**Integrity is not just about attackers.** A typo, a failed disk write or a bad transaction all break integrity without anyone attacking. That is why hashing, checksums and ACID are integrity controls, and why Clark-Wilson exists.

## Framework and tool selection

| **Cue**                                           | **Answer**      |
|---------------------------------------------------|-----------------|
| Categorize threat types                           | STRIDE          |
| Rate or score threat severity                     | DREAD           |
| Agile, DevOps, scalable                           | VAST            |
| Risk acceptable to stakeholders, open source      | TRIKE           |
| Business objectives plus compliance, risk-centric | PASTA           |
| Anonymous expert consensus                        | Delphi          |
| Shadow IT, SaaS visibility, cloud DLP             | CASB            |
| Misconfiguration, configuration drift, posture    | CSPM            |
| Container, VM, workload runtime                   | CWPP            |
| Enterprise architecture, four domains             | TOGAF (BDAT)    |
| Security architecture framework                   | SABSA           |
| IT governance                                     | COBIT           |
| Product evaluation                                | Common Criteria |
| Payment cards                                     | PCI-DSS         |
| Security management                               | ISO 27001       |
| Financial reporting                               | SOX             |

## Security model selection

| **Stem trigger Model**                                                 |                           |
|------------------------------------------------------------------------|---------------------------|
| Secret, classified, clearance, disclosure, no read up                  | **Bell-LaPadula**         |
| Integrity, data corruption, low-integrity input, no write up           | **Biba**                  |
| Consultant, contractor, competitors, unbiased, conflict of interest    | **Brewer-Nash**           |
| Create subject, delete object, transfer rights, ownership              | **Graham-Denning**        |
| Commercial integrity, transformation procedures, access control triple | **Clark-Wilson**          |
| Data quality (both integrity models)                                   | **Biba and Clark-Wilson** |
