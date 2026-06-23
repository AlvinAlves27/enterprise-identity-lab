# Part 3: SSO Integration

> **SAML 2.0 Federation with Salesforce** — implementing enterprise SSO with certificate-based trust.

## Objective

Implement SAML 2.0-based Single Sign-On between Microsoft Entra ID and Salesforce, demonstrating enterprise application federation, certificate-based trust, and seamless authentication flows.

## Technologies

- Microsoft Entra ID Enterprise Applications
- SAML 2.0 Protocol
- Salesforce Developer Edition
- X.509 Certificates
- Federation Metadata XML
- Claims / Attribute Mapping

## Architecture

![SSO Flow](../images/architecture/sso-flow.svg)

---

## Section 3.1: Enterprise Application Setup

### Overview

Add Salesforce from the Entra ID application gallery and configure user assignments with appropriate role mappings.

### Steps & Configuration

**Step 1: Add Enterprise Application**

Navigate to *Enterprise applications → New application → Browse Microsoft Entra App Gallery → search "Salesforce"*.

![Add Salesforce App from Gallery](../images/part-3/01-add-salesforce-app.png)

**Step 2: Assign User to Application**

Navigate to *Users and groups → Add user/group*.

![Assign User to Salesforce](../images/part-3/02-assign-user-to-salesforce.png)

**Step 3: Select Application Role**

![Select System Administrator](../images/part-3/03-select-system-administrator.png)

**Step 4: Confirm Assignment**

![User Assignment Confirmed](../images/part-3/04-user-assignment-confirmed.png)

### Application Assignment Summary

| User | Application | Role |
| --- | --- | --- |
| `rootadmin1@zerotrustlabs.it.com` | Salesforce | System Administrator |

### Outcome

- Salesforce enterprise application added from gallery
- User assignment configured with appropriate role
- Foundation established for SAML configuration

### Key Takeaways

- Gallery applications include pre-configured SAML templates
- User assignment controls who can access the application
- Role mapping can integrate with application-specific permissions

---

## Section 3.2: SAML Configuration (Entra ID Side)

### Overview

Configure SAML-based single sign-on in Microsoft Entra ID, establishing the Identity Provider (IdP) settings that Salesforce will trust.

### Steps & Configuration

**Step 1: Configure Basic SAML Settings**

Navigate to *Single sign-on → SAML → Edit Basic SAML Configuration*.

| Setting | Value |
| --- | --- |
| Identifier (Entity ID) | `https://<your-org>.develop.my.salesforce.com` |
| Reply URL (ACS) | `https://<your-org>.develop.my.salesforce.com` |
| Sign-on URL | `https://<your-org>.develop.my.salesforce.com` |

![Basic SAML Configuration](../images/part-3/05-basic-saml-config.png)

**Step 2: Review SAML Configuration Overview**

![SAML Overview](../images/part-3/06-saml-overview.png)

**Step 3: Download Federation Credentials**

Download both the Federation Metadata XML and Base64 Certificate.

![Download SAML Credentials](../images/part-3/07-download-saml-credentials.png)

### Downloaded Files

| File | Purpose |
| --- | --- |
| `Salesforce.xml` | Federation Metadata (complete IdP configuration) |
| `Salesforce.cer` | X.509 Certificate (signature verification) |

### Outcome

- SAML endpoints configured for Salesforce
- Federation metadata downloaded for import
- Signing certificate ready for trust establishment

### Key Takeaways

- Entity ID must exactly match Salesforce's expected identifier
- Federation Metadata XML contains all IdP configuration in one file
- Certificate enables Salesforce to verify SAML assertion signatures

---

## Section 3.3: Salesforce SSO Configuration

### Overview

Configure Salesforce as the Service Provider (SP) to trust Microsoft Entra ID as the Identity Provider, completing the federation trust.

### Steps & Configuration

**Step 1: Configure My Domain**

*Setup → My Domain → Verify domain deployment.*

![Salesforce My Domain](../images/part-3/08-salesforce-my-domain.png)

**Step 2: Enable SAML**

*Setup → Single Sign-On Settings → Edit → Enable SAML.*

![Salesforce SAML Enabled](../images/part-3/09-salesforce-saml-enabled.png)

**Step 3: Create SAML SSO Configuration**

