# Honeypot Strategies

Deception-based detection architecture for **Tempus Fugit**, designed to identify adversarial activity early, slow lateral movement, and gather threat intelligence — aligned to identified attack vectors (TA0001, T1003, T1204).

---

## Strategy Overview

The honeypot strategy uses a combination of:
- **Network honeypots** — Fake services (SSH, SMB, database) that generate critical alerts on any connection
- **Canary tokens** — Embedded triggers in documents, credentials, and URLs that phone home on access
- **Breadcrumb architecture** — False trails (fake admin accounts, fake source code folders) that lure attackers away from real assets

---

## Honeypot Deployments

| Honeypot | Type | Location | Threat Addressed | Purpose |
|---|---|---|---|---|
| Fake SSH / SMB File Server | Network | IT_Network subnet (DMZ-adjacent) | T1135 Network Share Discovery | Alert on any enumeration of fake file shares |
| Canary Admin Account (svc-backup-admin) | Credential | Active Directory | T1003 Credential Dumping | Any login attempt means credentials were dumped — immediate critical alert |
| Canary Source Code Folder (/repos/game-engine-v3-backup) | File-based | File server | T1052 / T1041 Exfiltration | Fake high-value IP folder; opening any file triggers a phone-home canary token |
| Fake Payment Database (payment_db_legacy) | Network | Internal VLAN (isolated) | T1190 / T1003 | Any connection to this dummy DB means attacker reached payment tier |
| Canary Chat API Token | Web / API | Chat server infrastructure | T1204 / T1056.004 | Fake API key embedded in config; any use indicates chat server compromise |
| Canary Document — HR Policy | File-based | Shared drive (HR folder) | T1052 Insider Threat | Document with embedded token; opened outside HR triggers insider threat alert |
| Canary URL Token | Web | Fake login/admin panel | TA0001 Phishing | Detects credential harvesting — any visit to fake admin URL triggers alert |

---

## Alert Triggers

Any interaction with the following generates an **immediate Critical-priority alert** routed to the SOC:

1. **Login attempt on canary admin account** (svc-backup-admin) — indicates credential dumping succeeded (T1003)
2. **Any connection to fake file server / SMB share** — indicates network share enumeration (T1135)
3. **Opening of canary source code document** — phone-home token fires, indicating IP exfiltration attempt (T1041)
4. **Any connection to fake payment database** — indicates attacker reached payment tier (T1190)
5. **Use of canary chat API token** — indicates chat server API compromise (T1204 / T1056.004)
6. **DNS resolution of canary domain** — indicates C2 beaconing or credential harvesting in progress (T1041)
7. **Opening of canary HR document outside HR team** — indicates insider threat activity (T1052)

---

## Integration with Detection Stack

| Canary Trigger | SIEM Rule | Severity | Response Playbook |
|---|---|---|---|
| Canary admin login | CANARY_CRED_USE | Critical | Isolate source IP, force AD password reset, page SOC |
| Fake SMB share access | CANARY_SMB_ACCESS | Critical | Isolate host, capture network traffic |
| Canary document opened | CANARY_DOC_OPEN | Critical | Identify opener, isolate workstation, begin IR |
| Fake payment DB connection | CANARY_PAYMENT_DB | Critical | Isolate host, lock payment system access, page IR team |
| Canary chat API key used | CANARY_CHAT_API | Critical | Revoke token, capture session, begin chat server IR |

---

## Canary Token Resources

- canarytokens.org — Free hosted canary tokens (documents, URLs, DNS, AWS keys)
- thinkst/canarytokens (GitHub) — Self-hosted option for full control
- OpenCanary (GitHub) — Self-hosted network honeypot daemon (SSH, SMB, HTTP, MySQL)

---

*MITRE ATT&CK Reference: https://attack.mitre.org/*
