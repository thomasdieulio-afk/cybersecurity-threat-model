# Detection Strategies

Behavioral detection logic, SIEM correlation rules, and IOC patterns for **Tempus Fugit**, aligned to identified attack vectors (TA0001, T1003, T1204).

---

## Detection Philosophy

1. **Prevention-layer telemetry** - MFA enforcement, email filtering, application allowlisting, EDR blocking
2. **Behavioral analytics** - Anomaly detection on login patterns, process execution, lateral movement
3. **Threat intelligence correlation** - MITRE ATT&CK-aligned IOC matching (TA0001, T1003, T1204)

---

## SIEM Correlation Rules

| Rule Name | Trigger Condition | Severity | MITRE TTP | Response |
|---|---|---|---|---|
| Spearphishing Initial Access | Email with malicious link/attachment delivered to employee | Critical | TA0001 | Block + quarantine + alert SOC |
| Credential Dumping Detected | LSASS memory access by non-system process | Critical | T1003 | Isolate endpoint + alert + force credential reset |
| Anomalous Login - Admin Account | Admin login from new IP or outside business hours | High | T1078 | Lock account + alert |
| Brute Force - Payment API | 10+ failed auth attempts against payment API in 5 min | High | T1190 | Rate-limit IP + alert |
| SQL Injection Attempt | SQL metacharacter pattern in payment API input fields | High | T1190 | Block request + alert |
| Lateral Movement via PsExec | PsExec or SMB exec spawned from non-admin host | High | T1135 / T1021 | Isolate host + alert |
| Network Share Enumeration | Bulk SMB share discovery from single host | High | T1135 | Alert + throttle |
| User Executed Malicious File | Unsigned or low-reputation binary execution on endpoint | High | T1204 | Quarantine + alert |
| Malicious Chat Link Delivery | URL in chat matching known phishing or malware domain | High | T1204 | Block URL + alert |
| Exfiltration Over C2 | Large outbound data transfer to unknown external IP | Critical | T1041 | Block + alert + capture PCAP |
| Canary Token Triggered | Any access to designated canary credential or document | Critical | - | Immediate escalation |

---

## Detection Coverage by Attack Vector

| Attack Vector | Detection Method | Coverage | MITRE ID |
|---|---|---|---|
| Spearphishing / Initial Access | Email gateway + SIEM alert on delivery + user report | High | TA0001 |
| Credential Dumping (Mimikatz / LSASS) | EDR process monitoring (LSASS access events) + SIEM | High | T1003 |
| SQL Injection against Payment API | WAF + SIEM input validation alerts | Medium | T1190 |
| Network Share Discovery (SMB enum) | SIEM SMB event correlation + EDR | Medium | T1135 |
| User Execution (malicious chat links) | EDR process execution logging + email/chat URL filtering | High | T1204 |
| Lateral Movement (PsExec / RDP) | SIEM correlation of remote exec events + EDR | High | T1021 |
| C2 Exfiltration | DNS monitoring + proxy logs + outbound data volume alerts | Medium | T1041 |

---

## Log Sources

| Source | Data Type | Retention | Primary Use |
|---|---|---|---|
| Firewall | Network flow, block events | 90 days | C2 detection, exfiltration |
| EDR | Process, file, network events | 180 days | T1003, T1204, lateral movement |
| SIEM | Aggregated and correlated alerts | 1 year | All vectors |
| Active Directory | Auth events, privilege changes, group changes | 1 year | T1078, T1003 |
| Email Gateway | Phishing delivery, attachment analysis | 90 days | TA0001 |
| Web Application Firewall (WAF) | SQL injection, web attacks | 90 days | T1190 |
| Chat Server Logs | Message delivery, link clicks, API calls | 90 days | T1204 |
| DNS Logs | Canary domain resolutions, C2 beaconing | 180 days | T1041, canary tokens |

---

## Indicators of Compromise (IOCs)

| Type | Description | Associated Vector | Confidence |
|---|---|---|---|
| Process name | lsass.exe accessed by non-system process | T1003 Credential Dumping | High |
| File signature | Mimikatz or similar credential harvesting tool | T1003 | High |
| Network pattern | Outbound connection to new external IP on unusual port | T1041 C2 Exfiltration | Medium |
| URL pattern | Fake login page URL matching corporate brand | TA0001 Phishing | High |
| Behavior | Bulk SMB share enumeration from a single workstation | T1135 Share Discovery | High |
| Behavior | PsExec spawned from non-admin workstation | T1021 Lateral Movement | High |
| DNS | Resolution of canary domain by internal host | Canary detection | Critical |

---

## Goals and Deadlines

| Goal | Target | Deadline |
|---|---|---|
| Reduce unauthorized logins from compromised credentials by 80% | MFA + EDR + SIEM fully deployed | October 29, 2026 |
| Detect 100% of credential dumping attempts via EDR/SIEM alerts | EDR monitoring on all endpoints | October 29, 2026 |
| Reduce malicious user execution on endpoints by 75% | Allowlisting + email filtering + user training | October 29, 2026 |

---

*MITRE ATT&CK Reference: https://attack.mitre.org/*