*Setup → Single Sign-On Settings → New.*

| Setting | Value |
| --- | --- |
| Name | `sts` |
| Issuer | `https://sts.windows.net/<tenant-id>/` |
| Entity ID | `https://<your-org>.develop.my.salesforce.com` |
| Identity Provider Certificate | `Salesforce.cer` (uploaded) |
| SAML Identity Type | Federation ID |
| Service Provider Initiated Request Binding | HTTP Redirect |

![SAML SSO Config Edit Form](../images/part-3/10-saml-sso-config-edit.png)

![SAML SSO Config Saved](../images/part-3/11-saml-sso-config-saved.png)

**Step 4: Configure Authentication Settings**

*Setup → My Domain → Authentication Configuration → Edit.*

![Salesforce Authentication Configuration](../images/part-3/12-salesforce-auth-config.png)

### Outcome

- SAML SSO configuration created in Salesforce
- IdP certificate uploaded and trusted
- Authentication service enabled on My Domain

### Key Takeaways

- Issuer URL identifies the specific Entra tenant
- Certificate upload establishes cryptographic trust
- My Domain authentication configuration controls login options

---

## Section 3.4: User Provisioning & Attribute Mapping

### Overview

Create matching user accounts in Salesforce and configure attribute claims to ensure proper identity mapping during SSO.

### Steps & Configuration

**Step 1: Create Salesforce User**

*Setup → Users → New User.*

| Field | Value |
| --- | --- |
| First Name | Root |
| Last Name | Admin1 |
| Email | `rootadmin1@zerotrustlabs.it.com` |
| Username | `rootadmin1@zerotrustlabs.it.com` |
| Profile | System Administrator |
| Federation ID | `rootadmin1@zerotrustlabs.it.com` |

![Create Salesforce User](../images/part-3/13-create-salesforce-user.png)

![Salesforce User Detail](../images/part-3/14-salesforce-user-detail.png)

**Step 2: Verify Attribute Mapping in Entra**

*Enterprise Applications → Salesforce → Single sign-on → Attributes & Claims.*

![Attribute Mapping](../images/part-3/15-attribute-mapping.png)

### Claim Mapping Summary

| Claim | Source Attribute | Target |
| --- | --- | --- |
| NameID | `user.userprincipalname` | Salesforce Federation ID |

### Outcome

- Matching user created in Salesforce
- Federation ID set to match Entra UPN
- Claim mapping configured for identity correlation

### Key Takeaways

- Manual provisioning required in Salesforce Developer Edition
- Federation ID is the key linking field for SSO
- Production environments typically use SCIM for automated provisioning

---

## Section 3.5: SSO Testing & Validation

### Overview

Validate the complete SSO flow from initial access through authenticated session, confirming seamless cross-platform authentication.

### Steps & Configuration

**Step 1: Access Salesforce Login**

![Salesforce Login](../images/part-3/16-salesforce-login.png)

**Step 2: Authenticate with Entra**

Click *Log in with sts* → redirected to Microsoft login.

![SSO Pick Account](../images/part-3/17-sso-pick-account.png)

**Step 3: Verify SSO Success**

![SSO Success](../images/part-3/18-sso-success.png)

**Step 4: Optional — Streamlined Authentication**

Disable local login to enforce SSO-only access.

![Enforce SSO Only](../images/part-3/19-enforce-sso-only.png)

### SSO Flow Verification

| Step | Expected Result | Status |
| --- | --- | --- |
| Access Salesforce | Redirect to IdP | ✅ |
| Select Account | Pre-populated from Windows session | ✅ |
| SAML Assertion | Automatic redirect back | ✅ |
| Salesforce Access | Authenticated session | ✅ |

### Outcome

- Complete SSO flow validated end-to-end
- Seamless authentication via Windows-connected account
- No password entry required after initial Windows login
- Optional enforcement of SSO-only access configured

### Key Takeaways

- SSO eliminates password fatigue for end users
- Windows-connected accounts enable true single sign-on
- Disabling local login enforces centralized authentication policy
- SSO provides a single audit trail for security compliance

---

**Previous:** [Part 2 — Hybrid Identity Bridge](02-hybrid-identity-bridge.md)
**Back to:** [Repo Overview](../README.md)
