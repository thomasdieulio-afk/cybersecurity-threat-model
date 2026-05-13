# Threat Actors

This document profiles the adversaries identified as relevant to the Tempus Fugit environment, mapped to key assets and MITRE ATT&CK techniques.

---

## Key Assets in Scope

| Asset | Description |
|---|---|
| Intellectual Property (IP) | Proprietary game source code |
| Payment Information | User billing data from paid subscriptions |
| In-Game Private Chat System | Features end-to-end encryption |

---

## Actor Profiles

### Actor 1 - Insider Threats

| Attribute | Detail |
|---|---|
| **Type** | Insider (employee, contractor, departing staff) |
| **Motivation** | Financial gain (sell trade secrets); career advantage (use IP at new job); revenge or dissatisfaction after workplace conflict |
| **Capability** | Medium - no technical exploit needed; misuse of legitimate access |
| **Targeting** | Intellectual Property (source code), Payment Information (billing records), Chat System (private logs) |
| **Known TTPs** | T1052 Exfiltration Over Physical Medium; T1048 Exfiltration Over Alt Protocol; T1078 Valid Accounts |

**Notes:** Insiders already have authenticated access, making the barrier extremely low. Typical behaviors: copying files to USB or personal cloud, downloading sensitive docs beyond their role, grabbing source code before quitting. They do not hack - they misuse legitimate access. Abuse of chat moderation tools is a secondary concern for privileged admins.

---

### Actor 2 - Nation-State Actors

| Attribute | Detail |
|---|---|
| **Type** | Nation-State / Government-sponsored |
| **Motivation** | Economic advantage - boost national industries without paying for R&D; military/strategic advantage; stealing IP saves years of research and billions of dollars |
| **Capability** | High - well-resourced, sophisticated, persistent |
| **Targeting** | Intellectual Property (proprietary game source code) |
| **Known TTPs** | T1566.001 Spearphishing Attachment; T1105 Ingress Tool Transfer; T1041 Exfiltration Over C2 Channel; T1135 Network Share Discovery |

**Notes:** Use spear phishing to compromise employees, deploy malware and backdoors for persistent access, move laterally to reach IP repositories, and conduct stealthy exfiltration over extended periods to avoid detection. Advanced, persistent, and hard to detect.

---

### Actor 3 - Advanced Persistent Threats (APTs)

| Attribute | Detail |
|---|---|
| **Type** | APT (government-sponsored or large criminal organizations) |
| **Motivation** | Long-term data theft for profit or sponsorship; quietly extract valuable IP over time |
| **Capability** | High - advanced tooling, patient, methodical |
| **Targeting** | Intellectual Property (source code), Payment Information |
| **Known TTPs** | T1566 Phishing; T1078 Valid Accounts; T1041 Exfiltration Over C2 Channel; T1003 Credential Dumping |

**Notes:** Operate silently over long periods, blending into normal network activity. Goal is to quietly extract valuable IP or financial data without triggering defenses.

---

### Actor 4 - Cybercriminals / Organized Crime

| Attribute | Detail |
|---|---|
| **Type** | Cybercriminal / Organized Crime |
| **Motivation** | Direct financial gain; sell payment card data in bulk on underground markets; account takeover; ransomware extortion |
| **Capability** | Medium to High - commodity tooling to sophisticated custom malware |
| **Targeting** | Payment Information (primary), Intellectual Property (secondary - sell on black market), Chat System (phishing vector) |
| **Known TTPs** | T1190 Exploit Public-Facing Application; T1566 Phishing; T1204 User Execution; T1486 Data Encrypted for Impact |

**Notes:** Purely profit-driven. Against payment data: SQL injection, Magecart-style web skimming (injecting code into checkout pages), credential stuffing. Against chat: fake support messages, phishing links, malicious files. Ransomware used for double-extortion.

---

### Actor 5 - Script Kiddies

| Attribute | Detail |
|---|---|
| **Type** | Low-skill opportunistic attacker |
| **Motivation** | Disruption and attention; bypassing filters; messing around rather than serious financial gain |
| **Capability** | Low - off-the-shelf tools, publicly available exploits |
| **Targeting** | In-Game Private Chat System |
| **Known TTPs** | T1498 Network Denial of Service; T1204 User Execution |

**Notes:** Target the chat system for disruption - spamming, flooding, bypassing word filters, abusing weak chat APIs or rate limits. Low sophistication but can cause service disruption and reputational damage.

---

## MITRE ATT&CK TTP Mapping

| Actor | Tactic | Technique | Technique ID |
|---|---|---|---|
| Insider Threats | Exfiltration | Exfiltration Over Physical Medium | T1052 |
| Insider Threats | Defense Evasion | Valid Accounts | T1078 |
| Nation-State Actors | Initial Access | Spearphishing Attachment | T1566.001 |
| Nation-State Actors | Discovery | Network Share Discovery | T1135 |
| Nation-State Actors | Exfiltration | Exfiltration Over C2 Channel | T1041 |
| APTs | Credential Access | Credential Dumping | T1003 |
| APTs | Initial Access | Phishing | T1566 |
| Cybercriminals | Initial Access | Exploit Public-Facing Application | T1190 |
| Cybercriminals | Execution | User Execution | T1204 |
| Cybercriminals | Impact | Data Encrypted for Impact (Ransomware) | T1486 |
| Script Kiddies | Impact | Network Denial of Service | T1498 |

---

*Reference: [MITRE ATT&CK](https://attack.mitre.org/)*
