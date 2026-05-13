# Attack Surface

Enumeration of all identified entry points, exposed assets, and attack vectors
within the scope of this threat model.

---

## Asset Inventory

| Asset | Type | Exposure | Criticality |
|---|---|---|---|
| [Asset Name] | Server / Endpoint / App / Network # Attack Surface

Enumeration of all identified entry points, exposed assets, and attack vectors for **Tempus Fugit** (online gaming platform).

---

## Asset Inventory

| Asset | Type | Exposure | Criticality |
|---|---|---|---|
| Proprietary Game Source Code (IP) | Data / Intellectual Property | Internal - file servers, source control | Critical |
| Payment Information (User Billing) | Data | Internal - payment database, billing API | Critical |
| In-Game Private Chat System | Application / People | Internet-facing - chat server, API | High |
| File Server | Hardware / Infrastructure | Internal network (IT_Network subnet) | Critical |
| Payment Database | Data / Hardware | Internal - backend payment processing | Critical |
| Chat Server | Hardware / Infrastructure | Internet-facing | High |

---

## Attack Surfaces & MITRE Mapping

| Key Asset | Attack Surface | Component Type | MITRE Technique | Relationship |
|---|---|---|---|---|
| Intellectual Property (IP) | Unauthorized file access via SMB enumeration | Hardware | T1135 - Network Share Discovery | Attackers enumerate file shares to locate and access proprietary game source code |
| Intellectual Property (IP) | Exfiltration of source code | Hardware | T1041 - Exfiltration Over C2 Channel | Once accessed, IP is covertly exfiltrated over command-and-control channels |
| Payment Information | SQL Injection against payment API - DB extraction | Data | T1190 - Exploit Public-Facing Application | SQLi directly exploits the public-facing payment input vector |
| Payment Information | Credential Dumping - payment system access | Data | T1003 - Credential Dumping | Stolen credentials allow direct authenticated access to payment data |
| In-Game Chat System | Chat server compromise - message interception | People | T1056.004 - Credential API Hooking | Attackers hook into chat processes to intercept private messages |
| In-Game Chat System | Abuse of chat API to deliver malicious payloads | People | T1204 - User Execution | Attackers weaponize chat messages to deliver malicious links or files to players |

---

## Entry Points

### External Entry Points

- [x] Public-facing web application / game client (chat API)
- [x] Payment API and checkout endpoints (SQLi, Magecart-style skimming)
- [x] Email - spearphishing against employees (TA0001 Initial Access)
- [x] DNS / Domain infrastructure
- [x] In-game chat system (phishing, malicious link delivery, social engineering)

### Internal Entry Points

- [x] Lateral movement to file server via compromised employee credentials
- [x] Insider threat - abuse of legitimate file server or database access
- [x] SMB enumeration once inside the IT_Network subnet (T1135)
- [x] Payment database access via credential dumping (T1003)
- [x] Chat server backend access by privileged insiders (moderators / admins)

---

## Risk Ratings

| Attack Surface | Risk | Likelihood | Impact | Rating |
|---|---|---|---|---|
| File Server | Unauthorized access, data theft, ransomware, misconfigured permissions, data loss | 4 - Likely | 5 - Catastrophic | **9 / 10** |
| Payment Database | Unauthorized access, SQL injection, data breach, data tampering, insider misuse | 4 - Likely | 5 - Catastrophic | **9 / 10** |
| Chat Server | Unauthorized access, phishing, spam, impersonation, malware delivery, DoS | 4 - Likely | 4 - Major | **8 / 10** |

---

## Attack Scenarios

### Scenario 1 - Phishing Entry Point to Compromise Internal Systems (TA0001)
A threat actor launches a spearphishing campaign against a Tempus Fugit employee using Gophish, delivering a convincing email that redirects to a fake login page. The victim submits corporate credentials, which the attacker uses to access Microsoft 365 and internal communications, then authenticate into the IT_Network subnet. PowerShell discovery enumerates systems and shared resources. After obtaining elevated credentials, the attacker moves laterally using PsExec, ultimately reaching the central database server containing sensitive financial and customer data.

### Scenario 2 - Stolen Credentials Unlock the Payment Database (T1003)
After establishing internal network access, the threat actor deploys Mimikatz to extract plaintext passwords, hashes, and Kerberos tickets from LSASS memory. With domain administrator credentials obtained, the attacker authenticates across systems without triggering traditional defenses, pivots deeper into the IT_Network, accesses the payment database, and exfiltrates sensitive financial data.

### Scenario 3 - Click to Compromise via Chat Malicious Link (T1204)
The attacker delivers a phishing email containing a disguised malicious attachment (e.g., fake invoice). Once the victim opens the file, it executes silently via PowerShell or Metasploit, establishing an initial foothold. The attacker gains control of the workstation, conducts internal reconnaissance, and moves toward the central database - all operating under legitimate user context to evade detection.

---

## Data Flow Diagram

> Add architecture diagrams to the `/diagrams` folder and reference them here.

![Architecture Diagram](../diagrams/architecture.png)| Internet-facing / Internal | Critical / High / Medium / Low |

---

## Entry Points

### External Entry Points

- [ ] Public-facing web applications
- [ ] VPN / Remote access portals
- [ ] Email (phishing vector)
- [ ] DNS / Domain infrastructure
- [ ] Cloud service APIs

### Internal Entry Points

- [ ] Lateral movement via compromised endpoints
- [ ] Insider threat / privileged user abuse
- [ ] Supply chain / third-party integrations
- [ ] Unpatched internal services

---

## Exposure Summary

| Vector | Exposure Level | Notes |
|---|---|---|
| Web Application | High | [Detail] |
| Email | High | [Detail] |
| Remote Access | Medium | [Detail] |
| Internal Network | Medium | [Detail] |
| Cloud Infrastructure | Low-High | [Detail] |

---

## Data Flow Diagram

> Add architecture diagrams to the `/diagrams` folder and reference them here.

![Architecture Diagram](../diagrams/architecture.png)
