# Mitigations

Prioritized security controls and countermeasures mapped to identified threats,
ordered by risk reduction impact.

---

## Priority Matrix

| Priority | Mitigation | Threat Addressed | Effort | Status |
|---|---|---|---|---|
| P1 | MFA on all external-facing systems | Credential attacks | Low | [ ] Planned |
| P1 | Email phishing filter (DMARC/DKIM/SPF) | Phishing | Low | [ ] Planned |
| P2 | Endpoint Detection & Response (EDR) | Malware, lateral movement | Medium | [ ] Planned |
| P2 | Network segmentation | Lateral movement | High | [ ] Planned |
| P2 | Canary token deployment | Insider threat, early detection | Low | [ ] Planned |
| P3 | Vulnerability management program | Exploit-based attacks | Medium | [ ] Planned |
| P3 | Security awareness training | Social engineering | Low | [ ] Planned |
| P3 | Zero trust architecture | All threat actors | High | [ ] Planned |

---

## Control Details

### MFA on External Systems

- **Type:** Preventive
- **Implementation:** Enforce MFA via identity provider (Okta, Azure AD, Duo)
- **Covers:** Remote access, VPN, admin consoles, email

### Email Security (DMARC / DKIM / SPF)

- **Type:** Preventive
- **Implementation:** Configure DNS records; enforce DMARC policy = reject
- **Covers:** Email spoofing, phishing campaigns

### Endpoint Detection & Response

- **Type:** Detective + Responsive
- **Implementation:** Deploy EDR agent to all endpoints; configure alerting
- **Covers:** Malware execution, lateral movement, privilege escalation

### Network Segmentation

- **Type:** Preventive
- **Implementation:** VLAN isolation between critical assets; firewall rules
- **Covers:** Lateral movement post-compromise

---

## Mitigation Tracking

> Update the Status column above as controls are implemented.
> Link to change tickets or PRs where applicable.
