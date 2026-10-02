# Domain 4: Communication and Network Security

## Contents

- [Modern network technologies](#modern-network-technologies)
- [The OSI model](#the-osi-model)
- [Topologies](#topologies)
- [Signaling and media access](#signaling-and-media-access)
- [Storage and circuits](#storage-and-circuits)
- [Firewalls](#firewalls)
- [NAT](#nat)
  - [Network zones](#network-zones)
- [Address resolution](#address-resolution)
- [Wireless](#wireless)
- [VPN and tunnelling](#vpn-and-tunnelling)
- [Insecure to secure protocol pairs](#insecure-to-secure-protocol-pairs)
- [Email security](#email-security)
- [Defence in depth: layered security](#defence-in-depth-layered-security)

---

## Modern network technologies

- **VXLAN:** a larger type of VLAN. Allows more segmentation

- **SDN (Software Defined Networking):** allows the network to be programmed using software

- **SD-WAN:** enables users in branch offices to connect remotely to enterprise networks. Security is mostly IPsec, VPN tunnels and firewalls

- **Li-Fi:** uses LED light to transmit data. **Requires line of sight**, which rules it out for most industrial and ranged scenarios

- **Zigbee:** a short-range wireless personal area network for monitoring IoT devices. **Range up to 100 m, very low power.** RF-based

- **5G:** faster, but carries some security issues as well

## The OSI model

| Layer | Name | Protocols and devices |
| --- | --- | --- |
| 7 | Application | HTTP, FTP, SMTP, DNS, SNMP, LDAP, Telnet, SSH |
| 6 | Presentation | TLS/SSL (often placed here), encoding, compression |
| 5 | Session | RPC, NetBIOS, PPTP |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, IPsec , ICMP, routers |
| 2 | Data Link | Ethernet, ARP, L2TP , PPP, switches, MAC addresses |
| 1 | Physical | Cables, hubs, repeaters, bits |

> Mnemonic (7 to 1): **All People Seem To Need Data Processing**

**Trap:** L2TP is named "layer two tunneling protocol" but relies on **IPsec at layer 3** for encryption. L2TP itself does not natively encrypt.

## Topologies

- **Mesh:** every device connected to every other device. High redundancy and reliability

- **Bus:** all devices share a single central cable (backbone)

- **Ring:** devices connected in a closed loop, data travels in one direction (or both in a dual ring)

- **Star:** all devices connect to a central hub or switch that controls communication

## Signaling and media access

- **Analog** is a continuous signal that varies in frequency. Not good over long distances

- **Digital** is on or off. Much better over long distances

**CSMA** is a technology that helps reduce collisions, working at layer 2 (Data Link).

- **CSMA/CA (Collision Avoidance):** announces intent to transmit *before* sending, to avoid a collision happening

- **CSMA/CD (Collision Detection):** responds *after* a collision by making stations wait a random amount of time before retransmitting

**Remember: CA acts before, CD acts when a collision happens**

**Token passing** is a separate media access method, not the same as CSMA/CA. One token circulates and only the holder may transmit.

## Storage and circuits

**iSCSI:** a protocol that transports block-level storage over standard IP networks, allowing a remote storage device to appear as a locally connected disk.

#### Private circuits vs packet switching:

- **Private circuit:** a dedicated physical path between two points, always there and always yours. Used when you need guaranteed bandwidth and low latency, such as connecting two office locations or critical infrastructure

- **Packet switching:** your data shares network infrastructure with everyone else's, broken into packets that find their way to the destination. This is what the internet uses. Much cheaper and more flexible for variable traffic, but without the same guarantees

## Firewalls

#### By generation and layer:

- **Static packet filter (layer 3):** filters traffic by examining message headers

- **Circuit-level (layer 5):** manages communication between trusted partners

- **Application-level (layer 7):** filters based on the application protocol

#### Stateless vs stateful:

|  | Layer | Reads | Remembers |
| --- | --- | --- | --- |
| Stateless | 3-4 | Headers only | Nothing |
| Stateful | 3-4 | Headers only | Connection state |
| NGFW / DPI | 3-7 | Headers and payload | Connection state, application, identity |

- A **stateless firewall** looks at each packet independently against predefined rules. It does not remember previous packets or understand whether a packet belongs to an established connection. Good for simple rules, speed, high-volume traffic

- A **stateful firewall** keeps track of connections via a **state table**. It knows packets belong to an established session, and can block anything that does not make sense for that session. Good for connection context and more intelligent rules

**Stateless: faster and simpler, less context. Stateful: more context, better connection awareness**

Practical difference: with a stateless firewall you need an explicit rule for return traffic, opening ports inbound. A stateful firewall allows return traffic automatically because it knows the connection was established from inside.

> **Note: stateful firewalls do not read the full packet either.** They work at layers 3 and 4. Reading actual content is **deep packet inspection**, which is NGFW territory.
>
> **Baselines are not a firewall function at all.** Building a model of normal behaviour and flagging deviations is anomaly-based IDS, UEBA or NDR.

**WAF (Web Application Firewall):** filters HTTP and HTTPS traffic between the internet and a web application. Good against SQL injection and XSS. Deployed inline as a reverse proxy.

> **NGFW (Next Generation Firewall): always stateful.** Deep packet inspection, plus:

- Application awareness (identifies the app regardless of port)

- Integrated IPS

- External threat intelligence feeds

- User identity awareness

- TLS inspection

## NAT

**NAT** hides private subnets behind public addresses.

The most common type is **PAT (Port Address Translation).** Many private IPs share one public IP, differentiated by port number. This is what a home router does. Most common and most efficient: 65,536 ports means many simultaneous sessions from one IP.

### Network zones

Three segment types, and the definitions are precise.

#### Intranet

A private network designed to host the same information services found on the internet. Web, email, and other services on internal servers not accessible to anyone outside the private network.

**Sharp edge the book draws:** networks that rely on external servers (positioned on the public internet) to provide information services internally are not intranets. If the server lives outside, it isn't an intranet.

#### Extranet

A cross between the internet and an intranet. A section of an organization's network sectioned off so it acts as an intranet for the private network but also serves information to the public internet.

- Often reserved for use by specific partners or customers

- Rarely on a public network

#### DMZ (demilitarized zone / perimeter network)

An extranet for public consumption. A network area, usually a subnet, designed to be accessed by outside visitors but still isolated from the private network.

**What lives there:** public web, email, file, and other resource servers. Systems that must be accessible from both internal and external networks.

**What does NOT live there:** databases. Book's explicit example: the web server goes in the DMZ; the database server isn't meant for public access, so it belongs on the internal network or at least a secured subnet separated from the DMZ. That's a favourite question.

#### How the DMZ is built

**Screened host:** a firewall-protected system logically positioned just inside a private network. All inbound traffic routes to it, and it acts as a proxy for trusted systems. Filters incoming traffic and protects the identity of the internal client. Single-firewall design.

**Screened subnet:** same concept, but a subnet is placed between two routers or firewalls, with the bastion host(s) inside that subnet. All inbound traffic goes to the bastion host, and only authorized traffic passes through the second router/firewall into the private network. This is the DMZ.

**Bastion host:** runs the bare minimum, no unnecessary services. Named after medieval castle architecture, where a bastion guardhouse sat in front of the main entrance as a first layer of protection. The term signals a sacrificial host that will receive all inbound attacks.

#### Firewall deployment architectures

| **Tier**                   | **Design**                                                                                              | **Protection**                    |
|----------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------|
| **Single-tier**            | Private network behind one firewall, then a router to the internet                                      | Generic attacks only. Minimal     |
| **Two-tier**               | Either one firewall with 3+ interfaces (DMZ off one leg), or two firewalls in series (DMZ between them) | Allows a DMZ. Moderate complexity |
| **Three-tier / multitier** | Multiple subnets in sequence, each progressively deeper                                                 | Highest                           |

#### Why segment at all (three reasons, all tested)

- **Boosting performance:** systems that communicate often go in the same segment, those that rarely communicate go elsewhere. Routers divide broadcast domains

- **Reducing communication problems:** contains congestion and broadcast storms to individual subsections

- **Providing security:** isolates traffic and user access to segments where they are authorized

Segments are created with switch-based VLANs, routers, or firewalls, individually or combined.

## Address resolution

- **ARP:** I have an IP, who has this MAC? (IP to MAC)

- **RARP:** I have a MAC, who has my IP? (MAC to IP)

**APIPA:** 169.254.0.0/16, self-assigned when DHCP fails. Link-local only, does not route. Seeing a 169.254 address on a host means DHCP failed.

## Wireless

**Standard Encryption Authentication Status**

| WEP | RC4, static key, weak IV | Shared key | Broken. Never the answer |
| --- | --- | --- | --- |
| WPA | TKIP (still RC4) | PSK or 802.1X | Interim fix, deprecated |
| WPA2 | CCMP/AES (802.11i) | PSK (Personal) or 802.1X/EAP (Enterprise) | Standard answer |
| WPA3 | AES-GCMP, SAE handshake | SAE replaces PSK, forward secrecy | Current best |

1.  **X** is port-based network access control. Three roles:

    - **Supplicant:** the client device

    - **Authenticator:** the switch or access point

    - **Authentication server:** RADIUS

**EAP** is the authentication *framework* that 802.1X carries. It is not encryption itself.

| **EAP variant Notes** |                                                           |
|-----------------------|-----------------------------------------------------------|
| **EAP-TLS**           | Certificates on **both** sides. Strongest, needs PKI      |
| **PEAP**              | Server cert only, builds a TLS tunnel, credentials inside |
| **EAP-TTLS**          | Similar to PEAP, more flexible inner methods              |
| **LEAP**              | Cisco proprietary, **broken**, never pick it              |
| **EAP-FAST**          | Cisco's LEAP replacement                                  |

**Exam cue:** WPA2-Enterprise plus authentication means EAP, 802.1X or RADIUS. WPA2-Personal means pre-shared key.

## VPN and tunnelling

**Protocol Layer Notes**

| IPsec | 3 (Network) | The actual security. AH gives integrity and authentication only. ESP gives confidentiality, integrity and authentication |
| --- | --- | --- |
| L2TP | 2 | No native encryption , pairs with IPsec |
| PPTP | 2 | Obsolete, broken |
| SSL/TLS VPN | 5-7 | Remote access, browser-based |

#### IPsec modes:

- **Transport mode:** payload encrypted, original IP header intact. Host-to-host, internal segments

- **Tunnel mode:** whole packet encrypted and re-encapsulated. Site-to-site, gateway-to-gateway

#### IPsec vs VPN, the distinction that gets tested:

- **IPsec** is the protocol suite. This is where the security comes from

- **VPN** is a use case: an encrypted tunnel over an untrusted network. It has **no security of its own**, it inherits whatever protocol you run: IPsec, SSL/TLS, WireGuard

A VPN is not "more secure than IPsec". A VPN *is* IPsec wearing a hat. If the stem asks what **provides** integrity and confidentiality, the answer is IPsec. If it asks about the deployment, the answer is VPN.

- Site-to-site (office to office, router-based) means **IPsec**

- Remote access (user to office) means **SSL/TLS VPN** or an IPsec client

- Internal, "across multiple network segments" means **IPsec transport mode**

**TLS** is the newer version of SSL, mainly used for encrypting HTTP traffic. It supports better encryption algorithms.

## Insecure to secure protocol pairs

The most testable table. If a stem says "not encrypted" or "sends credentials in cleartext", the answer is the right-hand column.

| Insecure | Port | Secure replacement | Port |
| --- | --- | --- | --- |
| FTP | 20/21 | SFTP (over SSH) or FTPS | 22 or 989/990 |
| Telnet | 23 | SSH | 22 |
| HTTP | 80 | HTTPS (TLS) | 443 |
| SMTP | 25 | SMTPS or STARTTLS | 465 or 587 |
| POP3 | 110 | POP3S | 995 |
| IMAP | 143 | IMAPS | 993 |
| LDAP | 389 | LDAPS | 636 |
| SNMPv1/v2c | 161/162 | SNMPv3 | 161/162 |
| DNS | 53 | DNSSEC (integrity), DoT 853 or DoH 443 (confidentiality) |  |
| TFTP | 69 | Avoid entirely, no authentication |  |

> **Never use FTP to transmit data securely.** It does not provide encryption. SFTP and SNMPv3 do.

**SFTP is for file transfers, not for ongoing data streams**

> **DNSSEC caveat:** provides integrity and authenticity only, **not** confidentiality. Common trap.

**SFTP vs FTPS:** SFTP is file transfer over SSH. FTPS is FTP with TLS bolted on. Both encrypt; SFTP is the usual answer.

## Email security

| **Protocol Purpose** |                                                                            |
|----------------------|----------------------------------------------------------------------------|
| **S/MIME**           | Encryption plus digital signatures for email. Uses PKI and certificates    |
| **PGP / OpenPGP**    | Same purpose, web of trust instead of a hierarchical CA                    |
| **SPF**              | Lists which servers may send for a domain                                  |
| **DKIM**             | Signs outbound mail, verifies integrity and origin                         |
| **DMARC**            | Policy layer on top of SPF and DKIM, tells receivers what to do on failure |

## Defence in depth: layered security

**The key word is SERIES**

**Term Means Correct for layering?**

| Series | Sequential. Each control must be passed in turn | Yes |
| --- | --- | --- |
| Parallel | Simultaneous, side by side. Defeat any one and you are through | No, wrong configuration |
| Multiple | True but not distinctive. Parallel security also has multiple controls | No |
| Filter | A single control type, not an arrangement | No |
