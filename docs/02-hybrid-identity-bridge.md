# Part 2: Hybrid Identity Bridge

> **Entra Connect & Directory Sync** — extending on-premises Active Directory to Microsoft Entra ID for seamless hybrid authentication.

## Objective

Establish hybrid identity infrastructure connecting on-premises Active Directory to Microsoft Entra ID, enabling seamless authentication across environments and demonstrating enterprise-grade directory synchronization.

## Technologies

- Microsoft Entra ID (Azure AD)
- Microsoft Entra Connect
- Active Directory Domain Services
- Password Hash Synchronization (PHS)
- Namecheap DNS Management
- Azure Virtual Machines

## Architecture

![Hybrid Sync Architecture](../images/architecture/hybrid-sync.svg)

---

## Section 2.1: Custom Domain Verification

### Overview

Before synchronizing identities, the custom domain must be verified in Entra ID to ensure users authenticate with organizational email addresses rather than the default `.onmicrosoft.com` domain.

### Steps & Configuration

**Step 1: Initiate Domain Addition**

Navigate to *Microsoft Entra admin center → Settings → Domain names → Add custom domain*.

![Add Custom Domain](../images/part-2/01-add-custom-domain.png)

**Step 2: Configure DNS TXT Record**

Add the verification TXT record to your domain registrar:

| Field | Value |
| --- | --- |
| Type | TXT |
| Host | `@` (root domain) |
| Value | `MS=ms########` (provided by Entra) |
| TTL | Automatic |

![DNS TXT Record in Namecheap](../images/part-2/02-dns-txt-record-namecheap.png)

**Step 3: Verify Domain Ownership**

Return to Entra and click *Verify* after DNS propagation (typically 5–15 minutes).

![Verify Domain](../images/part-2/03-verify-domain.png)

**Step 4: Confirm Verification Status**

![Domain Verified](../images/part-2/04-domain-verified.png)

### Outcome

- Custom domain `zerotrustlabs.it.com` successfully verified
- Users can now be created with organizational email addresses
- Foundation established for hybrid identity synchronization

### Key Takeaways

- DNS TXT record verification proves domain ownership without requiring email access
- Verification is a prerequisite for user synchronization with matching UPNs
- Multiple domains can be added for complex organizational structures

---

## Section 2.2: Cloud User & Group Management

### Overview

Create cloud-native users and security groups in Entra ID to establish baseline identity management before introducing hybrid synchronization.

### Steps & Configuration

**Step 1: Create Cloud User**

Navigate to *Users → New user → Create new user*.

| Field | Value |
| --- | --- |
| User principal name | `entrauser1@zerotrustlabs.it.com` |
| Display name | `entrauser1` |
| Auto-generate password | Yes |

![Create Cloud User](../images/part-2/05-create-cloud-user.png)

**Step 2: Create Security Group**

Navigate to *Groups → New group*.

| Field | Value |
| --- | --- |
| Group type | Security |
| Group name | `EntraSourced` |
| Membership type | Assigned |

![Create Security Group](../images/part-2/06-create-security-group.png)

**Step 3: Add User to Group**

*Edit group → Members → Add members → Select `entrauser1`.*

![Group Add User](../images/part-2/07-group-add-user.png)

![Group Members Confirmed](../images/part-2/08-group-members-confirmed.png)

**Step 4: Assign Directory Role**

Navigate to *User → Assigned roles → Add assignments → Global Reader*.

![Assign Global Reader Role](../images/part-2/09-assign-global-reader-role.png)

![Role Assigned](../images/part-2/10-role-assigned.png)

**Step 5: Verify User Authentication**

Test login at `login.microsoftonline.com` with new credentials.

![entrauser1 Sign-in](../images/part-2/11-entrauser1-signin.png)

![entrauser1 Success](../images/part-2/12-entrauser1-success.png)

### Outcome

- Cloud-only user `entrauser1` created and functional
- Security group `EntraSourced` established for access management
- Global Reader role assigned for read-only administrative access
- Authentication verified successfully

### Key Takeaways

- Cloud-only users have no on-premises footprint (On-premises sync = "No")
- Security groups enable scalable permission management
- Global Reader provides audit/monitoring access without modification rights
- Role-based access control is fundamental to zero-trust architecture

---

## Section 2.3: Entra Connect Installation & Configuration

### Overview

Deploy Microsoft Entra Connect to establish the synchronization bridge between on-premises Active Directory and cloud Entra ID, enabling hybrid identity scenarios.

### Steps & Configuration

**Step 1: Download Entra Connect**

Navigate to *Microsoft Entra admin center → Hybrid management → Microsoft Entra Connect → Download*.

![Entra Connect Download](../images/part-2/13-entra-connect-download.png)

![Entra Connect Installer](../images/part-2/14-entra-connect-installer.png)

**Step 2: Troubleshoot Permissions**

During installation, connecting to AD DS may fail with a permission error:

![Permission Error](../images/part-2/15-permission-error.png)

**Resolution:** Add the service account to the Enterprise Admins group.

![Add to Enterprise Admins](../images/part-2/16-add-to-enterprise-admins.png)

**Step 3: Complete Configuration**

- Select Express Settings for standard deployment
- Enable Password Hash Synchronization
- Configure Azure AD credentials (Global Administrator)
- Configure AD DS credentials (Enterprise Admin)

![Entra Connect Configuration Complete](../images/part-2/17-entra-connect-config-complete.png)

