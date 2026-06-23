# Enterprise Identity Lab

> **Hybrid Identity & SSO Implementation** — A comprehensive lab demonstrating enterprise-grade identity management using Microsoft Active Directory, Entra ID, and SAML-based Single Sign-On with Salesforce.

[![Microsoft Entra](https://img.shields.io/badge/Microsoft_Entra_ID-0078D4?style=flat-square&logo=microsoft&logoColor=white)](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id)
[![Active Directory](https://img.shields.io/badge/Active_Directory-0078D4?style=flat-square&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-domain-services)
[![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![SAML](https://img.shields.io/badge/SAML_2.0-FF6F00?style=flat-square)](https://docs.oasis-open.org/security/saml/v2.0/)
[![Salesforce](https://img.shields.io/badge/Salesforce-00A1E0?style=flat-square&logo=salesforce&logoColor=white)](https://www.salesforce.com/)

---

## Overview

This lab walks through the end-to-end design and deployment of a hybrid enterprise identity environment. It covers on-premises Active Directory, synchronization to Microsoft Entra ID via Entra Connect, and federation to a SaaS application (Salesforce) using SAML 2.0.

![Overview](architecture/overview.svg)

---

## Lab Parts

| Part | Focus | Status |
| --- | --- | --- |
| [Part 1: On-Premises Foundation](docs/01-on-premises-foundation.md) | Active Directory, Group Policy, OU design | ✅ Complete |
| [Part 2: Hybrid Identity Bridge](docs/02-hybrid-identity-bridge.md) | Entra Connect, custom domain, hybrid sync | ✅ Complete |
| [Part 3: SSO Integration](docs/03-sso-integration.md) | SAML 2.0 federation with Salesforce | ✅ Complete |

---

## Architecture at a Glance

**On-Premises Foundation**
![On-Prem Foundation](architecture/on-prem-foundation.svg)

**Hybrid Identity Sync**
![Hybrid Sync](architecture/hybrid-sync.svg)

**SSO Federation Flow**
![SSO Flow](architecture/sso-flow.svg)

---

## Skills Demonstrated

- **Active Directory Domain Services** — domain controller deployment, OU structure design, security group management, GPO configuration
- **Hybrid Identity** — Microsoft Entra Connect installation, custom domain verification, attribute synchronization, password hash sync
- **Federation & SSO** — SAML 2.0 protocol, X.509 certificate-based trust, federation metadata exchange, claim/attribute mapping
- **Cloud Infrastructure** — Azure VM deployment, virtual networking, NSG configuration, public/private IP management
- **Identity Lifecycle** — user provisioning, role assignment, application access governance
- **Documentation** — multi-section technical writing, architecture diagramming, configuration evidence capture

---

## Tech Stack

![Windows Server](https://img.shields.io/badge/Windows_Server_2019-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows_11-0078D4?style=flat-square&logo=windows11&logoColor=white)
![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?style=flat-square&logo=powershell&logoColor=white)
![Entra ID](https://img.shields.io/badge/Microsoft_Entra_ID-0078D4?style=flat-square&logo=microsoft&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-00A1E0?style=flat-square&logo=salesforce&logoColor=white)

---

## About

Built and documented by **Alvin Alves** — Identity & Access Management professional based in Brooklyn, NY.

- 🌐 Portfolio: [alvinalves.com](https://alvinalves.com)
- 💼 LinkedIn: [linkedin.com/in/alvin-alves](https://www.linkedin.com/in/alvin-alves/)
