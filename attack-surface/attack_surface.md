# Attack Surface

Enumeration of all identified entry points, exposed assets, and attack vectors
within the scope of this threat model.

---

## Asset Inventory

| Asset | Type | Exposure | Criticality |
|---|---|---|---|
| [Asset Name] | Server / Endpoint / App / Network | Internet-facing / Internal | Critical / High / Medium / Low |

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