### Configuration Summary

| Setting | Value |
| --- | --- |
| Sync Mode | Express Settings |
| Sign-on Method | Password Hash Synchronization |
| Source Directory | `zerotrustlabs.local` |
| Target Directory | `zerotrustlabs.it.com` |
| Sync Interval | 30 minutes (default) |

### Outcome

- Entra Connect successfully installed on Domain Controller
- Password Hash Synchronization enabled
- Initial sync completed
- Bi-directional identity bridge established

### Key Takeaways

- Enterprise Admins membership is required for the AD DS connector
- Express Settings suitable for single-forest, single-domain scenarios
- Password Hash Sync provides seamless cloud authentication without federation infrastructure
- Troubleshooting permissions is a common real-world deployment challenge

---

## Section 2.4: Global Administrator Setup

### Overview

Create a dedicated Global Administrator account for cloud management, separating daily operations from privileged access.

### Steps & Configuration

**Step 1: Create Admin User**

![Create Global Admin](../images/part-2/18-create-global-admin.png)

**Step 2: Verify User List**

Confirm all users visible in Entra admin center.

![Users List](../images/part-2/19-users-list.png)

**Step 3: Test Authentication**

![Global Admin Sign-in](../images/part-2/20-globaladmin-signin.png)

### Azure Infrastructure Status

![VMs Running](../images/part-2/21-vms-running.png)

### Outcome

- Dedicated Global Administrator account created
- Separation of duties established (daily user vs. admin)
- Both VMs operational in Azure East US region

### Key Takeaways

- Dedicated admin accounts improve security auditing
- Global Administrator should be used sparingly (break-glass scenarios)
- Consider Privileged Identity Management (PIM) for just-in-time access

---

## Section 2.5: UPN Configuration & Sync Verification

### Overview

Configure User Principal Name (UPN) suffixes in Active Directory to match the verified cloud domain, enabling seamless hybrid authentication.

### Steps & Configuration

**Step 1: Add UPN Suffix in Active Directory**

Open *Active Directory Domains and Trusts → Right-click root → Properties → Add UPN suffix*.

![Add UPN Suffix](../images/part-2/22-upn-suffix-add.png)

**Step 2: Change User UPN**

Open *AD Users and Computers → User Properties → Account tab → Change UPN suffix*.

![User UPN Change](../images/part-2/23-user-upn-change.png)

**Step 3: Force Delta Sync**

Execute PowerShell command to trigger immediate synchronization:

```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```

![Force Sync PowerShell](../images/part-2/24-force-sync-powershell.png)

**Step 4: Verify Synchronization**

Check *Entra admin center → Users* and verify the *On-premises sync* column shows *Yes*.

![On-Prem Sync Verified](../images/part-2/25-onprem-sync-verified.png)

### Verification Checklist

| User | Source | On-premises Sync |
| --- | --- | --- |
| `rootadmin1` | Windows Server AD | ✅ Yes |
| `entrauser1` | Microsoft Entra ID | No |
| `globaladmin` | Microsoft Entra ID | No |

### Outcome

- UPN suffix `@zerotrustlabs.it.com` added to AD forest
- User accounts updated with cloud-matching UPN
- Delta sync executed successfully
- Hybrid users visible in Entra with sync status confirmed

### Key Takeaways

- UPN matching is critical for seamless single sign-on
- Delta sync provides on-demand synchronization (vs. waiting 30 minutes)
- *On-premises sync = Yes* confirms successful hybrid identity

---

## Section 2.6: Hybrid Device Registration

### Overview

Register Windows client devices with both on-premises AD and Entra ID, enabling hybrid device management scenarios.

### Steps & Configuration

**Step 1: Verify Domain Membership**

![Domain Membership Verified](../images/part-2/26-domain-membership-verified.png)

**Step 2: Add Work Account**

*Settings → Accounts → Access work or school → Connect.*

![Work Account Setup](../images/part-2/27-work-account-setup.png)

![Work Account Prompt](../images/part-2/28-work-account-prompt.png)

**Step 3: Test Hybrid User Authentication**

Add synced user's work account to device.

![Account Added](../images/part-2/29-account-added.png)

![Hybrid Device State](../images/part-2/30-hybrid-device-state.png)

**Step 4: Verify Hybrid State**

![Hybrid Device in Azure](../images/part-2/31-hybrid-device-azure.png)

![Hybrid Device Detail](../images/part-2/32-hybrid-device-detail.png)

### Device Registration Summary

| Join Type | Directory | Result |
| --- | --- | --- |
| Domain Join | `zerotrustlabs.local` | ✅ Computer object in AD |
| Entra Registered | `zerotrustlabs.it.com` | ✅ Device in Entra ID |

### Outcome

- Windows client successfully domain-joined to on-premises AD
- Work account added for Entra registration
- Both cloud and synced users can authenticate on device
- Hybrid device state achieved

### Key Takeaways

- *Entra Registered* differs from *Hybrid Azure AD Join* (full join requires Entra Connect device writeback)
- Work accounts enable SSO to cloud applications
- Hybrid state provides flexibility for migration scenarios
- Device registration is the foundation for Conditional Access policies

---

**Previous:** [Part 1 — On-Premises Foundation](01-on-premises-foundation.md)
**Next:** [Part 3 — SSO Integration](03-sso-integration.md) → federating Entra ID to Salesforce via SAML 2.0.
