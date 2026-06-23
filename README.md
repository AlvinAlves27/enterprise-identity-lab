# Enterprise Identity Lab

*Hybrid identity infrastructure built from scratch — on-premises Active Directory federated to Microsoft Entra ID, with SAML 2.0 SSO to Salesforce.*

![Overview](images/architecture/overview.svg)

---

## What this lab proves

This lab builds end-to-end enterprise identity infrastructure from the ground up. It starts with an on-premises Active Directory environment hosted in Azure, extends to a hybrid identity model via Microsoft Entra Connect, and finishes with SAML 2.0 federation to Salesforce.

The goal is to demonstrate the full IAM stack an enterprise actually relies on — directory services, hybrid sync, identity lifecycle, and federated SSO — not just isolated configurations.

---

## Lab walkthroughs

The lab spans three parts, each building on the prior:

| Part | What it builds | Stack |
| --- | --- | --- |
| 📘 [**Part 1: On-Premises Foundation**](docs/01-on-premises-foundation.md) | Active Directory forest, DNS, OUs, security groups, RBAC, domain-joined Windows 11 client | AD DS · Windows Server 2019 · Windows 11 · Azure VNet |
| 📘 [**Part 2: Hybrid Identity Bridge**](docs/02-hybrid-identity-bridge.md) | Custom domain verification, Entra Connect deployment, password hash sync, UPN matching, hybrid device registration | Microsoft Entra ID · Entra Connect · Password Hash Sync |
| 📘 [**Part 3: SSO Integration**](docs/03-sso-integration.md) | SAML 2.0 federation to Salesforce, attribute mapping, certificate-based trust | SAML 2.0 · X.509 · Federation Metadata |

---

## Architecture highlights

**Part 1 — On-prem foundation:** A Windows Server 2019 domain controller running AD DS and DNS, plus a domain-joined Windows 11 Enterprise client, all hosted inside an isolated Azure virtual network. Five user accounts and a nested security group (`onpremadmin` → Domain Admins → Enterprise Admins) implement RBAC.

![On-Prem Foundation](images/architecture/on-prem-foundation.svg)

**Part 2 — Hybrid bridge:** Microsoft Entra Connect synchronizes the on-prem directory to a verified custom domain (`zerotrustlabs.it.com`) in Microsoft Entra ID, using Password Hash Synchronization for seamless cloud authentication. UPN suffix matching enables single sign-on across environments.

![Hybrid Sync](images/architecture/hybrid-sync.svg)

**Part 3 — SAML federation:** A Salesforce enterprise application is federated to Entra ID via SAML 2.0. X.509 certificate trust, Federation ID mapping, and `Login with sts` provider integration deliver passwordless SSO from the Windows-connected session straight into Salesforce.

![SSO Flow](images/architecture/sso-flow.svg)

---

## Skills demonstrated

- **Hybrid identity architecture** — bridging on-prem AD and Entra ID at the directory, user, and device layers
- **Active Directory administration** — forest creation, OU design, security groups, RBAC with nested membership
- **DNS** — split DNS (internal + external), client DNS configuration, troubleshooting domain resolution
- **Microsoft Entra Connect** — installation, Express Settings, Password Hash Synchronization, delta sync via PowerShell
- **Custom domain verification** — DNS TXT record provisioning at the registrar (Namecheap)
- **UPN suffix management** — matching cloud and on-prem identifiers for seamless SSO
- **Hybrid device registration** — domain join + Entra Registered, foundation for Conditional Access
- **SAML 2.0 federation** — IdP/SP trust establishment, certificate exchange, claim mapping, Federation ID
- **Federation troubleshooting** — Enterprise Admins permission errors during Entra Connect setup
- **PowerShell automation** — `Start-ADSyncSyncCycle -PolicyType Delta` for on-demand synchronization

---

## Tech stack

![Microsoft Entra](https://img.shields.io/badge/Microsoft_Entra_ID-0078D4?style=flat-square&logo=microsoft&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active_Directory-1B3A5B?style=flat-square&logo=windows&logoColor=white)
![Entra Connect](https://img.shields.io/badge/Entra_Connect-0078D4?style=flat-square&logo=microsoft&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server_2019-0078D4?style=flat-square&logo=windows&logoColor=white)
![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-00A1E0?style=flat-square&logo=salesforce&logoColor=white)
![SAML](https://img.shields.io/badge/SAML_2.0-EE0000?style=flat-square)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?style=flat-square&logo=powershell&logoColor=white)

---

## Repo structure

```
enterprise-identity-lab/
├── README.md                                ← You are here
├── docs/
│   ├── 01-on-premises-foundation.md         ← Part 1 deep dive
│   ├── 02-hybrid-identity-bridge.md         ← Part 2 deep dive
│   └── 03-sso-integration.md                ← Part 3 deep dive
└── images/
    ├── architecture/                        ← System diagrams
    ├── part-1/                              ← Part 1 screenshots
    ├── part-2/                              ← Part 2 screenshots
    └── part-3/                              ← Part 3 screenshots
```

> **Note on screenshots:** Personally identifiable information (email addresses, tenant IDs) has been redacted from screenshots before publishing.

---

📍 United States · 🌐 [alvinalves.com](https://alvinalves.com) · 💼 [LinkedIn](https://www.linkedin.com/in/alvin-alves/)
