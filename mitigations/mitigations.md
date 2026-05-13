# Mitigations

Prioritized security controls and countermeasures for **Tempus Fugit**, mapped directly to identified attack vectors (TA0001, T1003, T1204) and ordered by risk reduction impact. All controls target a deployment deadline of **October 29, 2026**.

---

## Priority Matrix

| Priority | Mitigation | Threat Addressed | MITRE TTP | Effort | Status |
|---|---|---|---|---|---|
| P1 | Multi-Factor Authentication (MFA) on all external-facing systems | Credential attacks, phishing, initial access | TA0001 / T1078 | Low | [ ] Planned |
| P1 | Email phishing filter (DMARC / DKIM / SPF) | Spearphishing initial access | TA0001 | Low | [ ] Planned |
| P1 | Credential Guard (Windows) | Credential dumping via LSASS | T1003 | Low-Medium | [ ] Planned |
| P1 | Least privilege enforcement | Credential abuse, insider threat, lateral movement | T1003 / T1078 | Medium | [ ] Planned |
| P2 | Endpoint Detection and Response (EDR) | Malware execution, lateral movement, credential dumping | T1003 / T1204 | Medium | [ ] Planned |
| P2 | Web Application Firewall (WAF) with SQL injection rules | Payment API exploitation | T1190 | Medium | [ ] Planned |
| P2 | Application allowlisting | Malicious user execution on endpoints | T1204 | Medium | [ ] Planned |
| P2 | Canary token deployment (credentials, documents, URLs) | Insider threat, credential dumping, early lateral movement detection | T1003 / T1052 | Low | [ ] Planned |
| P2 | Network segmentation - isolate payment DB and file server VLANs | Lateral movement post-compromise | T1135 / T1021 | High | [ ] Planned |
| P3 | Security awareness and phishing simulation training | Social engineering, user execution | TA0001 / T1204 | Low | [ ] Planned |
| P3 | Patch management / vulnerability management program | Exploit-based attacks on public-facing apps | T1190 | Medium | [ ] Planned |
| P3 | Chat API rate limiting and input validation | Malicious payload delivery via chat | T1204 | Low-Medium | [ ] Planned |
| P3 | Zero trust architecture | All threat actors | All | High | [ ] Planned |

---

## Control Details

### MFA on All External-Facing Systems (P1)

- **Type:** Preventive
- **Implementation:** Enforce MFA via identity provider (Azure AD / Okta / Duo) on VPN, Microsoft 365, admin consoles, payment portal, and game client login
- **Covers:** TA0001 Initial Access, T1078 Valid Accounts
- **Evidence:** Microsoft reports MFA blocks 99.9% of automated account attacks (Verizon DBIR)
- **Deadline:** October 29, 2026

### Email Security - DMARC / DKIM / SPF (P1)

- **Type:** Preventive
- **Implementation:** Configure DNS records for all sending domains; enforce DMARC policy = reject
- **Covers:** TA0001 Spearphishing, email spoofing campaigns
- **Deadline:** October 29, 2026

### Credential Guard (P1)

- **Type:** Preventive
- **Implementation:** Enable Windows Credential Guard on all domain-joined workstations and servers to protect LSASS memory from Mimikatz-style attacks
- **Covers:** T1003 Credential Dumping
- **Deadline:** October 29, 2026

### Endpoint Detection and Response - EDR (P2)

- **Type:** Detective + Responsive
- **Implementation:** Deploy EDR agent (CrowdStrike, SentinelOne, or Microsoft Defender for Endpoint) to all endpoints; configure alerts for LSASS access, unsigned binary execution, lateral movement
- **Covers:** T1003 Credential Dumping, T1204 User Execution, lateral movement
- **Goal:** Detect 100% of credential dumping attempts through EDR alerts
- **Deadline:** October 29, 2026

### Application Allowlisting (P2)

- **Type:** Preventive
- **Implementation:** Deploy application allowlisting (AppLocker or WDAC) on all endpoints to prevent unauthorized or unsigned executables from running
- **Covers:** T1204 User Execution
- **Goal:** Reduce malicious user execution on endpoints by 75% within 6 months
- **Deadline:** October 29, 2026

### Canary Token Deployment (P2)

- **Type:** Detective
- **Implementation:** Deploy canary tokens across fake admin credentials in AD, canary source code folders on file server, canary documents in shared drives, canary chat API keys, canary DNS domains
- **Covers:** T1003, T1052, T1135, T1041 - triggers on insider threat and lateral movement
- **Deadline:** October 29, 2026

### Network Segmentation (P2)

- **Type:** Preventive
- **Implementation:** VLAN isolation - payment database and file server on separate VLANs with strict firewall ACLs; zero-trust micro-segmentation for lateral movement prevention
- **Covers:** T1135 Network Share Discovery, T1021 Lateral Movement
- **Deadline:** October 29, 2026

### Security Awareness Training (P3)

- **Type:** Preventive
- **Implementation:** Quarterly phishing simulations (Gophish or KnowBe4); mandatory training for all employees on phishing, social engineering, and safe file handling
- **Covers:** TA0001 Initial Access, T1204 User Execution
- **Evidence:** IBM reports faster detection and human training significantly reduces breach impact; Verizon DBIR shows 70%+ of breaches involve human interaction
- **Deadline:** October 29, 2026

---

## Mitigation Goals Summary

| Goal | Metric | Deadline |
|---|---|---|
| Prevent credential theft and detect unauthorized credential use | Reduce successful unauthorized logins by 80% within 6 months | October 29, 2026 |
| Detect 100% of credential dumping attempts | EDR + SIEM alerts on all LSASS access events | October 29, 2026 |
| Reduce malicious user execution | Reduce unauthorized/malicious file execution by 75% within 6 months | October 29, 2026 |

---

## Stakeholder Alignment

| Solution | Stakeholder | Why They Should Care |
|---|---|---|
| MFA + Credential Guard + EDR + SIEM | IT Security Team | Credential dumping can lead to full system compromise, data breaches, and undetected attacker access if not prevented and detected |
| Application Allowlisting + Email Filtering + User Training | IT Security Team | Malicious user execution is a major entry point for malware and can quickly lead to widespread system compromise if not prevented |

---

## Mitigation Tracking

> Update Status checkboxes above as controls are implemented. Link to change tickets or PRs where applicable. All controls must be fully operational by **October 29, 2026**.
