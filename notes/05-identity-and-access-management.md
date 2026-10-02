# Domain 5: Identity and Access Management

## Contents

- [AAA](#aaa)
  - [RADIUS and TACACS+](#radius-and-tacacs)
- [The four steps](#the-four-steps)
- [The three protocols](#the-three-protocols)
- [Kerberos mechanics](#kerberos-mechanics)
- [The rest of the missing Domain 5 vocabulary](#the-rest-of-the-missing-domain-5-vocabulary)
- [Identity lifecycle](#identity-lifecycle)
- [Authentication factors](#authentication-factors)
- [Biometrics](#biometrics)
- [Security control categories](#security-control-categories)
- [Access control lists](#access-control-lists)
  - [Access control matrix vs ACL vs capability list](#access-control-matrix-vs-acl-vs-capability-list)
- [Social engineering and phishing](#social-engineering-and-phishing)
- [Federation and provisioning standards](#federation-and-provisioning-standards)

---

## AAA

> **AAA = Authentication + Authorization + Accounting**

**NAS** = a device providing network access, which communicates with the AAA server.

**RADIUS TACACS+**

| Transport | UDP | TCP |
| --- | --- | --- |
| Encrypts | Password only | Entire session |
| Combines | Authentication and authorization together | Separates all three |
| Used for | Remote and network user access | Network device administration |
| Origin | Open standard | Cisco |
| Port | 1812/1813 | 49 |

Exam cue: "network administrator accessing devices" means TACACS+. "Remote users connecting" means RADIUS.

**Diameter** is the RADIUS successor, used in telecom and 4G.

**Active Directory uses Kerberos for authentication. Kerberos uses tickets. Port 88**

### RADIUS and TACACS+

#### Why they exist

Centralized remote authentication services. They provide separation of the authentication and authorization processes for remote clients from what's done for LAN/local clients.

**Security reason for the separation:** if the RADIUS or TACACS+ server is compromised, only remote connectivity is affected, not the rest of the network. That's the tested justification.

## The four steps

#### Identification, Authentication, Authorization, Accountability

- **Identification:** claim it is you

- **Authentication:** prove it is you. Providing proof means identity

- **Authorization:** what actions you can perform after authentication. Providing permission means access

- **Accountability:** auditing those logs, tying the action to the person

**Accountability requires attribution.** A smartcard alone gives authorization, not accountability, because you cannot prove who held the card. Attribution needs something tied to the person: biometrics, or a second factor.

## The three protocols

| **Protocol**              | **Does**                                                                                                                            | **Format**                      | **Use it for**                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|------------------------------------------------------------------------|
| **SAML 2.0**              | Authentication and authorisation. Federation between an Identity Provider (IdP) and a Service Provider (SP)                         | XML assertions                  | Enterprise SSO, workforce access to SaaS. The classic corporate answer |
| **OAuth 2.0**             | Authorisation only. Not authentication. Delegated access: lets app A act on your behalf at service B without giving A your password | JSON, access tokens (often JWT) | API access delegation, "allow this app to read your calendar"          |
| **OpenID Connect (OIDC)** | Authentication layer built on top of OAuth 2.0. Adds an ID token carrying identity claims                                           | JSON / JWT                      | Modern web and mobile login, consumer SSO                              |

- The single most tested fact: OAuth is authorisation, not authentication. If a stem says "authenticate the user" and offers OAuth, it is the distractor. OIDC is the one that authenticates.

- SAML roles: Principal (the user), Identity Provider (IdP) asserts identity, Service Provider (SP) consumes the assertion and grants access. SAML assertion types: authentication, attribute, authorisation decision.

- OAuth roles: Resource Owner (the user), Client (the app requesting), Authorisation Server (issues tokens), Resource Server (holds the data).

- Federation = trust relationship letting identities from one domain be used in another. The user has one identity, many services. SSO = authenticate once, access many systems within a session. Federation enables SSO across organisational boundaries.

- SSO trade-off: better user experience and fewer passwords, but a single point of compromise. One stolen session gets everything. Compensate with MFA and short session lifetimes.

- IDaaS (Identity as a Service): cloud-hosted identity, e.g. Okta, Entra ID, Ping. SCIM is the standard for automated provisioning and deprovisioning between systems.

## Kerberos mechanics

| **Component**                     | **Role**                                                                                  |
|-----------------------------------|-------------------------------------------------------------------------------------------|
| **KDC (Key Distribution Center)** | The trusted third party. Holds all keys. Single point of failure. Contains the AS and TGS |
| **AS (Authentication Service)**   | Verifies the user and issues the TGT                                                      |
| **TGS (Ticket Granting Service)** | Exchanges a valid TGT for a service ticket for a specific resource                        |
| **TGT (Ticket Granting Ticket)**  | Proof you authenticated. Time-limited. Presented to the TGS to get service tickets        |
| **ST (Service Ticket)**           | Grants access to one specific service. Presented to the resource server                   |
| **Principal**                     | Any entity with a Kerberos identity: user, service, or host                               |
| **Realm**                         | The Kerberos administrative domain                                                        |

- Flow: user authenticates to the AS → receives a TGT → presents the TGT to the TGS → receives a service ticket → presents the ST to the target service. The password never crosses the network.

- Attacks: Golden ticket (attacker has the krbtgt account hash and forges TGTs for anyone, indefinitely). Silver ticket (forged service ticket for one service). Kerberoasting (request service tickets and crack them offline for the service account password). Pass the ticket. All named under outline 3.7.

## The rest of the missing Domain 5 vocabulary

| **Term**                             | **Meaning**                                                                                                                                                |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Just-In-Time (JIT) provisioning      | Access granted only at the moment of need and automatically expires. Reduces standing privilege. The modern answer to privilege creep                      |
| Credential management system         | Password vault. Stores, rotates and audits credentials centrally. Ties to PAM                                                                              |
| Session management                   | Timeout, re-authentication, concurrent session limits, secure token handling, logout invalidating server-side state                                        |
| Passwordless authentication          | FIDO2 / WebAuthn, passkeys, biometrics tied to a device. Removes the shared secret entirely, so phishing and credential stuffing stop working              |
| Registration and identity proofing   | Verifying a person is who they claim before issuing credentials. Identity assurance levels. This precedes everything else in the lifecycle                 |
| Risk-based / adaptive access control | Access decision considers context: device, location, time, behaviour anomaly. Step-up authentication when risk is elevated. This is Zero Trust in practice |
| PDP / PEP                            | Policy Decision Point makes the allow/deny decision. Policy Enforcement Point enforces it at the resource. Split named explicitly in the outline           |
| Service account management           | Non-human accounts. No interactive login, long-lived credentials, rotation, least privilege. Frequently the weakest identity in an environment             |
| Privilege escalation / sudo auditing | Logging and reviewing every elevation of privilege. Detective control on admin activity                                                                    |

## Identity lifecycle

**Create identity, Provision account, Provision access, Authenticate, Manage access, Deprovision**

- **Create identity** means establishing the entity record: a unique identifier plus relevant attributes

- You cannot authorize something that does not yet exist

- Deprovision on termination or role change

## Authentication factors

**Type Factor Examples**

|  |  |  |
| --- | --- | --- |
| 1 | Something you know | Password, PIN, passphrase, security question |
| 2 | Something you have | Smartcard, token, key fob, phone with an app, certificate |
| 3 | Something you are | Fingerprint, iris, retina, face, voice, hand geometry |
| 4 | Somewhere you are | Geolocation, IP |
| 5 | Something you do | Keystroke dynamics, gait, signature dynamics |

> **MFA requires factors from different types.** Password + PIN is not MFA, both are Type 1. Password + token is MFA.
>
> Quick trick: **MFA is always the best authentication** because it is multi-factor. The best *single* authentication method could be biometric.

## Biometrics

**Accuracy Acceptance**

| Retina | Highest | Lowest. Invasive, requires eye contact, reveals medical data (diabetes, hypertension, pregnancy show in retinal blood vessels) |
| --- | --- | --- |
| Iris | Very high | High. Contactless, works at a distance, through glasses |
| Fingerprint | Good | High, but spoofable |
| Hand geometry | Low | High |
| Voice | Low | High |
| Signature dynamics | Low | High |

Accuracy and acceptance run in opposite directions. Questions mentioning both want the compromise, which is **iris**.

> **Iris is almost always the answer** when a stem asks for the best biometric, especially if it mentions accuracy and privacy or user comfort together.

#### Error rates:

- **FAR (False Acceptance Rate)** = Type 2 error. The dangerous one, the wrong person gets in

- **FRR (False Rejection Rate)** = Type 1 error. The right person is refused

- **CER (Crossover Error Rate)** = where FAR and FRR meet. Lower CER means a better system

## Security control categories

#### Three main types:

- **Logical / technical** (policies, systems)

- **Physical** (fence)

- **Administrative** (training)

#### Seven control functions:

| **Function Example** |                                               |
|----------------------|-----------------------------------------------|
| **Preventive**       | Locks                                         |
| **Detective**        | IDS                                           |
| **Corrective**       | Antivirus, fixes issues that already happened |
| **Compensating**     | DR plan, WAF in front of vulnerable code      |
| **Directive**        | Security policies                             |
| **Recovery**         | Backups                                       |
| **Deterrent**        | Fences                                        |

**A corrective control fixes issues that already happened**

> Ranking on MOST EFFECTIVE questions: **preventive beats detective beats corrective.**

## Access control lists

> **ACLs are not centralized.** They require per-folder or per-object access control, which is why they do not scale.

### Access control matrix vs ACL vs capability list

They are the same data structure read in different directions. That's the whole concept.

An access control matrix is a table of subjects and objects showing what actions each subject can perform on each object.

|           | **Document file** | **Printer** | **Network share** |
|-----------|-------------------|-------------|-------------------|
| **Bob**   | Read              | Print       | No access         |
| **Alice** | Read/Write        | Print       | Read              |
| **Carol** | No access         | No access   | Read/Write        |

- Each column is an ACL

- Each row is a capability list

ACL is tied to the OBJECT. It lists valid actions each subject can perform on that one object. Read the column "Document file" downward: Bob can read, Alice can read/write, Carol has none.

Capability list is tied to the SUBJECT. It lists valid actions that subject can take on each object. Read the row "Bob" across: read the document, print, nothing on the share. Book's image: a key ring of accesses and rights.

#### Which one, and why (the tested part)

**Book is blunt:** using only capability lists is a management nightmare.

To remove access to one object, every subject holding a capability for it must be individually manipulated. Managing access per user account is much harder than managing access per object. So ACLs win administratively.

|                                 | **ACL**                    | **Capability list**                 |
|---------------------------------|----------------------------|-------------------------------------|
| **Attached to**                 | Object                     | Subject                             |
| **Answers**                     | "Who can touch this file?" | "What can this user touch?"         |
| **Revoke access to one object** | Easy, edit one ACL         | Nightmare, edit every subject       |
| **Audit a user's total access** | Hard, check every ACL      | Easy, read the key ring             |
| **Real-world use**              | NTFS, Windows files        | Kerberos tickets, capability tokens |

#### Trigger words

- "tied to the object", "who can access this resource", "permissions on a file/folder" → **ACL**

- "tied to the subject", "key ring", "what this user can access", "row of the matrix" → **capability list**

- "table of subjects and objects" → **matrix**

#### Two more facts

The matrix shown in the book is a DAC system. A MAC or rule-based matrix is built simply by replacing subject names with classifications or roles.

Systems use matrices to quickly determine whether a requested action by a subject on an object is authorized.

**Ties to your existing note:** "ACLs are not centralized, require per-object access control, which is why they do not scale." Careful with that, since the book's complaint is the opposite. ACLs scale better than capability lists. The scaling problem with ACLs is against RBAC, where you assign to groups instead of per-object.

## Social engineering and phishing

- **Phishing:** broad, untargeted

- **Spear phishing:** targets specific groups or individuals

- **Whaling:** targets high-level executives

- **Vishing:** uses voice

- **Smishing:** uses SMS

---

## Federation and provisioning standards

| Standard | What it does | Format | Cue |
| --- | --- | --- | --- |
| **SAML 2.0** | Federated **authentication**, exchanges assertions | XML | Enterprise SSO, identity provider, service provider |
| **OAuth 2.0** | **Authorization** only, delegated access | JSON/REST | "Allow this app to access my account", API access tokens |
| **OpenID Connect** | Authentication layer **on top of** OAuth 2.0 | JSON/REST | "Sign in with Google", consumer SSO, ID token |
| **SPML** | Service Provisioning Markup Language, account **provisioning** | XML | Automated account creation across systems |
| **SCIM** | System for Cross-domain Identity Management, modern provisioning | JSON/REST | Cloud user lifecycle sync |
| **XACML** | eXtensible Access Control Markup Language, **policy** decisions | XML | Attribute-based policy evaluation, PDP/PEP |

**The split that gets tested:** SAML is authentication, OAuth is authorization, OpenID Connect adds authentication on top of OAuth. If the stem says "delegated access without sharing the password", that is OAuth. If it says "single sign-on between enterprises", that is SAML.

**XACML architecture:**

- **PEP** (Policy Enforcement Point): intercepts the request, enforces the decision
- **PDP** (Policy Decision Point): evaluates policy and decides
- **PIP** (Policy Information Point): supplies attributes
- **PAP** (Policy Administration Point): where policies are authored
