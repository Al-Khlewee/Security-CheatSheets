# 2026 HIPAA Compliance & Technical Safeguards Cheat Sheet

> **Definitive Guide for Covered Entities (CEs) and Business Associates (BAs)**  
> *Last Updated: January 2026 | Effective Compliance Deadlines: February 16, 2026*

---

## Table of Contents

1. [The Four Pillars of HIPAA](#1-the-four-pillars-of-hipaa)
2. [2026 Critical Deadlines & Privacy Updates](#2-2026-critical-deadlines--privacy-updates)
3. [Technical Safeguards (The "How-To")](#3-technical-safeguards-the-how-to)
4. [Administrative & Physical Safeguards](#4-administrative--physical-safeguards)
5. [Business Associate Agreements (BAAs) & Vendors](#5-business-associate-agreements-baas--vendors)
6. [Breach Notification Protocol](#6-breach-notification-protocol)
7. [Quick Reference Tables](#7-quick-reference-tables)

---

## 1. The Four Pillars of HIPAA

### 1.1 Core Rule Definitions

| **Rule** | **Citation** | **Primary Purpose** | **Applies To** |
|----------|--------------|---------------------|----------------|
| **Privacy Rule** | 45 CFR Part 160 & 164 (Subparts A & E) | Governs the **use and disclosure** of Protected Health Information (PHI); establishes patient rights | CEs and BAs (via BAA) |
| **Security Rule** | 45 CFR Part 164 (Subpart C) | Mandates **administrative, physical, and technical safeguards** for electronic PHI (ePHI) | CEs and BAs |
| **Breach Notification Rule** | 45 CFR §§164.400-414 | Establishes **notification requirements** when unsecured PHI is compromised | CEs and BAs |
| **Omnibus Rule** (2013) | 78 FR 5566 | **Strengthened enforcement**, extended liability to BAs, modified breach definition to "harm threshold" removal | CEs and BAs |

---

### 1.2 Required vs. Addressable Implementation Specifications

The Security Rule distinguishes between two types of implementation specifications:

| **Specification Type** | **Definition** | **Compliance Obligation** |
|------------------------|----------------|---------------------------|
| **Required (R)** | Must be implemented exactly as specified | No flexibility; mandatory implementation |
| **Addressable (A)** | Must assess whether "reasonable and appropriate" | Implement as-is, implement equivalent alternative, OR document why not applicable |

#### ⚠️ 2026 Regulatory Trend: Addressable → Required

The **HHS Office for Civil Rights (OCR)** and proposed HIPAA Security Rule updates signal a significant shift:

```
┌─────────────────────────────────────────────────────────────────────────┐
│  CRITICAL CHANGE: Most "Addressable" specifications are being          │
│  reclassified as "Required" in 2026 rulemaking proposals.              │
│                                                                         │
│  Examples now trending toward REQUIRED status:                          │
│  • Encryption of ePHI at rest and in transit                           │
│  • Multi-Factor Authentication (MFA)                                    │
│  • Network segmentation                                                 │
│  • Penetration testing and vulnerability scanning                       │
└─────────────────────────────────────────────────────────────────────────┘
```

> **Compliance Alert: Section 1**  
> 🚨 **Common 2026 Audit Failure:** Organizations still treating "Addressable" as "Optional." OCR auditors are rejecting boilerplate "not applicable" documentation without substantive risk-based justification. Expect penalties for entities that failed to implement encryption or MFA claiming these were "not reasonable."

---

## 2. 2026 Critical Deadlines & Privacy Updates

### 2.1 February 16, 2026 Deadline: Notice of Privacy Practices (NPP)

**Regulatory Basis:** 45 CFR §164.520 (as amended by the 2024 Final Rule)

All Covered Entities **MUST** update their Notice of Privacy Practices by **February 16, 2026** to include:

| **Required NPP Update** | **Description** | **Implementation Guidance** |
|-------------------------|-----------------|----------------------------|
| **Reproductive Health Care PHI Protections** | Explicit statement that PHI related to lawful reproductive health care will NOT be disclosed for investigation/prosecution purposes | Add new section titled "Your Rights Regarding Reproductive Health Information" |
| **Right to Access in Electronic Format** | Clarify patient's right to receive ePHI in electronic format of their choice (if readily producible) | Specify supported formats (PDF, XML, FHIR, etc.) |
| **Fee Limitations** | Updated disclosure of fee structure for PHI access (limited to cost-based fees) | Remove references to "reasonable, cost-based" without specifics; provide actual fee schedule |
| **Part 2 Alignment Statement** | For entities handling Substance Use Disorder (SUD) records, integration notice required | Cross-reference 42 CFR Part 2 protections |

#### NPP Distribution Requirements (2026)

```
Distribution Method              Requirement
─────────────────────────────────────────────────────────────────
Website Posting                  REQUIRED - Prominent homepage link
First Service Encounter          REQUIRED - Provide paper/electronic copy
Good Faith Acknowledgment        REQUIRED - Document attempt to obtain signature
Material Revision Notice         REQUIRED within 60 days of change
Email Distribution               PERMITTED with prior patient consent
```

---

### 2.2 Reproductive Health Care PHI Protections

**Effective Date:** December 23, 2024 (Full Compliance: February 16, 2026)

The **HIPAA Privacy Rule to Support Reproductive Health Care Privacy** establishes:

| **Protection Category** | **Prohibited Disclosure Scenario** | **Exception** |
|------------------------|-----------------------------------|---------------|
| **Reproductive Health Care** | Disclosure to investigate/prosecute patient seeking lawful care | Court order from court of competent jurisdiction with specific findings |
| **Reproductive Health Care** | Disclosure to identify persons involved in providing lawful care | Subpoena alone is INSUFFICIENT |
| **Reproductive Health Care** | Disclosure for civil/criminal liability related to lawful reproductive care | Written attestation required from requestor |

#### Attestation Requirement (New in 2026)

For any request for PHI potentially related to reproductive health care, entities **MUST**:

1. **Obtain written attestation** that the request is NOT for prohibited purposes
2. **Verify attestation validity** (not facially defective)
3. **Document the attestation** in the designated record set
4. **Retain for 6 years** per standard HIPAA retention requirements

---

### 2.3 Substance Use Disorder (SUD) Records: Part 2 Alignment

**Regulatory Basis:** 42 CFR Part 2 (as amended, effective February 16, 2026)

The **Confidentiality of Substance Use Disorder Patient Records** rule aligns Part 2 with HIPAA:

| **Aspect** | **Pre-2026 (Part 2 Standalone)** | **2026 Aligned Framework** |
|------------|----------------------------------|---------------------------|
| **Consent Scope** | Narrow, program-specific | Single HIPAA-compliant consent for TPO |
| **Redisclosure** | Absolute prohibition with notice | Aligned with HIPAA minimum necessary |
| **Breach Notification** | No federal requirement | HIPAA Breach Notification Rule applies |
| **Enforcement** | DOJ referral only | OCR direct enforcement authority |
| **Patient Rights** | Limited access rights | Full HIPAA access, amendment, accounting rights |

> **Compliance Alert: Section 2**  
> 🚨 **Common 2026 Audit Failure:** NPPs not updated by February 16, 2026 deadline. OCR is prioritizing desk audits of NPP compliance. Failure to include reproductive health care attestation procedures is a **per-violation penalty exposure** (up to $68,928 per violation, $2,067,813 annual cap for willful neglect uncorrected).

---

## 3. Technical Safeguards (The "How-To")

**Regulatory Basis:** 45 CFR §164.312

### 3.1 Access Controls Checklist

**Standard:** §164.312(a)(1) – Implement technical policies to allow access only to authorized persons/software

| **Specification** | **Type** | **2026 Requirement** | **Implementation Checklist** |
|-------------------|----------|---------------------|------------------------------|
| **Unique User Identification** | Required | ✅ Mandatory | ☐ Assign unique ID to each workforce member<br>☐ Prohibit shared/generic accounts<br>☐ Integrate with identity provider (IdP)<br>☐ Document ID assignment/revocation procedures |
| **Emergency Access Procedure** | Required | ✅ Mandatory | ☐ Define "break-glass" scenarios<br>☐ Implement emergency access accounts (monitored)<br>☐ Require post-incident justification documentation<br>☐ Test annually during DR exercises |
| **Automatic Logoff** | Addressable→**Required** | ✅ Trending Mandatory | ☐ 15-minute inactivity timeout (recommended)<br>☐ Screen lock on all workstations<br>☐ Session termination for web applications<br>☐ Document exception justifications |
| **Encryption and Decryption** | Addressable→**Required** | ✅ Trending Mandatory | ☐ Encrypt all ePHI at rest<br>☐ Encrypt all ePHI in transit<br>☐ Key management procedures documented<br>☐ Hardware Security Module (HSM) for key storage |

#### Multi-Factor Authentication (MFA) Requirements

**2026 Standard:** MFA is now effectively **REQUIRED** for all remote access and privileged accounts

```
┌──────────────────────────────────────────────────────────────────────────┐
│  MFA IMPLEMENTATION HIERARCHY (Strongest to Acceptable)                  │
├──────────────────────────────────────────────────────────────────────────┤
│  1. FIDO2/WebAuthn Hardware Keys (e.g., YubiKey)         ★★★★★          │
│  2. Platform Authenticators (Windows Hello, Face ID)      ★★★★☆          │
│  3. TOTP Authenticator Apps (Microsoft/Google Auth)       ★★★☆☆          │
│  4. Push Notifications (with number matching)             ★★★☆☆          │
│  5. SMS/Voice OTP                                         ★★☆☆☆          │
│     ⚠️ DISCOURAGED - vulnerable to SIM swap attacks                     │
└──────────────────────────────────────────────────────────────────────────┘
```

**MFA Checklist:**
- [ ] MFA enabled for all remote/VPN access
- [ ] MFA enabled for all cloud service access (EHR, email, file storage)
- [ ] MFA enabled for privileged/administrative accounts
- [ ] MFA enabled for access from new/unrecognized devices
- [ ] Phishing-resistant MFA for high-risk roles (IT admins, executives)
- [ ] MFA bypass procedures documented and restricted

---

### 3.2 Audit Controls and Integrity

**Standard:** §164.312(b) – Audit Controls | §164.312(c)(1) – Integrity

#### Audit Control Requirements

| **Requirement** | **Implementation Specification** | **Minimum Data Elements** |
|-----------------|----------------------------------|---------------------------|
| **Hardware Audit** | Log system-level access events | Timestamp, User ID, Device ID, Action, Success/Failure |
| **Software Audit** | Log application-level PHI access | Timestamp, User ID, Patient ID, Record Accessed, Action Type |
| **Procedural Audit** | Documented review process | Review frequency, reviewer role, escalation procedure |

#### Integrity Controls

| **Control Type** | **Implementation** | **Verification Method** |
|------------------|-------------------|------------------------|
| **Cryptographic Hashing** | SHA-256 minimum for all ePHI files | Hash comparison on retrieval |
| **Digital Signatures** | For clinical documents, prescriptions | Certificate validation |
| **Database Integrity** | Row-level checksums, transaction logs | Automated integrity monitoring |
| **Backup Verification** | Hash validation of backup data | Quarterly restoration testing |

**Proving Data Integrity (Audit Evidence):**

```
Evidence Chain for Data Integrity:
═══════════════════════════════════════════════════════════════════════════

1. CREATION         → Timestamp + User ID + Initial Hash Value
                          ↓
2. ACCESS LOG       → Every read/write with hash verification
                          ↓
3. MODIFICATION     → Version control + Delta + New Hash + User ID
                          ↓
4. TRANSMISSION     → TLS session log + Integrity check at endpoints
                          ↓
5. STORAGE          → At-rest encryption + Periodic hash verification
                          ↓
6. RETENTION        → Immutable audit log (WORM storage recommended)
```

---

### 3.3 Encryption Requirements

**Standard:** §164.312(a)(2)(iv) and §164.312(e)(2)(ii)

#### Encryption at Rest (Data Storage)

| **Algorithm** | **Key Length** | **2026 Status** | **Use Case** |
|---------------|----------------|-----------------|--------------|
| **AES-256-GCM** | 256-bit | ✅ **REQUIRED** | Primary standard for all ePHI storage |
| **AES-256-CBC** | 256-bit | ✅ Acceptable | Legacy systems (migrate to GCM) |
| **AES-128** | 128-bit | ⚠️ Discouraged | Not recommended for new implementations |
| **3DES** | 168-bit | ❌ **PROHIBITED** | Deprecated; must migrate immediately |

**At-Rest Encryption Checklist:**
- [ ] Full-disk encryption (FDE) on all endpoints (BitLocker, FileVault)
- [ ] Database encryption (TDE for SQL Server, Oracle; native for cloud DBs)
- [ ] File-level encryption for network shares containing ePHI
- [ ] Backup encryption with separate key management
- [ ] Mobile device encryption (MDM-enforced)
- [ ] Removable media encryption (USB drives, external HDDs)

#### Encryption in Transit (Data Transmission)

| **Protocol** | **Version** | **2026 Status** | **Configuration Requirements** |
|--------------|-------------|-----------------|-------------------------------|
| **TLS** | 1.3 | ✅ **REQUIRED** | Preferred for all new implementations |
| **TLS** | 1.2 | ✅ Acceptable | Only with strong cipher suites (see below) |
| **TLS** | 1.0/1.1 | ❌ **PROHIBITED** | Must be disabled; known vulnerabilities |
| **SSL** | All versions | ❌ **PROHIBITED** | Must be disabled; critically vulnerable |

**Required TLS 1.2 Cipher Suites (if TLS 1.3 not available):**

```
Acceptable Cipher Suites (Priority Order):
──────────────────────────────────────────────────────────────────────────
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256

PROHIBITED Cipher Suites (Must Disable):
──────────────────────────────────────────────────────────────────────────
Any cipher containing: RC4, DES, 3DES, MD5, NULL, EXPORT, anon
```

**In-Transit Encryption Checklist:**
- [ ] TLS 1.3 enabled on all web servers and APIs
- [ ] TLS 1.0/1.1 disabled across all systems
- [ ] HSTS (HTTP Strict Transport Security) enabled
- [ ] Certificate management process documented
- [ ] Certificate expiration monitoring automated
- [ ] VPN with strong encryption for remote access (IPsec/IKEv2 or WireGuard)
- [ ] Email encryption for PHI (TLS, S/MIME, or secure portal)

> **Compliance Alert: Section 3**  
> 🚨 **Common 2026 Audit Failure:** (1) MFA not implemented for remote access citing "addressable" status—OCR rejects this in nearly all cases. (2) TLS 1.0/1.1 still enabled on legacy systems. (3) Audit logs exist but lack retention policy (minimum 6 years required) or lack documented review procedures.

---

## 4. Administrative & Physical Safeguards

### 4.1 Risk Analysis (Security Risk Assessment - SRA)

**Regulatory Basis:** 45 CFR §164.308(a)(1)(ii)(A) – **REQUIRED**

The SRA is the **foundational requirement** of HIPAA Security Rule compliance.

#### SRA Process Framework

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    NIST-BASED SRA METHODOLOGY                           │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  STEP 1: SCOPE DEFINITION                                               │
│  ├── Identify all ePHI repositories (EHR, databases, file shares)      │
│  ├── Map data flows (creation → storage → transmission → disposal)     │
│  └── Inventory all systems/applications touching ePHI                  │
│                                                                         │
│  STEP 2: THREAT IDENTIFICATION                                          │
│  ├── Natural threats (fire, flood, power failure)                      │
│  ├── Human threats (malicious insider, external attacker, error)       │
│  └── Environmental threats (HVAC failure, contamination)               │
│                                                                         │
│  STEP 3: VULNERABILITY IDENTIFICATION                                   │
│  ├── Technical vulnerabilities (unpatched systems, misconfigurations)  │
│  ├── Administrative vulnerabilities (lack of training, policies)       │
│  └── Physical vulnerabilities (access controls, media disposal)        │
│                                                                         │
│  STEP 4: CONTROL ANALYSIS                                               │
│  ├── Document existing controls                                         │
│  └── Evaluate control effectiveness                                     │
│                                                                         │
│  STEP 5: LIKELIHOOD DETERMINATION                                       │
│  └── Probability rating: High / Medium / Low                           │
│                                                                         │
│  STEP 6: IMPACT ANALYSIS                                                │
│  └── Impact rating: High / Medium / Low (based on CIA triad)           │
│                                                                         │
│  STEP 7: RISK DETERMINATION                                             │
│  └── Risk Level = Likelihood × Impact                                  │
│                                                                         │
│  STEP 8: CONTROL RECOMMENDATIONS                                        │
│  └── Prioritized remediation plan with timelines                       │
│                                                                         │
│  STEP 9: DOCUMENTATION                                                  │
│  └── Complete written SRA report with management sign-off              │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

#### SRA Frequency Requirements

| **Trigger Event** | **SRA Requirement** |
|-------------------|---------------------|
| Initial HIPAA compliance | Full comprehensive SRA |
| Annual review | Update/validate existing SRA |
| Significant system change | Targeted SRA for affected systems |
| Security incident | Post-incident SRA review |
| Merger/acquisition | Full SRA of acquired entities |
| New technology deployment | Pre-implementation risk assessment |

---

### 4.2 Recognized Security Practices (RSP) Safe Harbor

**Regulatory Basis:** HITECH Act §13412 (as amended 2021)

**Critical for 2026:** OCR **MUST** consider an entity's adoption of "Recognized Security Practices" when determining:

- Fines and penalties
- Audit scope and duration
- Remediation timelines
- Resolution agreement terms

#### Qualifying Recognized Security Practices

| **Framework** | **Description** | **Documentation Required** |
|---------------|-----------------|---------------------------|
| **NIST Cybersecurity Framework (CSF 2.0)** | Comprehensive risk-based framework | CSF profile mapping, maturity assessment |
| **NIST SP 800-171** | Protecting CUI (applicable to hybrid environments) | System Security Plan (SSP), POA&M |
| **HIPAA Security Rule (strict implementation)** | Full addressable specification implementation | SRA, policies, evidence of implementation |
| **SOC 2 Type II** | Third-party assurance report | Current SOC 2 report (within 12 months) |
| **HITRUST CSF Certification** | Healthcare-specific comprehensive framework | Valid HITRUST certification |

**Safe Harbor Requirements:**
1. **Actively in use** for at least 12 months prior to incident
2. **Consistently applied** across the organization
3. **Documented and demonstrable** to OCR auditors
4. **Covers the systems/data involved** in the incident

---

### 4.3 Physical Safeguards

**Regulatory Basis:** 45 CFR §164.310

#### Workstation Security

| **Requirement** | **Type** | **Implementation Specification** |
|-----------------|----------|----------------------------------|
| **Workstation Use** | Required | ☐ Define appropriate workstation functions<br>☐ Restrict ePHI access by role<br>☐ Prohibit personal use policies |
| **Workstation Security** | Required | ☐ Physical access controls to workstations<br>☐ Screen positioning (prevent shoulder surfing)<br>☐ Privacy screens on monitors<br>☐ Clean desk policy enforcement |

#### Media Disposal

| **Media Type** | **Disposal Method** | **Documentation Required** |
|----------------|--------------------|-----------------------------|
| **Paper/Film** | Cross-cut shredding (≥ P-4) or incineration | Certificate of destruction |
| **Hard Drives (HDD)** | Degaussing + physical destruction OR NIST 800-88 Clear/Purge | Destruction certificate with serial numbers |
| **Solid State Drives (SSD)** | Cryptographic erase + physical destruction (shredding) | Destruction certificate with serial numbers |
| **Mobile Devices** | Factory reset + physical destruction | Destruction certificate with IMEI/serial |
| **Backup Tapes** | Degaussing + physical destruction | Destruction certificate |
| **Optical Media (CD/DVD)** | Shredding or incineration | Certificate of destruction |

**Disposal Chain of Custody:**
```
Creation of Disposal Record → Secure Storage Pending Disposal → 
Transfer to Destruction Vendor (if applicable) → Witnessed Destruction → 
Certificate of Destruction → Retention of Certificate (6 years)
```

#### Facility Access Controls

| **Specification** | **Type** | **Implementation Requirements** |
|-------------------|----------|--------------------------------|
| **Contingency Operations** | Addressable | Procedures for physical access during disaster recovery |
| **Facility Security Plan** | Addressable→**Trending Required** | Documented physical security controls |
| **Access Control & Validation** | Addressable→**Trending Required** | Visitor management, badge access, access logs |
| **Maintenance Records** | Addressable | Log of physical modifications to facility |

**Facility Access Checklist:**
- [ ] Badge/key card access to areas containing ePHI
- [ ] Visitor sign-in/escort requirements
- [ ] Security cameras at entry points and server rooms
- [ ] Server room access restricted to authorized personnel
- [ ] Environmental controls (HVAC, fire suppression) for data centers
- [ ] Alarm systems with monitoring

> **Compliance Alert: Section 4**  
> 🚨 **Common 2026 Audit Failure:** (1) SRA not updated annually or after significant changes—#1 cited deficiency in OCR audits. (2) No documented RSP program to invoke Safe Harbor. (3) Media disposal certificates missing or incomplete (lacking serial numbers). (4) Server room access not logged or reviewed.

---

## 5. Business Associate Agreements (BAAs) & Vendors

### 5.1 Modern BAA Checklist (2026 Requirements)

**Regulatory Basis:** 45 CFR §164.502(e) and §164.504(e)

#### 5-Point BAA Compliance Checklist

| **#** | **Required Element** | **2026 Specifics** | **Contract Language Guidance** |
|-------|---------------------|-------------------|-------------------------------|
| **1** | **Permitted Uses and Disclosures** | Explicitly define scope; prohibit reproductive health care PHI disclosure for investigations | "BA shall not use or disclose PHI for any purpose other than as expressly permitted by this Agreement or as Required by Law, excluding prohibited disclosures under 45 CFR 164.502(a)(5)(iii)." |
| **2** | **Safeguards Requirement** | Reference specific Security Rule requirements; mandate encryption, MFA, vulnerability management | "BA shall implement administrative, physical, and technical safeguards including, at minimum: AES-256 encryption at rest, TLS 1.3 in transit, and MFA for all remote access." |
| **3** | **Breach Notification & Incident Reporting** | Include 24-hour notification requirement; define "security incident" reporting | "BA shall notify CE of any Security Incident within 24 hours of discovery and any Breach within 24 hours of discovery, prior to the 60-day statutory deadline." |
| **4** | **Subcontractor Flow-Down** | Require equivalent BAAs with all subcontractors | "BA shall ensure any subcontractor that creates, receives, maintains, or transmits PHI agrees to the same restrictions and conditions as contained in this Agreement." |
| **5** | **Return/Destruction of PHI** | Specify destruction standards and certification requirements | "Upon termination, BA shall return or destroy all PHI per NIST SP 800-88 standards and provide written certification of destruction within 30 days." |

#### Additional 2026 BAA Provisions (Recommended)

| **Provision** | **Purpose** | **Sample Language** |
|---------------|-------------|---------------------|
| **Right to Audit** | Verify BA compliance | "CE reserves the right to audit BA's security practices upon reasonable notice." |
| **Insurance Requirements** | Ensure financial responsibility | "BA shall maintain cyber liability insurance with minimum coverage of $X million." |
| **Security Attestation** | Annual compliance verification | "BA shall provide annual written attestation of HIPAA Security Rule compliance." |
| **Penetration Testing** | Verify security controls | "BA shall conduct annual penetration testing and provide summary results to CE." |
| **Incident Response Plan** | Ensure preparedness | "BA shall maintain and test an Incident Response Plan at least annually." |

---

### 5.2 2026 BA Expectations: Incident Reporting Timelines

**Regulatory Trend:** OCR enforcement actions and settlements indicate expectation of **24-hour notification** from BA to CE for security incidents and breaches.

| **Event Type** | **Statutory Requirement** | **2026 Best Practice/Trend** |
|----------------|--------------------------|------------------------------|
| **Security Incident** | No specific timeline in regulation | **24 hours** from discovery |
| **Breach (Confirmed)** | 60 days to individuals; "without unreasonable delay" to CE | **24 hours** to CE from discovery |
| **Breach (Suspected)** | N/A | **Immediate** notification of investigation |

**BA Incident Response Obligations:**

```
TIMELINE: BA INCIDENT DISCOVERY → CE NOTIFICATION
═══════════════════════════════════════════════════════════════════════════

Hour 0     │ Incident/Breach Discovery
           ↓
Hour 1-4   │ Initial Triage & Containment
           ↓
Hour 4-12  │ Preliminary Impact Assessment (PHI involved? How many records?)
           ↓
Hour 12-24 │ NOTIFY CE (with available information)
           │ ├── Nature of incident
           │ ├── PHI potentially affected
           │ ├── Containment status
           │ └── Preliminary timeline
           ↓
Day 1-10   │ Forensic Investigation
           ↓
Day 10-30  │ Complete Breach Assessment
           ↓
Day 30-60  │ Support CE with individual notifications (if breach confirmed)
```

---

### 5.3 CE vs. BA Responsibilities Comparison

| **Responsibility** | **Covered Entity (CE)** | **Business Associate (BA)** |
|--------------------|------------------------|----------------------------|
| **BAA Execution** | Must have BAA with all BAs | Must execute BAA before receiving PHI |
| **Privacy Rule Compliance** | Full compliance required | Compliance via BAA terms |
| **Security Rule Compliance** | Full compliance required | **Direct compliance required** (since 2013) |
| **Breach Notification to Individuals** | CE responsibility | Notify CE; may assist with individual notice |
| **Breach Notification to HHS** | CE responsibility | Report to CE |
| **Breach Notification to Media** | CE responsibility (if >500) | Report to CE |
| **Risk Analysis** | Required for own environment | Required for own environment |
| **Training** | Required for workforce | Required for workforce |
| **Documentation Retention** | 6 years | 6 years |

> **Compliance Alert: Section 5**  
> 🚨 **Common 2026 Audit Failure:** (1) BAAs older than 2021 missing subcontractor flow-down provisions. (2) BAAs lacking specific security requirements (encryption standards, MFA). (3) No documented process for BA oversight/due diligence. (4) Incident notification provisions still at 60 days rather than contractual 24-hour requirement.

---

## 6. Breach Notification Protocol

### 6.1 Definitions: Breach vs. Security Incident

**Critical Distinction:** Not every security incident is a breach, but every breach is a security incident.

| **Term** | **Definition (45 CFR §164.402)** | **Example** |
|----------|----------------------------------|-------------|
| **Security Incident** | Attempted or successful unauthorized access, use, disclosure, modification, or destruction of ePHI OR interference with system operations | Failed login attempts, malware detection, phishing attempt blocked |
| **Breach** | Acquisition, access, use, or disclosure of unsecured PHI in violation of the Privacy Rule that **compromises the security or privacy** of the PHI | Ransomware encrypting patient database, laptop stolen with unencrypted ePHI, employee snooping |

#### Breach Presumption & Exceptions

**Default Position:** An impermissible use or disclosure of PHI is **presumed to be a breach** unless the CE/BA demonstrates low probability of compromise.

**Four-Factor Risk Assessment:**

```
To overcome breach presumption, assess:
───────────────────────────────────────────────────────────────────────────
1. NATURE OF PHI     │ What identifiers? Clinical information? Financial?
                     │ (More sensitive = higher risk)
───────────────────────────────────────────────────────────────────────────
2. UNAUTHORIZED      │ Who received/accessed the PHI? 
   RECIPIENT         │ (Another CE = lower risk; public internet = higher risk)
───────────────────────────────────────────────────────────────────────────
3. ACTUAL            │ Was PHI actually acquired or viewed?
   ACQUISITION       │ (Returned unopened = lower risk)
───────────────────────────────────────────────────────────────────────────
4. EXTENT OF         │ What steps mitigated the risk?
   MITIGATION        │ (Immediate retrieval, recipient attestation)
───────────────────────────────────────────────────────────────────────────
```

**Three Statutory Exceptions (Not a Breach):**

| **Exception** | **Description** | **Documentation Required** |
|---------------|-----------------|---------------------------|
| **Unintentional Acquisition** | Workforce member acting in good faith, within scope of authority, no further use/disclosure | Document circumstances, remediation |
| **Inadvertent Disclosure** | Disclosure to authorized workforce member, no further use/disclosure | Document circumstances |
| **Good Faith Belief** | Recipient could not reasonably retain the PHI | Document basis for belief |

---

### 6.2 Breach Notification Timelines

**Regulatory Basis:** 45 CFR §§164.404-408

| **Notification Recipient** | **Trigger** | **Timeline** | **Method** |
|---------------------------|-------------|--------------|------------|
| **Individuals** | Any breach of unsecured PHI | **Within 60 days** of discovery | First-class mail (or email if consented); substitute notice if contact info insufficient |
| **HHS Secretary (OCR)** | Breach affecting <500 individuals | **Within 60 days of end of calendar year** | HHS Breach Portal (annual log) |
| **HHS Secretary (OCR)** | Breach affecting ≥500 individuals | **Within 60 days** of discovery (contemporaneous) | HHS Breach Portal |
| **Media** | Breach affecting ≥500 individuals in single state/jurisdiction | **Within 60 days** of discovery | Prominent media outlets in affected area |

#### Breach Notification Content Requirements

**Individual Notice Must Include:**

```
┌─────────────────────────────────────────────────────────────────────────┐
│  REQUIRED BREACH NOTIFICATION ELEMENTS                                  │
├─────────────────────────────────────────────────────────────────────────┤
│  1. Brief description of WHAT HAPPENED (including date of breach and   │
│     date of discovery)                                                  │
│                                                                         │
│  2. Types of PHI INVOLVED (e.g., name, SSN, diagnosis, treatment)      │
│                                                                         │
│  3. Steps INDIVIDUALS SHOULD TAKE to protect themselves                │
│     (e.g., credit monitoring, password changes)                        │
│                                                                         │
│  4. Brief description of what the ENTITY IS DOING to investigate,      │
│     mitigate harm, and prevent future breaches                         │
│                                                                         │
│  5. CONTACT PROCEDURES including toll-free number, email, postal       │
│     address, and website for additional information                    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

#### Breach Timeline Visualization

```
DAY 0                 DAY 1-30              DAY 31-60             YEAR END
  │                      │                      │                    │
  ▼                      ▼                      ▼                    ▼
DISCOVERY ──────> INVESTIGATION ──────> NOTIFICATION ──────> HHS LOG
  │               & RISK ASSESSMENT       (if breach)        (if <500)
  │                      │                      │
  │                      │                      ├──> Individuals
  │                      │                      ├──> HHS (if ≥500)
  │                      │                      └──> Media (if ≥500 in state)
  │                      │
  └── 24 hrs ──> BA notifies CE (best practice)
```

---

### 6.3 Breach Documentation Requirements

| **Documentation Element** | **Retention Period** | **Content** |
|--------------------------|---------------------|-------------|
| **Risk Assessment** | 6 years | Four-factor analysis for all impermissible disclosures |
| **Notification Letters** | 6 years | Copies of all individual notifications |
| **Breach Log** | 6 years | All breaches, including those <500 |
| **Investigation Report** | 6 years | Findings, root cause, remediation |
| **Substitute Notice Records** | 6 years | Evidence of media/web posting if applicable |

> **Compliance Alert: Section 6**  
> 🚨 **Common 2026 Audit Failure:** (1) Risk assessment not conducted for every impermissible disclosure—OCR requires documented four-factor analysis even if determination is "not a breach." (2) Breach log not maintained for incidents <500. (3) Notification letters missing required elements. (4) Failure to notify HHS within 60 days for large breaches (contemporaneous requirement misunderstood).

---

## 7. Quick Reference Tables

### Privacy Rule vs. Security Rule Comparison

| **Aspect** | **Privacy Rule** | **Security Rule** |
|------------|------------------|-------------------|
| **Scope** | All PHI (paper, oral, electronic) | ePHI only |
| **Focus** | Use and disclosure permissions | Protection and safeguards |
| **Primary Standards** | Minimum necessary, individual rights, authorizations | Administrative, physical, technical safeguards |
| **Flexibility** | Specific use/disclosure rules | Risk-based "reasonable and appropriate" |
| **Patient Rights** | Access, amendment, restriction, accounting, confidential communications | N/A (supported by technical controls) |
| **Enforcement** | OCR (civil), DOJ (criminal) | OCR (civil), DOJ (criminal) |

### Covered Entity vs. Business Associate Obligations

| **Obligation** | **Covered Entity** | **Business Associate** |
|----------------|-------------------|------------------------|
| **Privacy Rule** | Full compliance | Via BAA terms |
| **Security Rule** | Full compliance | **Full compliance** |
| **Breach Notification (to individuals)** | ✅ Responsible | ❌ (Notify CE only) |
| **Breach Notification (to HHS)** | ✅ Responsible | ❌ (Notify CE only) |
| **BAA Required** | With all BAs | With all subcontractors |
| **Risk Analysis** | ✅ Required | ✅ Required |
| **Policies & Procedures** | ✅ Required | ✅ Required |
| **Training** | ✅ Required | ✅ Required |
| **Documentation Retention** | 6 years | 6 years |

### Penalty Tiers (2026 Adjusted for Inflation)

| **Tier** | **Culpability** | **Penalty Range per Violation** | **Annual Cap** |
|----------|-----------------|--------------------------------|----------------|
| **Tier 1** | Lack of Knowledge | $137 – $68,928 | $2,067,813 |
| **Tier 2** | Reasonable Cause | $1,379 – $68,928 | $2,067,813 |
| **Tier 3** | Willful Neglect (Corrected) | $13,785 – $68,928 | $2,067,813 |
| **Tier 4** | Willful Neglect (Not Corrected) | $68,928 – $2,067,813 | $2,067,813 |

*Note: Penalty amounts adjusted annually for inflation. Criminal penalties (DOJ) may also apply.*

---

## Appendix A: 2026 Compliance Checklist Summary

### Immediate Actions (By February 16, 2026)

- [ ] Update Notice of Privacy Practices with reproductive health care protections
- [ ] Update NPP with Part 2 alignment statement (if applicable)
- [ ] Distribute updated NPP and update website
- [ ] Implement attestation procedures for reproductive health care PHI requests
- [ ] Review and update all BAAs for 24-hour notification requirement

### Ongoing Technical Requirements

- [ ] MFA implemented for all remote access and privileged accounts
- [ ] AES-256 encryption at rest for all ePHI
- [ ] TLS 1.3 (or TLS 1.2 with approved ciphers) for all ePHI transmission
- [ ] TLS 1.0/1.1 and SSL disabled across all systems
- [ ] Audit logging enabled with 6-year retention
- [ ] Annual Security Risk Assessment completed and documented

### Recognized Security Practices

- [ ] Select and implement qualifying RSP framework (NIST CSF, HITRUST, etc.)
- [ ] Document 12+ months of consistent RSP implementation
- [ ] Maintain evidence portfolio for Safe Harbor claims

---

## Appendix B: Key Regulatory References

| **Topic** | **Citation** | **URL** |
|-----------|--------------|---------|
| HIPAA Privacy Rule | 45 CFR Part 164 Subpart E | [ecfr.gov](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-E) |
| HIPAA Security Rule | 45 CFR Part 164 Subpart C | [ecfr.gov](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C) |
| Breach Notification Rule | 45 CFR Part 164 Subpart D | [ecfr.gov](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-D) |
| Part 2 (SUD Records) | 42 CFR Part 2 | [ecfr.gov](https://www.ecfr.gov/current/title-42/chapter-I/subchapter-A/part-2) |
| NIST CSF 2.0 | NIST | [nist.gov/cyberframework](https://www.nist.gov/cyberframework) |
| NIST SP 800-88 (Media Sanitization) | NIST | [nist.gov](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final) |
| OCR Breach Portal | HHS | [ocrportal.hhs.gov](https://ocrportal.hhs.gov/ocr/breach/breach_report.jsf) |
| OCR HIPAA FAQs | HHS | [hhs.gov/hipaa](https://www.hhs.gov/hipaa/for-professionals/faq/index.html) |

---

**Document Version:** 1.0  
**Effective Date:** January 2026  
**Next Review:** January 2027  
**Classification:** For CE/BA Compliance Use

---

*This cheat sheet is provided for informational purposes and does not constitute legal advice. Consult with qualified legal counsel and compliance professionals for organization-specific guidance.*
