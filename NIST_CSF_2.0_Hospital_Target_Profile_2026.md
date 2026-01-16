# NIST CSF 2.0 Target Profile: 500-Bed Acute Care Hospital

> **Document Classification:** Audit-Ready Compliance Artifact  
> **Organization Type:** 500-Bed Acute Care Hospital  
> **Framework Version:** NIST Cybersecurity Framework 2.0 (February 2024)  
> **Prepared By:** Healthcare Cybersecurity Consulting Team  
> **Date:** January 2026  
> **Compliance Alignment:** HIPAA Security Rule (45 CFR §164.312), NIST SP 800-66 Rev. 2

---

## Executive Summary

This Target Profile defines the desired cybersecurity posture for a 500-bed acute care hospital operating critical healthcare systems including Electronic Medical Records (EMR), Laboratory and Radiology Information Systems (LIS/RIS), Building Management Systems (BMS), Hospital/Oncology Information Systems (HIS/OIS), and Internet of Medical Things (IoMT) devices. The profile addresses the unique risk landscape of clinical environments where cybersecurity failures directly impact patient safety and care continuity.

> ⚠️ **CRITICAL DESIGN PRINCIPLE: AVAILABILITY FIRST**
>
> This Target Profile prioritizes **Availability** above Confidentiality and Integrity. In healthcare, system downtime correlates directly with patient mortality. Studies show a 20-35% increase in in-hospital mortality during ransomware-induced EHR outages. Every design decision in this profile must ask: *"How does this control maintain or restore clinical operations?"*

### Alignment with NIST IR 8374 (Ransomware Risk Management)

This Target Profile adapts the generic ransomware guidance from **NIST IR 8374** to the hospital environment. Key adaptations include:

| **IR 8374 Recommendation** | **Hospital-Specific Adaptation** |
|:---------------------------|:---------------------------------|
| Identify critical assets | Tier assets by patient safety impact, not just business value |
| Network segmentation | Clinical VLAN isolation preventing lateral movement from admin to surgical systems |
| Backup integrity | Immutable, air-gapped backups with clinical data reconciliation procedures |
| Incident response | Clinical downtime procedures enabling paper-based care continuity |
| Recovery prioritization | Life-critical IoMT → EMR → Lab/Radiology → Pharmacy → Administrative |

### Special Considerations Addressed

This profile provides detailed guidance for three critical hospital-specific challenges:

1. **🔧 Legacy Equipment Management** — Securing unpatchable IoMT devices (Windows XP MRIs, legacy infusion pumps) through compensating controls
2. **🔒 Network Segmentation** — Preventing ransomware lateral movement from administrative areas (reception) to clinical areas (surgical wing)
3. **📋 Clinical Continuity** — Paper-based downtime procedures ensuring physicians can safely treat patients during system restoration

### Scope of Critical Systems

| **System Category** | **Representative Systems** | **PHI/Safety Impact** | **Risk Classification** |
|:--------------------|:---------------------------|:----------------------|:------------------------|
| **EMR** | Epic, Cerner | High PHI Volume; Clinical Decision Support | Critical |
| **LIS** | Laboratory Information System | Diagnostic Results; PHI | Critical |
| **RIS** | Radiology Information System (PACS Integration) | Imaging PHI; Diagnostic Data | Critical |
| **BMS** | Building Management (HVAC, Life Safety, Fire Suppression) | Patient Safety; Environmental Controls | Critical |
| **HIS** | Hospital Information System (ADT, Billing, Scheduling) | Administrative PHI; Revenue Cycle | High |
| **OIS** | Oncology Information System (Radiotherapy Planning) | Treatment Delivery; Patient Safety | Critical |
| **IoMT** | 5,000+ Medical Devices (Infusion Pumps, MRI, Ventilators, Monitors) | Direct Patient Care; Life-Critical | Critical |

### Implementation Tier Target

| **Current Assessment** | **Target Tier** | **Timeline** |
|:-----------------------|:----------------|:-------------|
| Tier 2: Risk-Informed | Tier 3: Repeatable | 18-24 Months |

---

## Function 1: GOVERN (GV)

### Purpose
Establish and monitor the organization's cybersecurity risk management strategy, expectations, and policy with specific emphasis on clinical operations and PHI protection.

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **GV.OC-01** | Organizational mission is understood and informs cybersecurity risk management | Hospital mission of patient care delivery informs all security decisions; PHI protection enables trust-based care relationships | High | HIPAA §164.308(a)(1)(i); NIST SP 800-66 §4.1 |
| **GV.OC-02** | Internal and external stakeholders are understood, and their needs and expectations regarding cybersecurity risk management are understood and considered | Stakeholder mapping includes patients, clinicians, Joint Commission, CMS, state health departments, cyber insurers, and medical device manufacturers | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **GV.OC-03** | Legal, regulatory, and contractual requirements regarding cybersecurity are understood and managed | HIPAA Security Rule, HITECH, state breach notification laws, FDA medical device guidance, and BAA obligations are inventoried and tracked | High | HIPAA §164.308(a)(1)(ii)(A); 45 CFR §164.306(a) |
| **GV.OC-04** | Critical objectives, capabilities, and services that stakeholders depend on are understood and communicated | EMR uptime for clinical decision-making, BMS for life safety, infusion pump availability for medication delivery are documented as critical dependencies | High | HIPAA §164.308(a)(7)(ii)(E); NIST SP 800-66 §4.14 |
| **GV.OC-05** | Outcomes, capabilities, and services that the organization depends on are understood and communicated | Dependencies on EHR vendors (Epic/Cerner), medical device OEMs, cloud services, and utility providers are documented with SLAs | Medium | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **GV.RM-01** | Risk management objectives are established and agreed upon by organizational stakeholders | Board-approved risk appetite statement addresses acceptable PHI exposure thresholds and clinical system downtime tolerances | High | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **GV.RM-02** | Risk appetite and risk tolerance statements are established, communicated, and maintained | Quantified risk tolerance: Zero tolerance for life-critical system failures; <4-hour RTO for EMR; defined PHI breach thresholds | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.1 |
| **GV.RM-03** | Cybersecurity risk management activities and outcomes are included in enterprise risk management processes | CISO reports to CRO/CFO with quarterly risk register updates to Board Audit Committee; cyber risk integrated into hospital ERM | High | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |
| **GV.RM-04** | Strategic direction that describes appropriate risk response options is established and communicated | Risk response matrix defines accept/mitigate/transfer/avoid criteria for clinical vs. administrative systems | Medium | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **GV.RR-01** | Organizational leadership is responsible and accountable for cybersecurity risk and fosters a culture of cybersecurity awareness | CEO/Board accountability documented; CISO has direct reporting line; security awareness embedded in clinical onboarding | High | HIPAA §164.308(a)(2); NIST SP 800-66 §4.4 |
| **GV.RR-02** | Roles, responsibilities, and authorities related to cybersecurity risk management are established, communicated, understood, and enforced | RACI matrix defines responsibilities for Security, IT, Clinical Engineering, Compliance, and department heads | High | HIPAA §164.308(a)(2); NIST SP 800-66 §4.4 |
| **GV.RR-03** | Adequate resources are allocated commensurate with cybersecurity risk strategy, roles and responsibilities, and policies | Security budget aligned to 6-8% of IT spend; dedicated clinical security FTEs for IoMT and BMS | Medium | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **GV.RR-04** | Cybersecurity is included in human resources practices | Background checks, security training requirements, and acceptable use acknowledgments are mandatory for all workforce with ePHI access | High | HIPAA §164.308(a)(3); NIST SP 800-66 §4.9 |
| **GV.PO-01** | Policy for managing cybersecurity risks is established based on organizational context, cybersecurity strategy, and priorities | Hospital-wide Information Security Policy approved by Board; clinical-specific addenda for EMR, BMS, and medical devices | High | HIPAA §164.316(a); NIST SP 800-66 §4.22 |
| **GV.PO-02** | Policy for managing cybersecurity risks is reviewed, updated, communicated, and enforced | Annual policy review cycle; policy acknowledgment tracking; sanctions for non-compliance documented and enforced | High | HIPAA §164.316(b)(2)(iii); NIST SP 800-66 §4.22 |
| **GV.OV-01** | Cybersecurity risk management strategy outcomes are reviewed to inform and adjust strategy and direction | Quarterly KRI/KPI review with executive leadership; post-incident strategy adjustments documented | Medium | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |
| **GV.OV-02** | The cybersecurity risk management strategy is reviewed and adjusted to ensure coverage of organizational requirements and risks | Annual strategy assessment aligned to threat landscape changes (ransomware trends, IoMT vulnerabilities) | Medium | HIPAA §164.308(a)(1)(ii)(D); NIST SP 800-66 §4.3 |
| **GV.OV-03** | Organizational cybersecurity risk management performance is evaluated and reviewed for adjustments needed | Metrics include: Mean Time to Detect (MTTD), patch compliance rates, phishing simulation results, audit findings closure rate | Medium | HIPAA §164.308(a)(1)(ii)(D); NIST SP 800-66 §4.3 |
| **GV.SC-01** | A cybersecurity supply chain risk management program, strategy, objectives, policies, and processes are established and agreed to by organizational stakeholders | C-SCRM program addresses medical device vendors, cloud EMR providers, HIE connections, and BMS contractors | High | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **GV.SC-02** | Cybersecurity roles and responsibilities for suppliers, customers, and partners are established, communicated, and coordinated internally and externally | BAAs executed with all vendors accessing PHI; security requirements in RFPs; vendor security scorecards maintained | High | HIPAA §164.308(b)(1); 45 CFR §164.314 |
| **GV.SC-03** | Cybersecurity supply chain risk management is integrated into cybersecurity and enterprise risk management | Medical device procurement includes security assessment; vendor risk tiers (Critical/High/Medium/Low) defined | High | HIPAA §164.308(b)(3); NIST SP 800-66 §4.19 |
| **GV.SC-04** | Suppliers are known and prioritized by criticality | Tier 1 vendors (Epic/Cerner, BMS provider, infusion pump OEM) receive enhanced oversight and quarterly reviews | High | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **GV.SC-05** | Requirements to address cybersecurity risks in supply chains are established, prioritized, and integrated into contracts and other agreements | Security exhibit in all vendor contracts; right-to-audit clauses; breach notification requirements; SLA for security patches | High | HIPAA §164.314(a)(2)(i); NIST SP 800-66 §4.20 |
| **GV.SC-06** | Planning and due diligence are performed to reduce risks before entering into supplier relationships | Pre-contract security assessments (SOC 2, HITRUST, penetration test results) required for PHI-accessing vendors | Medium | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **GV.SC-07** | The risks posed by a supplier, their products, and their services are understood, recorded, prioritized, assessed, responded to, and monitored over the relationship lifecycle | Continuous vendor monitoring via security ratings services; annual reassessment of critical vendors | Medium | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |

---

## Function 2: IDENTIFY (ID)

### Purpose
Develop organizational understanding of cybersecurity risk to systems, assets, data, and capabilities critical to hospital operations and patient care.

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **ID.AM-01** | Inventories of hardware managed by the organization are maintained | Complete inventory of servers, workstations, network devices, and 5,000+ IoMT devices including infusion pumps, ventilators, imaging equipment, and patient monitors | High | HIPAA §164.310(d)(1); NIST SP 800-66 §4.17 |
| **ID.AM-02** | Inventories of software, services, and systems managed by the organization are maintained | Software inventory including EMR modules, LIS/RIS applications, BMS controllers, medical device firmware versions, and licensed applications | High | HIPAA §164.310(d)(1); NIST SP 800-66 §4.17 |
| **ID.AM-03** | Representations of the organization's authorized network communication and internal and external data flows are maintained | Network diagrams showing clinical network segments, BMS isolated networks, IoMT VLANs, DMZ architecture, and external data flows (HIE, clearinghouses, cloud services) | High | HIPAA §164.312(e)(1); NIST SP 800-66 §4.18 |
| **ID.AM-04** | Inventories of services provided by suppliers are maintained | Catalog of third-party services: Cloud EMR hosting, managed security services, medical device remote monitoring, biomedical equipment maintenance | Medium | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **ID.AM-05** | Assets are prioritized based on classification, criticality, resources, and impact to the mission | Asset criticality tiers: Tier 1 (Life-Critical: Ventilators, Infusion Pumps, BMS Life Safety), Tier 2 (Care-Critical: EMR, LIS, RIS), Tier 3 (Operations: HIS, Email) | High | HIPAA §164.308(a)(7)(ii)(E); NIST SP 800-66 §4.14 |
| **ID.AM-07** | Inventories of data and corresponding metadata for designated data types are maintained | PHI data inventory by system: EMR (clinical notes, orders, results), LIS (lab values), RIS (imaging studies), HIS (demographics, billing) with classification and retention | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.1 |
| **ID.AM-08** | Systems, hardware, software, and services are managed throughout their lifecycle | Medical device lifecycle management: procurement security assessment, deployment hardening, patch management, decommissioning with PHI sanitization | High | HIPAA §164.310(d)(2)(i); NIST SP 800-66 §4.17 |
| **ID.RA-01** | Vulnerabilities in assets are identified, validated, and recorded | Continuous vulnerability scanning of IT assets; periodic assessment of IoMT devices; penetration testing of EMR and external-facing systems | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **ID.RA-02** | Cyber threat intelligence is received from information sharing forums and sources | Participation in H-ISAC (Health Information Sharing and Analysis Center); FBI InfraGard Healthcare; HHS 405(d) threat briefings | Medium | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **ID.RA-03** | Internal and external threats to the organization are identified and recorded | Threat register includes: Ransomware groups targeting healthcare, nation-state actors, insider threats, medical device exploitation | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **ID.RA-04** | Potential impacts and likelihoods of threats exploiting vulnerabilities are identified and recorded | Risk assessment methodology (FAIR, NIST 800-30) applied to prioritize risks; clinical impact scoring included in risk ratings | High | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **ID.RA-05** | Threats, vulnerabilities, likelihoods, and impacts are used to understand inherent risk and inform risk response prioritization | Risk register with inherent/residual scores; quarterly review with clinical and IT leadership; risk-based patching prioritization | High | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **ID.RA-06** | Risk responses are chosen, prioritized, planned, tracked, and communicated | POA&M (Plan of Action and Milestones) for all identified risks; risk exception process with executive approval | Medium | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **ID.RA-07** | Changes and exceptions are managed, assessed for risk impact, recorded, and tracked | Change Advisory Board (CAB) reviews all changes to clinical systems; emergency change procedures for patient safety scenarios | High | HIPAA §164.308(a)(1)(ii)(D); NIST SP 800-66 §4.3 |
| **ID.RA-08** | Processes for receiving, analyzing, and responding to vulnerability disclosures are established | Coordinated vulnerability disclosure process for internally discovered and externally reported vulnerabilities; medical device ICS-CERT coordination | Medium | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **ID.RA-09** | The authenticity and integrity of hardware and software are assessed prior to acquisition and use | Secure procurement process: vendor security attestations, software integrity verification, medical device FDA premarket cybersecurity documentation review | Medium | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **ID.RA-10** | Critical suppliers are assessed prior to acquisition | Security due diligence for Tier 1 vendors: SOC 2 Type II review, HITRUST certification preference, security questionnaire, penetration test evidence | High | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **ID.IM-01** | Improvements are identified from evaluations | Post-incident reviews, audit findings, and tabletop exercise lessons learned drive security program improvements | Medium | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |
| **ID.IM-02** | Improvements are identified from security tests and exercises | Annual penetration testing, quarterly vulnerability assessments, and tabletop exercises generate improvement backlog | Medium | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |
| **ID.IM-03** | Improvements are identified from operational execution | Incident metrics, help desk security tickets, and clinical workflow friction points inform continuous improvement | Low | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |

---

## Function 3: PROTECT (PR) — *Enhanced Focus Area*

### Purpose
Implement appropriate safeguards to ensure delivery of critical healthcare services and protection of PHI. This function is prioritized for clinical continuity and PHI integrity.

### 3.1 Identity Management, Authentication, and Access Control (PR.AA)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **PR.AA-01** | Identities and credentials for authorized users, services, and hardware are managed by the organization | Centralized identity management (Active Directory/Azure AD) for workforce; service accounts for system integrations; PKI certificates for medical device authentication | High | HIPAA §164.312(d); NIST SP 800-66 §4.13 |
| **PR.AA-02** | Identities are proofed and bound to credentials based on the context of interactions | Identity proofing for workforce: HR verification, background check completion, badge issuance tied to system access; patient portal identity verification per NIST 800-63 | High | HIPAA §164.312(d); NIST SP 800-66 §4.13 |
| **PR.AA-03** | Users, services, and hardware are authenticated | MFA mandatory for all EMR access, remote access, and privileged accounts; smart card/proximity badge for clinical workstations; certificate-based authentication for IoMT where supported | High | HIPAA §164.312(d); NIST SP 800-66 §4.13 |
| **PR.AA-04** | Identity assertions are protected, conveyed, and verified | SAML/OIDC assertions for SSO to clinical applications; HL7 FHIR SMART on FHIR for authorized API access; assertion signing and encryption | Medium | HIPAA §164.312(d); NIST SP 800-66 §4.13 |
| **PR.AA-05** | Access permissions, entitlements, and authorizations are defined and managed in accordance with the principle of least privilege and separation of duties | Role-Based Access Control (RBAC) in EMR aligned to clinical roles; separation of duties for pharmacy, ordering, and administration; privileged access management (PAM) for IT administrators | High | HIPAA §164.312(a)(1); NIST SP 800-66 §4.11 |
| **PR.AA-06** | Physical access to assets is managed, monitored, and enforced | Badge access to data centers, server rooms, and medication dispensing areas; visitor escort requirements; CCTV monitoring of sensitive areas | High | HIPAA §164.310(a)(1); NIST SP 800-66 §4.16 |

### 3.2 Awareness and Training (PR.AT)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **PR.AT-01** | Personnel are provided awareness and training so they possess the knowledge and skills to perform tasks with cybersecurity risk implications | Annual security awareness training mandatory for all workforce; clinical-specific modules for EMR security, PHI handling, and medical device safety | High | HIPAA §164.308(a)(5)(i); NIST SP 800-66 §4.10 |
| **PR.AT-02** | Individuals in specialized roles are provided awareness and training so they possess the knowledge and skills to perform tasks with cybersecurity risk implications | Role-based training: IT security staff (technical certifications), Clinical Engineering (medical device security), Privacy Officers (HIPAA updates), Incident Responders (forensics) | High | HIPAA §164.308(a)(5)(ii)(A); NIST SP 800-66 §4.10 |

### 3.3 Data Security (PR.DS)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **PR.DS-01** | The confidentiality, integrity, and availability of data-at-rest are protected | AES-256 encryption for all ePHI at rest: EMR databases, file shares, backup media, laptop drives (BitLocker/FileVault), mobile devices | High | HIPAA §164.312(a)(2)(iv); NIST SP 800-66 §4.12 |
| **PR.DS-02** | The confidentiality, integrity, and availability of data-in-transit are protected | TLS 1.2/1.3 for all ePHI transmission: EMR interfaces, HL7/FHIR APIs, email (TLS enforced), VPN for remote access, wireless encryption (WPA3-Enterprise) | High | HIPAA §164.312(e)(2)(ii); NIST SP 800-66 §4.18 |
| **PR.DS-10** | The confidentiality, integrity, and availability of data-in-use are protected | Secure enclaves for sensitive processing; memory encryption where available; screen privacy filters in clinical areas; automatic session timeout | Medium | HIPAA §164.312(a)(2)(iii); NIST SP 800-66 §4.11 |
| **PR.DS-11** | Backups of data are created, protected, maintained, and tested for restoration | 3-2-1 backup strategy for EMR and critical systems: 3 copies, 2 media types, 1 offsite/air-gapped; immutable backups for ransomware resilience; quarterly restoration testing | High | HIPAA §164.308(a)(7)(ii)(A); NIST SP 800-66 §4.14 |

### 3.4 Platform Security (PR.PS)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **PR.PS-01** | Configuration management practices are established and applied | Secure baseline configurations for servers, workstations, network devices; CIS Benchmarks applied; medical device hardening per manufacturer guidance and FDA recommendations | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **PR.PS-02** | Software is maintained, replaced, and removed commensurate with risk | Patch management: Critical patches within 14 days (30 days for IoMT pending clinical validation); end-of-life software replacement program; medical device firmware update procedures | High | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **PR.PS-03** | Hardware is maintained, replaced, and removed commensurate with risk | Hardware lifecycle management; end-of-support device replacement; secure disposal with PHI sanitization (NIST 800-88 media sanitization) | Medium | HIPAA §164.310(d)(2)(i); NIST SP 800-66 §4.17 |
| **PR.PS-04** | Log records are generated and made available for continuous monitoring | Centralized logging: EMR audit logs, network device logs, authentication events, BMS alerts, medical device logs (where available); minimum 12-month retention | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **PR.PS-05** | Installation and execution of unauthorized software is prevented | Application whitelisting on clinical workstations; endpoint detection and response (EDR); Group Policy restrictions; medical device USB port controls | High | HIPAA §164.308(a)(1)(ii)(B); NIST SP 800-66 §4.3 |
| **PR.PS-06** | Secure software development practices are integrated, and their performance is monitored | Secure development lifecycle for in-house clinical applications; code review requirements; static/dynamic analysis; penetration testing before production deployment | Medium | HIPAA §164.312(b); NIST SP 800-66 §4.11 |

### 3.5 Technology Infrastructure Resilience (PR.IR)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **PR.IR-01** | Networks and environments are protected from unauthorized access and activity | Network segmentation: Clinical VLAN, IoMT isolated network, BMS air-gapped network, Guest WiFi segregation; next-generation firewalls with IPS; micro-segmentation for critical systems | High | HIPAA §164.312(e)(1); NIST SP 800-66 §4.18 |
| **PR.IR-02** | The organization's technology assets are protected from environmental threats | Data center environmental controls: Fire suppression, HVAC redundancy, UPS with generator backup; medical device environmental monitoring | Medium | HIPAA §164.310(a)(2)(ii); NIST SP 800-66 §4.16 |
| **PR.IR-03** | Mechanisms are implemented to achieve resilience requirements in normal and adverse situations | High availability for EMR (99.9% uptime SLA); redundant network paths; failover clustering for critical applications; load balancing | High | HIPAA §164.308(a)(7)(ii)(B); NIST SP 800-66 §4.14 |
| **PR.IR-04** | Adequate resource capacity is maintained to ensure availability | Capacity planning for EMR, storage, and network bandwidth; scalability for surge conditions (pandemic, mass casualty); cloud burst capability | Medium | HIPAA §164.308(a)(7)(ii)(B); NIST SP 800-66 §4.14 |

### 3.6 BMS and IoMT-Specific Protection Controls

| **Control Area** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:-----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **BMS Isolation** | Building Management Systems are logically and/or physically isolated from clinical networks | BMS (HVAC, Life Safety, Fire Suppression) on dedicated air-gapped network or DMZ with strict firewall rules; no direct internet connectivity | High | HIPAA §164.312(e)(1); NIST SP 800-82 |
| **BMS Access Control** | BMS access restricted to authorized facilities and security personnel | Dedicated BMS administrator accounts; MFA for BMS console access; change management for BMS modifications | High | HIPAA §164.312(a)(1); NIST SP 800-82 |
| **BMS Monitoring** | BMS anomalies are detected and alerted | Integration of BMS alerts into SIEM; monitoring for unauthorized configuration changes; life safety system integrity monitoring | High | HIPAA §164.312(b); NIST SP 800-82 |
| **IoMT Inventory** | All connected medical devices are inventoried and risk-classified | Automated medical device discovery; device classification by clinical criticality and network connectivity; shadow IT detection | High | HIPAA §164.310(d)(1); FDA Premarket Guidance |
| **IoMT Network Segmentation** | Medical devices are segmented from general IT network | Dedicated IoMT VLANs; NAC enforcement; micro-segmentation for high-risk devices (infusion pumps, ventilators) | High | HIPAA §164.312(e)(1); FDA Postmarket Guidance |
| **IoMT Vulnerability Management** | Medical device vulnerabilities are identified and mitigated | Coordination with device manufacturers for patches; compensating controls when patching not feasible; monitoring for medical device vulnerability disclosures (ICS-CERT, manufacturer alerts) | High | HIPAA §164.308(a)(1)(ii)(A); FDA Postmarket Guidance |
| **IoMT Secure Lifecycle** | Medical devices are securely configured, maintained, and decommissioned | Secure default configuration; disable unnecessary services/ports; PHI removal before decommissioning; firmware integrity verification | High | HIPAA §164.310(d)(2)(i); FDA Premarket Guidance |

### 3.7 Legacy Medical Equipment Protection (Unpatchable IoMT) — *Critical Focus Area*

> **Challenge:** Hospitals operate thousands of medical devices running end-of-life operating systems (Windows XP, Windows 7, Windows Embedded) that cannot be patched due to FDA certification requirements, manufacturer limitations, or clinical validation constraints. These devices represent critical attack surfaces for ransomware lateral movement.

#### Legacy Device Inventory Classification

| **Device Category** | **Typical OS** | **Patch Status** | **Clinical Criticality** | **Compensating Control Priority** |
|:--------------------|:---------------|:-----------------|:-------------------------|:---------------------------------|
| **MRI/CT Scanners** | Windows XP/7 Embedded | Unpatchable (FDA certified) | High — Diagnostic imaging | Critical |
| **Infusion Pumps** | Proprietary/Windows CE | Unpatchable (Manufacturer locked) | Critical — Medication delivery | Critical |
| **Ventilators** | Proprietary RTOS | Unpatchable (Life-sustaining) | Critical — Life support | Critical |
| **Patient Monitors** | Windows XP Embedded | Unpatchable (Legacy systems) | Critical — Vital signs | Critical |
| **Ultrasound Systems** | Windows 7/10 | Delayed patching (Clinical validation) | High — Diagnostic imaging | High |
| **Lab Analyzers** | Windows 7/Proprietary | Unpatchable (Manufacturer controlled) | High — Diagnostic results | High |
| **PACS Workstations** | Windows 7/10 | Delayed patching | High — Image review | High |
| **Medication Dispensing (Pyxis)** | Windows Embedded | Unpatchable (Vendor managed) | High — Medication safety | High |

#### Compensating Controls Framework for Unpatchable Devices

| **Control Layer** | **Implementation** | **Rationale** | **Verification Method** |
|:------------------|:-------------------|:--------------|:------------------------|
| **1. Network Micro-Segmentation** | Dedicated VLAN per device class with stateful firewall inspection; explicit allow-list of required communication paths | Prevents lateral movement; limits blast radius | Quarterly firewall rule review; penetration testing |
| **2. Virtual Patching (IPS)** | Deploy network-based IPS signatures for known vulnerabilities affecting legacy OS (MS17-010/EternalBlue, BlueKeep, etc.) | Blocks exploitation attempts at network layer | Weekly signature updates; monthly detection validation |
| **3. Application Whitelisting** | Where supported, deploy application control to allow only authorized binaries | Prevents ransomware execution even if delivered | Baseline validation; change detection alerting |
| **4. USB/Removable Media Controls** | Disable or restrict USB ports; require encrypted, scanned media for software updates | Blocks common malware delivery vector | Physical port audits; endpoint policy verification |
| **5. Network Access Control (NAC)** | 802.1X or MAC-based authentication; automatic quarantine for non-compliant devices | Prevents unauthorized devices; enforces segmentation | NAC policy testing; rogue device scanning |
| **6. Traffic Inspection** | East-West traffic inspection between legacy device VLANs and other network segments | Detects lateral movement attempts; blocks C2 traffic | SIEM correlation; traffic anomaly detection |
| **7. Endpoint Detection (Where Possible)** | Deploy lightweight EDR agents on devices that can support them (newer Windows Embedded) | Provides visibility into device behavior | EDR console monitoring; agent health checks |
| **8. Network Behavior Analysis** | Baseline normal traffic patterns; alert on deviations (new connections, port scans, SMB enumeration) | Detects early ransomware reconnaissance | NBA platform tuning; monthly baseline review |
| **9. Hardening** | Disable unnecessary services (SMBv1, RDP if unused), remove unused software, local firewall rules | Reduces attack surface on legacy systems | Configuration audits; hardening checklist validation |
| **10. Offline Backup** | Maintain offline, tested backups of device configurations and clinical data | Enables recovery without paying ransom | Quarterly restoration testing |

#### Legacy Device Communication Policy

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                    LEGACY IoMT NETWORK COMMUNICATION MATRIX                   │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ALLOWED TRAFFIC (Explicit Permit)                                           │
│  ─────────────────────────────────                                           │
│  • MRI/CT → PACS Server (DICOM: TCP 104, 11112)                              │
│  • MRI/CT → RIS (HL7: TCP 2575)                                              │
│  • Infusion Pumps → Pharmacy Server (Proprietary: TCP 443)                   │
│  • Patient Monitors → Central Monitoring (Proprietary: TCP 8080)             │
│  • All Devices → Syslog Server (UDP 514, TCP 514)                            │
│  • All Devices → NTP Server (UDP 123)                                        │
│  • All Devices → DNS Server (UDP/TCP 53) — Restricted to internal DNS        │
│  • Vendor Remote Access → Jump Host Only (VPN-authenticated, time-limited)  │
│                                                                              │
│  DENIED TRAFFIC (Implicit Deny All Else)                                     │
│  ─────────────────────────────────                                            │
│  ✗ Legacy IoMT → Internet (ANY)                                              │
│  ✗ Legacy IoMT → Corporate Network (ANY)                                     │
│  ✗ Legacy IoMT → EMR Servers (Direct — must route via integration engine)   │
│  ✗ Workstations → Legacy IoMT (SMB, RDP, WMI)                                │
│  ✗ Legacy IoMT → Legacy IoMT Cross-VLAN (Device-to-device restricted)        │
│  ✗ ANY → Legacy IoMT (SMBv1: TCP 445, 139 — EternalBlue prevention)          │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

#### Vendor Management for Legacy Equipment

| **Requirement** | **Contract Language** | **Verification** |
|:----------------|:----------------------|:-----------------|
| **Security Patch Commitment** | Vendor must provide security patches for critical vulnerabilities within 90 days of disclosure or provide documented compensating controls | Quarterly vendor security review |
| **End-of-Life Notification** | 24-month advance notice of end-of-support with migration path documentation | Annual vendor roadmap review |
| **Remote Access Security** | All remote access via hospital-approved VPN with MFA; no persistent connections; full audit logging | Remote session audit logs |
| **Vulnerability Disclosure** | Vendor must notify hospital within 72 hours of becoming aware of vulnerabilities in deployed devices | Vendor communication audit |
| **Compensating Control Support** | Vendor must document supported compensating controls if patches unavailable | Pre-procurement security assessment |

#### Legacy Device Risk Acceptance Criteria

When compensating controls cannot fully mitigate risk, formal risk acceptance requires:

1. **Clinical Necessity Justification** — Documented clinical need for continued device operation
2. **Compensating Control Documentation** — All implemented controls with residual risk assessment
3. **Monitoring Commitment** — Enhanced monitoring and alerting for accepted-risk devices
4. **Replacement Timeline** — Approved capital plan for device replacement (maximum 36-month horizon)
5. **Executive Approval** — CISO and CMO joint signature for life-critical devices; CISO for others

### 3.8 Network Segmentation for Ransomware Prevention — *Critical Focus Area*

> **Objective:** Prevent ransomware from propagating from any compromised endpoint (e.g., a receptionist's PC infected via phishing) to clinical systems (surgical wing, ICU, OR). Segmentation is the primary defense-in-depth control after perimeter defenses fail.

#### Network Zone Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                           HOSPITAL NETWORK SEGMENTATION ARCHITECTURE                          │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│                                    ┌──────────────┐                                         │
│                                    │   INTERNET   │                                         │
│                                    └──────┬───────┘                                         │
│                                           │                                                 │
│                                    ┌──────▼───────┐                                         │
│                                    │   Perimeter  │                                         │
│                                    │   Firewall   │                                         │
│                                    └──────┬───────┘                                         │
│                                           │                                                 │
│         ┌─────────────────────────────────┼─────────────────────────────────┐               │
│         │                                 │                                 │               │
│  ┌──────▼───────┐                  ┌──────▼───────┐                  ┌──────▼───────┐       │
│  │     DMZ      │                  │  INTERNAL    │                  │    GUEST     │       │
│  │  (Zone 0)    │                  │   CORE FW    │                  │   WIFI       │       │
│  └──────────────┘                  └──────┬───────┘                  │  (Zone 99)   │       │
│  • Patient Portal                         │                          └──────────────┘       │
│  • External APIs                          │                          • Internet only        │
│  • VPN Concentrator                       │                          • No internal access   │
│                                           │                                                 │
│         ┌─────────────────────────────────┼─────────────────────────────────┐               │
│         │                                 │                                 │               │
│  ┌──────▼───────┐                  ┌──────▼───────┐                  ┌──────▼───────┐       │
│  │ ZONE 1       │                  │ ZONE 2       │                  │ ZONE 3       │       │
│  │ CORPORATE/   │                  │ CLINICAL     │                  │ CRITICAL     │       │
│  │ ADMIN        │                  │ OPERATIONS   │                  │ CARE         │       │
│  └──────────────┘                  └──────────────┘                  └──────────────┘       │
│  • Reception PCs     ◄─────────────────────┐                         • Surgical Wing       │
│  • HR/Finance        DENIED                │                         • ICU                 │
│  • Guest Services                          │                         • OR Systems          │
│  • General Admin     No direct path ───────┘                         • Ventilators         │
│  • Email/Web                                                         • Life-Critical IoMT  │
│                                                                                             │
│  ┌──────────────┐                  ┌──────────────┐                  ┌──────────────┐       │
│  │ ZONE 4       │                  │ ZONE 5       │                  │ ZONE 6       │       │
│  │ DATA CENTER  │                  │ IoMT         │                  │ BMS          │       │
│  │ SERVERS      │                  │ GENERAL      │                  │ AIR-GAPPED   │       │
│  └──────────────┘                  └──────────────┘                  └──────────────┘       │
│  • EMR Servers                     • MRI/CT                          • HVAC Controls       │
│  • Database Servers                • Infusion Pumps                  • Life Safety         │
│  • PACS/RIS                        • Patient Monitors                • Fire Suppression    │
│  • Backup Servers                  • Lab Analyzers                   • Physical Access     │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

#### Inter-Zone Traffic Rules (Ransomware Prevention Focus)

| **Source Zone** | **Destination Zone** | **Permitted Traffic** | **Denied Traffic** | **Rationale** |
|:----------------|:---------------------|:----------------------|:-------------------|:--------------|
| **Zone 1 (Corporate)** | Zone 2 (Clinical) | HTTPS to EMR Portal (443) via VDI only | SMB, RDP, WMI, SSH, All direct protocols | Receptionist infection cannot reach clinical systems |
| **Zone 1 (Corporate)** | Zone 3 (Critical Care) | **NONE** | ALL | Zero trust between admin and life-critical |
| **Zone 1 (Corporate)** | Zone 4 (Servers) | HTTPS to approved apps via LB | SMB, RDP, Database ports | Web-only access; no file share access |
| **Zone 1 (Corporate)** | Zone 5 (IoMT) | **NONE** | ALL | No admin access to medical devices |
| **Zone 1 (Corporate)** | Zone 6 (BMS) | **NONE** | ALL | Air-gapped by design |
| **Zone 2 (Clinical)** | Zone 3 (Critical Care) | HL7/FHIR (2575, 443), Specific clinical protocols | SMB, RDP, General file transfer | Clinical data exchange only |
| **Zone 2 (Clinical)** | Zone 4 (Servers) | EMR/LIS/RIS protocols, Database via app tier | Direct DB access from workstations | Application-layer access only |
| **Zone 2 (Clinical)** | Zone 5 (IoMT) | Clinical monitoring (specific ports) | SMB, RDP, Management protocols | One-way data flow preferred |
| **Zone 4 (Servers)** | Zone 5 (IoMT) | Integration engine traffic only | Direct server-to-device | Integration hub mediates all communication |
| **Zone 5 (IoMT)** | Zone 5 (IoMT) | Limited peer communication (same device class) | Cross-class communication | Infusion pump A cannot talk to MRI B |
| **Zone 6 (BMS)** | ANY | **NONE** outbound | ALL | True air-gap; out-of-band management only |
| **ANY** | ANY | N/A | SMBv1 (TCP 445 v1), MS-RPC (TCP 135), NetBIOS (137-139) | EternalBlue/Ransomware propagation prevention |

#### Micro-Segmentation Implementation

| **Technology** | **Use Case** | **Implementation Notes** |
|:---------------|:-------------|:-------------------------|
| **Next-Gen Firewall (NGFW)** | Zone-level segmentation | L7 application awareness; IPS inline; SSL inspection (except medical device traffic) |
| **Software-Defined Segmentation** | Micro-segmentation within zones | VMware NSX, Cisco ACI, Illumio; policy follows workload |
| **802.1X NAC** | Endpoint zone assignment | Device profiling; auto-VLAN assignment; quarantine for non-compliant |
| **Private VLANs** | IoMT device isolation | Prevents device-to-device communication within same VLAN |
| **Zero Trust Network Access (ZTNA)** | Remote clinical access | Replace VPN with identity-aware, application-specific access |

#### Lateral Movement Detection Controls

| **Detection Method** | **Data Source** | **Alert Trigger** | **Response** |
|:---------------------|:----------------|:------------------|:-------------|
| **SMB Enumeration** | Network flow data | Scanning for port 445 across multiple hosts | Block source IP; isolate endpoint |
| **Credential Harvesting** | EDR, Network traffic | LSASS access, Mimikatz patterns, Pass-the-Hash | Immediate endpoint isolation; credential reset |
| **RDP Lateral Movement** | Authentication logs | RDP from workstation to workstation | Alert SOC; investigate source |
| **PsExec/WMI Remote Execution** | Endpoint logs, Network | Remote service creation, WMI process spawn | Block and isolate; forensic capture |
| **Unusual Cross-Zone Traffic** | Firewall logs | Zone 1 → Zone 3 attempt | Auto-block; high-priority alert |
| **Ransomware Behavior** | EDR | Mass file encryption, VSS deletion, Ransom note creation | Auto-isolate; activate IR |

---

## Function 4: DETECT (DE)

### Purpose
Enable timely discovery of cybersecurity events that may impact clinical operations, patient safety, or PHI confidentiality.

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **DE.CM-01** | Networks and network services are monitored to find potentially adverse events | Network monitoring: IDS/IPS on network perimeter and internal segments; NetFlow analysis; DNS query logging; monitoring of clinical network traffic patterns | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **DE.CM-02** | The physical environment is monitored to find potentially adverse events | Physical security monitoring: CCTV in data centers, badge access logs, environmental sensors (temperature, humidity, water detection) | Medium | HIPAA §164.310(a)(1); NIST SP 800-66 §4.16 |
| **DE.CM-03** | Personnel activity and technology usage are monitored to find potentially adverse events | User behavior analytics (UBA): Anomalous EMR access patterns, after-hours access, bulk data queries, failed authentication monitoring | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **DE.CM-06** | External service provider activities and services are monitored to find potentially adverse events | Third-party access monitoring: VPN session logging, vendor remote access reviews, cloud service activity logs, managed service provider oversight | High | HIPAA §164.308(b)(1); NIST SP 800-66 §4.19 |
| **DE.CM-09** | Computing hardware, devices, software, and services are monitored to find potentially adverse events | Endpoint detection and response (EDR) on workstations and servers; SIEM correlation; medical device anomaly detection where supported | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **DE.AE-02** | Potentially adverse events are analyzed to better understand associated activities | Security event analysis: Triage procedures, correlation rules, playbooks for common scenarios (ransomware indicators, PHI exfiltration patterns) | High | HIPAA §164.308(a)(1)(ii)(D); NIST SP 800-66 §4.3 |
| **DE.AE-03** | Information is correlated from multiple sources | SIEM integration: EMR audit logs, network logs, authentication events, endpoint alerts, BMS alerts, physical security events correlated for context | High | HIPAA §164.312(b); NIST SP 800-66 §4.11 |
| **DE.AE-04** | The estimated impact and scope of adverse events are understood | Impact assessment procedures: PHI exposure estimation, affected patient count determination, clinical operations impact analysis | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **DE.AE-06** | Information on adverse events is provided to authorized staff and tools | Alert routing: Security operations center (SOC), on-call escalation, clinical leadership notification for patient safety events | High | HIPAA §164.308(a)(6)(i); NIST SP 800-66 §4.14 |
| **DE.AE-07** | Cyber threat intelligence and other contextual information are integrated into the analysis | Threat intelligence feeds integrated into SIEM: H-ISAC indicators, healthcare-specific ransomware IOCs, medical device vulnerability intelligence | Medium | HIPAA §164.308(a)(1)(ii)(A); NIST SP 800-66 §4.2 |
| **DE.AE-08** | Incidents are declared when adverse events meet defined criteria | Incident declaration thresholds: PHI access anomaly, ransomware indicator, BMS manipulation, medical device compromise; escalation matrix | High | HIPAA §164.308(a)(6)(i); NIST SP 800-66 §4.14 |

### Detection Use Cases for Hospital Environment

| **Detection Scenario** | **Data Sources** | **Alert Criteria** | **Priority** |
|:-----------------------|:-----------------|:-------------------|:-------------|
| **EMR PHI Exfiltration** | EMR audit logs, DLP, network traffic | Bulk record access, print spikes, USB activity, large outbound transfers | High |
| **Ransomware Precursors** | EDR, network logs, email gateway | Known ransomware IOCs, lateral movement, privilege escalation, encryption behavior | High |
| **Credential Compromise** | Authentication logs, UBA | Failed login spikes, impossible travel, off-hours access, service account anomalies | High |
| **Medical Device Anomaly** | IoMT monitoring platform, network logs | Unexpected network connections, firmware modification, configuration changes | High |
| **BMS Manipulation** | BMS logs, SIEM | Unauthorized configuration changes, environmental threshold alterations, remote access anomalies | High |
| **Insider Threat** | EMR audit logs, UBA, badge access | Accessing records without treatment relationship, snooping patterns, policy violations | High |

---

## Function 5: RESPOND (RS) — *Enhanced Focus Area*

### Purpose
Enable rapid and effective response to detected cybersecurity incidents to minimize clinical disruption, protect patient safety, and contain PHI exposure.

### 5.1 Incident Management (RS.MA)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RS.MA-01** | The incident response plan is executed in coordination with relevant third parties once an incident is declared | IRP activation: Incident Commander, clinical operations liaison, legal/compliance, communications, external coordination (law enforcement, HHS, cyber insurance carrier) | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.MA-02** | Incident reports are triaged and validated | Triage procedures: Severity classification (Critical/High/Medium/Low), PHI impact assessment, clinical operations impact determination | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.MA-03** | Incidents are categorized and prioritized | Categorization taxonomy: Data breach, ransomware, medical device compromise, BMS incident, insider threat, availability event; prioritization based on patient safety impact | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.MA-04** | Incidents are escalated or elevated as needed | Escalation matrix: Patient safety incidents to CMO, PHI breaches to Privacy Officer, major incidents to CEO/Board, regulatory notification triggers | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.MA-05** | The criteria for initiating incident recovery are applied | Recovery initiation criteria: Threat containment verified, forensic preservation complete, root cause identified (or contained), clinical operations safe to restore | High | HIPAA §164.308(a)(7)(i); NIST SP 800-66 §4.14 |

### 5.2 Incident Analysis (RS.AN)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RS.AN-03** | Analysis is performed to establish what has taken place during an incident and root cause | Forensic analysis: Chain of custody procedures, memory/disk imaging, log analysis, timeline reconstruction, malware reverse engineering (as needed) | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.AN-06** | Actions performed during investigation are recorded, and records' integrity and provenance are preserved | Investigation documentation: Timestamped actions log, evidence handling records, forensic tool outputs, analysis reports; legal hold procedures | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.AN-07** | Incident data and metadata are collected, and their integrity and provenance are preserved | Evidence collection: Secure evidence storage, hash verification, chain of custody, preservation for potential litigation or regulatory investigation | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.AN-08** | The magnitude of the incident is estimated and validated | Impact quantification: Number of affected patients, PHI elements exposed, systems compromised, clinical operations disrupted, financial exposure estimation | High | HIPAA §164.402; NIST SP 800-66 §4.14 |

### 5.3 Incident Response Reporting and Communication (RS.CO)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RS.CO-02** | Internal and external stakeholders are notified of incidents | Stakeholder notification: Executive leadership, Board (for significant incidents), affected departments, clinical staff (for operational impacts) | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.CO-03** | Information is shared with designated internal and external stakeholders | Information sharing: H-ISAC community, law enforcement (FBI, Secret Service), HHS/OCR (for reportable breaches), state attorneys general, media (per communications plan) | High | HIPAA §164.404-408; NIST SP 800-66 §4.14 |

### 5.4 Incident Mitigation (RS.MI)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RS.MI-01** | Incidents are contained | Containment procedures: Network isolation, account disablement, system quarantine, clinical downtime procedures activation; balance containment with clinical continuity | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |
| **RS.MI-02** | Incidents are eradicated | Eradication procedures: Malware removal, compromised credential reset, vulnerability remediation, system rebuild from known-good state; verification before restoration | High | HIPAA §164.308(a)(6)(ii); NIST SP 800-66 §4.14 |

### 5.5 Clinical Downtime Procedures

| **Control Area** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:-----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **Downtime Planning** | Clinical operations continue during cybersecurity incidents | Documented downtime procedures for EMR, LIS, RIS, pharmacy systems; paper-based fallback processes; downtime forms available and staff trained | High | HIPAA §164.308(a)(7)(i); NIST SP 800-66 §4.14 |
| **Downtime Activation** | Rapid transition to downtime procedures when needed | Clear activation criteria; communication cascade; downtime workstation availability; clinical leadership notification | High | HIPAA §164.308(a)(7)(i); NIST SP 800-66 §4.14 |
| **Downtime Operations** | Patient care continues safely during system outages | Medication administration verification, laboratory result communication, imaging access alternatives, patient identification procedures | High | HIPAA §164.308(a)(7)(i); NIST SP 800-66 §4.14 |
| **Downtime Recovery** | Orderly restoration and data reconciliation | System restoration prioritization, data backentry procedures, order reconciliation, audit of downtime period activities | High | HIPAA §164.308(a)(7)(ii)(D); NIST SP 800-66 §4.14 |

### 5.6 Incident Response Scenarios

| **Incident Type** | **Immediate Actions** | **Clinical Considerations** | **Notification Requirements** |
|:------------------|:----------------------|:----------------------------|:------------------------------|
| **Ransomware Attack** | Network isolation, forensic preservation, containment assessment, restore from backup | Activate clinical downtime procedures; prioritize life-critical systems; communicate with clinical leadership | Law enforcement, HHS (if PHI breach), cyber insurance, potentially media |
| **EMR Compromise** | Account lockout, session termination, access log review, password resets | Downtime procedures; verify order integrity; patient safety assessment | Privacy Officer, potentially HHS, affected patients if PHI accessed |
| **Medical Device Compromise** | Device isolation, clinical engineering engagement, manufacturer notification, patient safety assessment | Alternative device availability; patient monitoring continuity; FDA MedWatch if patient harm | FDA (if safety event), manufacturer, potentially patients |
| **BMS Attack** | Isolate BMS network, manual override to safe state, facilities team activation | Patient evacuation readiness (life safety); environmental monitoring; ICU/OR prioritization | Local authorities (if safety threat), potentially media |
| **PHI Breach** | Access termination, scope determination, evidence preservation | Impact assessment for affected patients; credit monitoring consideration | HHS within 60 days (500+ individuals), state AGs, affected individuals, media (500+ in state) |
| **Insider Threat** | Account suspension, access revocation, HR coordination | Minimal clinical disruption; confidential investigation | Law enforcement (if criminal), Privacy Officer, HR |

---

## Function 6: RECOVER (RC)

### Purpose
Restore capabilities and services impaired by cybersecurity incidents to enable clinical operations and minimize impact on patient care.

### 6.1 Incident Recovery Plan Execution (RC.RP)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RC.RP-01** | The recovery portion of the incident response plan is executed once initiated from the incident response process | Recovery plan activation: Restoration prioritization (life-critical → care-critical → operations), resource mobilization, vendor coordination | High | HIPAA §164.308(a)(7)(i); NIST SP 800-66 §4.14 |
| **RC.RP-02** | Recovery actions are selected, scoped, and prioritized considering the criticality and overall organizational objectives | Clinical-driven prioritization: Ventilators/Infusion Pumps → EMR → LIS/RIS → Pharmacy → HIS; RPO/RTO-based sequencing | High | HIPAA §164.308(a)(7)(ii)(B); NIST SP 800-66 §4.14 |
| **RC.RP-03** | The integrity of backups and other restoration assets is verified before use | Backup validation: Integrity verification, malware scanning of backup media, test restoration before production recovery | High | HIPAA §164.308(a)(7)(ii)(D); NIST SP 800-66 §4.14 |
| **RC.RP-04** | Critical functions and cybersecurity risk management are considered to establish post-incident operational norms | Post-incident security posture: Enhanced monitoring, compensating controls, accelerated patching; lessons learned integration | High | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |
| **RC.RP-05** | The integrity of restored assets is verified, systems and services are restored, and normal operations are confirmed | Restoration verification: System functionality testing, data integrity checks, clinical workflow validation, user acceptance before full operations | High | HIPAA §164.308(a)(7)(ii)(D); NIST SP 800-66 §4.14 |
| **RC.RP-06** | The end of incident recovery is declared based on criteria, and incident-related documentation is completed | Recovery closure: Restoration completion criteria, post-incident report, lessons learned documentation, POA&M for identified gaps | Medium | HIPAA §164.308(a)(8); NIST SP 800-66 §4.15 |

### 6.2 Incident Recovery Communication (RC.CO)

| **Subcategory** | **Desired Outcome** | **Hospital Context** | **Priority** | **Informative Reference** |
|:----------------|:--------------------|:---------------------|:-------------|:--------------------------|
| **RC.CO-03** | Recovery activities and progress in restoring capabilities are communicated to internal and external stakeholders | Status communication: Regular updates to executive leadership, clinical departments, Board; external communication to regulators, patients (if applicable), media | High | HIPAA §164.308(a)(7)(ii)(E); NIST SP 800-66 §4.14 |
| **RC.CO-04** | Public updates on incident recovery are shared using approved methods and messaging | Public communication: Coordinated with legal/PR; HIPAA-compliant messaging; patient notification per breach notification requirements | Medium | HIPAA §164.404; NIST SP 800-66 §4.14 |

### 6.4 Clinical Continuity During System Recovery — *Critical Focus Area*

> **AVAILABILITY IMPERATIVE:** Patient care cannot stop during cyber incidents. This section defines how physicians, nurses, and clinical staff maintain safe patient care using paper-based procedures while systems are restored. Clinical Continuity is the bridge between incident containment and full system recovery.

#### 6.3.1 Paper-Based Downtime Procedures

##### Pre-Positioned Downtime Supplies

| **Location** | **Supplies Required** | **Quantity** | **Refresh Cycle** |
|:-------------|:----------------------|:-------------|:------------------|
| **Every Nursing Unit** | Downtime forms packet, Medication administration records (MAR), Patient assessment forms, Lab requisition forms, Radiology requisition forms, Physician order sheets | 72-hour supply per unit census | Monthly verification |
| **Emergency Department** | Trauma sheets, Triage forms, ED order sets, Discharge instruction templates, Patient tracking whiteboard supplies | 72-hour supply for surge capacity | Weekly verification |
| **Operating Rooms** | Pre-printed preference cards, Surgical count sheets, Anesthesia records, Post-op order sets, Consent forms | 72-hour supply | Daily verification |
| **Pharmacy** | Medication reference books (paper), Drug interaction references, Insulin sliding scale references, Antibiotic dosing charts, Downtime dispensing logs | Permanent reference | Annual update |
| **Radiology** | Paper requisitions, CD burning supplies (for offline image transfer), Portable reading station battery backup | 72-hour supply | Monthly verification |
| **Laboratory** | Paper requisitions, Critical value reporting forms, Specimen labeling supplies, Pneumatic tube backup procedures | 72-hour supply | Monthly verification |

##### Downtime Workflow Procedures

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                              CLINICAL DOWNTIME WORKFLOW                                       │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐   │
│  │   PHASE 1       │    │   PHASE 2       │    │   PHASE 3       │    │   PHASE 4       │   │
│  │   ACTIVATION    │───►│   OPERATIONS    │───►│   TRANSITION    │───►│   RECOVERY      │   │
│  │   (0-30 min)    │    │   (Ongoing)     │    │   (When Ready)  │    │   (Post-Restore)│   │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘   │
│                                                                                             │
│  PHASE 1: ACTIVATION (First 30 Minutes)                                                    │
│  ─────────────────────────────────────────                                                  │
│  □ Incident Commander declares downtime via overhead announcement + pager blast            │
│  □ Charge nurses retrieve downtime supply kits from unit stock                             │
│  □ Print last available patient census from downtime workstations (if available)           │
│  □ Retrieve last known medication lists from backup (paper or cached)                       │
│  □ Notify all physicians on duty via downtime pager code                                   │
│  □ Activate runner system for inter-department communication                               │
│  □ Post downtime status on unit whiteboards                                                │
│                                                                                             │
│  PHASE 2: ONGOING OPERATIONS                                                                │
│  ──────────────────────────────                                                             │
│  MEDICATION ADMINISTRATION:                                                                 │
│    • Verify patient identity: Wristband + verbal confirmation (name + DOB)                 │
│    • Document on paper MAR: Time, medication, dose, route, nurse initials                  │
│    • Two-nurse verification for high-alert medications (insulin, heparin, chemotherapy)    │
│    • Call pharmacy for non-emergent questions; stat medications via runner                 │
│    • Pain medication tracking on separate controlled substance log                         │
│                                                                                             │
│  PHYSICIAN ORDERS:                                                                          │
│    • Write orders on pre-printed order sheets (legible handwriting required)               │
│    • Verbal orders: Read-back required; document "V.O." with physician name               │
│    • Stat labs/imaging: Call department directly; send paper requisition via runner        │
│    • New admissions: Paper H&P; verify allergies verbally with patient/family              │
│                                                                                             │
│  LABORATORY:                                                                                │
│    • Paper requisitions with patient label (from pre-printed stock)                        │
│    • Critical values: Phone directly to ordering physician; document call-back             │
│    • Maintain paper log of all resulted specimens for later EMR entry                      │
│                                                                                             │
│  RADIOLOGY:                                                                                 │
│    • Paper requisitions; clinical indication required for all exams                        │
│    • Verbal preliminary reads for stat exams; paper report follows                         │
│    • Images stored locally; transferred via CD if PACS unavailable                         │
│                                                                                             │
│  PATIENT IDENTIFICATION (CRITICAL FOR SAFETY):                                              │
│    • Two patient identifiers required for ALL care activities                              │
│    • Wristband must match paper documents                                                  │
│    • If wristband missing: Supervisor verification + new wristband application            │
│    • Photo ID for outpatient/ED if available                                               │
│                                                                                             │
│  PHASE 3: TRANSITION (System Restoration Begins)                                            │
│  ───────────────────────────────────────────                                                │
│  □ Incident Commander announces: "Downtime ending; transition period begins"              │
│  □ Continue paper documentation for in-progress activities                                 │
│  □ Begin data entry of paper records into restored EMR (prioritize active orders)         │
│  □ Nursing staff enter medications administered during downtime                            │
│  □ Physicians co-sign verbal orders entered into system                                    │
│  □ Lab and radiology staff reconcile paper results with electronic records                 │
│                                                                                             │
│  PHASE 4: POST-RECOVERY RECONCILIATION                                                      │
│  ────────────────────────────────────────                                                   │
│  □ All paper documentation scanned to patient record                                       │
│  □ Medication reconciliation for every patient active during downtime                      │
│  □ Order verification: All paper orders transcribed and verified                           │
│  □ Quality review: Sample audit of downtime documentation accuracy                         │
│  □ Incident report: Any near-misses or errors during downtime documented                  │
│  □ Paper supplies restocked within 24 hours                                                │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

##### Critical Clinical Scenarios During Downtime

| **Scenario** | **Immediate Action** | **Documentation** | **Safety Check** |
|:-------------|:---------------------|:------------------|:-----------------|
| **Code Blue / Cardiac Arrest** | Proceed with ACLS protocol; use paper code sheet | Document all medications, times, interventions on paper code record | Two-person verification of all medications drawn |
| **Stat Medication Order** | Pharmacy provides medication to runner; nurse administers | Paper MAR documentation; verbal read-back of order | Two-nurse verification for high-alert meds |
| **Blood Transfusion** | Use paper transfusion record; manual crossmatch verification | Document unit number, start time, vital signs on paper | Two-RN verification of patient ID and blood unit |
| **New Admission** | Paper admission packet; verbal allergy verification | Paper H&P, orders, nursing assessment | Allergy band placed; allergies on all paper documents |
| **Surgical Case** | Proceed if already prepped; paper surgical record | Paper anesthesia record, count sheets, post-op orders | Time-out performed verbally with full team |
| **Medication Refill Request** | Pharmacist verifies via paper records or patient's pill bottles | Document verification method and decision | Critical medications prioritized; non-urgent deferred |
| **Abnormal Lab/Critical Value** | Lab calls physician directly; documents call-back | Paper critical value log with time and recipient | Read-back of values required; physician acknowledges |

##### Post-Downtime Data Reconciliation

| **Data Type** | **Entry Priority** | **Entry Deadline** | **Verification Method** | **Responsible Party** |
|:--------------|:-------------------|:-------------------|:------------------------|:----------------------|
| **Active Medication Orders** | Critical | 2 hours post-restore | Pharmacist review of entered orders | Nursing + Pharmacy |
| **Administered Medications** | High | 4 hours post-restore | MAR reconciliation with paper records | Nursing |
| **Physician Orders** | High | 4 hours post-restore | Physician co-signature | Physicians + Unit Clerk |
| **Lab Results** | High | 4 hours post-restore | LIS reconciliation | Laboratory |
| **Radiology Results** | High | 8 hours post-restore | PACS/RIS reconciliation | Radiology |
| **Nursing Assessments** | Medium | 12 hours post-restore | Chart review | Nursing |
| **Vital Signs** | Medium | 12 hours post-restore | Flowsheet entry | Nursing/Tech |
| **Physician Notes** | Medium | 24 hours post-restore | Physician signature | Physicians |

#### 6.3.2 Clinical Staff Roles During Downtime

| **Role** | **Downtime Responsibilities** | **Communication Method** |
|:---------|:------------------------------|:-------------------------|
| **Charge Nurse** | Activate unit downtime procedures; distribute supplies; coordinate runners; maintain patient census whiteboard | Overhead paging, runners, personal cell (backup) |
| **Staff Nurse** | Paper documentation; medication administration with paper MAR; patient identification verification | Direct communication; runner for stat needs |
| **Physician** | Verbal/paper orders; handwritten notes if prolonged; phone consultation | Pager (if operational), direct call, runners |
| **Pharmacist** | Paper order verification; medication dispensing with paper log; drug information resource | Phone, runners |
| **Unit Clerk** | Manage paper requisitions; coordinate runners; maintain communication log | Phone, fax (backup), runners |
| **Lab Technician** | Process specimens with paper requisitions; phone critical values | Phone, pneumatic tube (if operational) |
| **Radiology Technologist** | Paper requisitions; portable exams; verbal prelim reads | Phone, runners |
| **Respiratory Therapist** | Paper ventilator records; manual settings backup; patient rounding | Direct communication, pagers |
| **Runner/Transport** | Physical delivery of requisitions, results, specimens, medications | Direct movement |

#### 6.3.3 Downtime Duration-Based Escalation

| **Duration** | **Status** | **Escalation Actions** | **Clinical Considerations** |
|:-------------|:-----------|:-----------------------|:---------------------------|
| **0-4 Hours** | Level 1: Standard Downtime | Standard paper procedures; no case deferrals | Continue normal operations with paper backup |
| **4-12 Hours** | Level 2: Extended Downtime | Cancel elective procedures; defer non-urgent admissions; increase runner staffing | Prioritize emergency and urgent cases only |
| **12-24 Hours** | Level 3: Prolonged Downtime | Activate Incident Command Center; consider patient diversion for new cases; staff augmentation | Medication supply assessment; pharmacy prioritization |
| **24-72 Hours** | Level 4: Critical Downtime | Full ICS activation; mutual aid requests; potential partial patient transfer to unaffected facilities | Reassess all patient care plans; consider discharge of stable patients |
| **>72 Hours** | Level 5: Disaster | Coordinate with regional healthcare coalition; HHS involvement; potential federal assistance | Assess continued operational capability; patient transfer planning |

### 6.4 Recovery Time and Recovery Point Objectives

| **System Category** | **Recovery Time Objective (RTO)** | **Recovery Point Objective (RPO)** | **Justification** |
|:--------------------|:----------------------------------|:-----------------------------------|:------------------|
| **Life-Critical IoMT** (Ventilators, Infusion Pumps) | 15 minutes | N/A (Real-time devices) | Direct patient life support |
| **EMR** | 4 hours | 1 hour | Clinical decision-making continuity |
| **LIS** | 4 hours | 1 hour | Diagnostic result availability |
| **RIS/PACS** | 8 hours | 4 hours | Imaging access for diagnosis |
| **Pharmacy System** | 4 hours | 1 hour | Medication safety |
| **BMS Life Safety** | Immediate (manual override) | N/A | Patient and building safety |
| **BMS HVAC** | 4 hours | N/A | Environmental control |
| **HIS (Billing)** | 24 hours | 4 hours | Revenue cycle operations |
| **OIS (Radiotherapy)** | 8 hours | 4 hours | Cancer treatment scheduling |
| **Email/Communication** | 8 hours | 4 hours | Operational communication |

---

## Appendix A: NIST IR 8374 Ransomware Controls \u2014 Hospital Adaptation

> This appendix maps the recommendations from **NIST IR 8374 (Ransomware Risk Management: A Cybersecurity Framework Profile)** to hospital-specific implementations, with Availability prioritized above Confidentiality and Integrity.

### IR 8374 IDENTIFY Function Adaptations

| **IR 8374 Recommendation** | **Hospital Implementation** | **Availability Focus** |
|:---------------------------|:----------------------------|:-----------------------|
| **ID.AM: Identify all organizational assets** | IoMT device discovery including legacy Windows XP/7 systems; asset criticality tiered by patient safety impact (Life-Critical > Care-Critical > Operational) | Asset inventory enables rapid isolation of infected segments while preserving life-critical device availability |
| **ID.BE: Understand business environment** | Map clinical workflows that depend on each system; document paper-based alternatives for every electronic workflow | Enables immediate fallback to paper procedures; maintains clinical operations during ransomware response |
| **ID.RA: Risk assessment** | Include ransomware scenarios in tabletop exercises; model lateral movement paths from administrative to clinical zones | Identifies segmentation gaps before exploitation; prioritizes controls that preserve availability |

### IR 8374 PROTECT Function Adaptations

| **IR 8374 Recommendation** | **Hospital Implementation** | **Availability Focus** |
|:---------------------------|:----------------------------|:-----------------------|
| **PR.AC: Access management** | Privilege de-escalation for clinical workstations; prevent administrative account use on endpoints; disable SMBv1 hospital-wide | Limits ransomware's ability to spread; preserves segmented zones |
| **PR.AT: Security training** | Clinical-specific phishing training; downtime procedure drills quarterly | Staff can maintain care operations without systems; reduces initial infection vectors |
| **PR.DS: Data security (Backup focus)** | Immutable backups with air-gap or offline storage; 3-2-1-1 rule (3 copies, 2 media, 1 offsite, 1 immutable); backup integrity testing quarterly | Guarantees restoration capability; prevents backup encryption by ransomware |
| **PR.IP: Configuration management** | Disable unnecessary services on all endpoints; application whitelisting on clinical workstations; legacy device virtual patching | Reduces attack surface; compensates for unpatchable IoMT |
| **PR.PT: Network segmentation** | Six-zone architecture with internal firewalls; micro-segmentation for legacy IoMT; block SMB/RDP between zones | **Primary ransomware defense**; prevents lateral movement from reception to surgical wing |

### IR 8374 DETECT Function Adaptations

| **IR 8374 Recommendation** | **Hospital Implementation** | **Availability Focus** |
|:---------------------------|:----------------------------|:-----------------------|
| **DE.CM: Continuous monitoring** | EDR on all patchable endpoints; network behavior analysis for IoMT segments; SIEM correlation with ransomware detection rules | Early detection enables rapid isolation; minimizes blast radius and downtime |
| **DE.AE: Anomaly detection** | Baseline medical device traffic patterns; alert on new connections from legacy IoMT; monitor for mass file encryption behavior | Detects ransomware before widespread encryption; protects clinical system availability |
| **DE.DP: Detection processes** | 24/7 SOC coverage or MSSP; ransomware-specific playbooks; automated isolation capabilities | Reduces MTTD/MTTR; preserves availability through rapid response |

### IR 8374 RESPOND Function Adaptations

| **IR 8374 Recommendation** | **Hospital Implementation** | **Availability Focus** |
|:---------------------------|:----------------------------|:-----------------------|
| **RS.RP: Incident response planning** | Ransomware-specific playbook with clinical downtime integration; pre-authorized isolation decisions; escalation to CMO for clinical impact | Enables immediate containment while activating paper procedures; maintains patient care |
| **RS.CO: Communications** | Pre-drafted ransomware notification templates; patient communication plan; regulatory notification workflow (HHS, state AG) | Reduces confusion during incident; enables clinical staff to focus on patient care |
| **RS.MI: Mitigation** | Automated endpoint isolation capability; pre-defined network containment actions; legacy device manual disconnect procedures | Rapid containment limits spread; maintains availability of unaffected zones |

### IR 8374 RECOVER Function Adaptations

| **IR 8374 Recommendation** | **Hospital Implementation** | **Availability Focus** |
|:---------------------------|:----------------------------|:-----------------------|
| **RC.RP: Recovery planning** | Phased recovery: Life-Critical IoMT \u2192 EMR \u2192 Lab/Radiology \u2192 Pharmacy \u2192 Administrative; clean restoration from immutable backups | Prioritizes patient safety systems; enables clinical operations before full recovery |
| **RC.IM: Improvements** | Post-incident review mandatory; segmentation gap analysis; legacy device compensating control reassessment | Prevents recurrence; improves resilience for future incidents |
| **RC.CO: Communications** | Clinical staff updated on restoration progress; downtime-to-online transition announcements; patient notification per HIPAA | Manages transition from paper to electronic; maintains trust |

### Ransomware-Specific Recovery Sequence

```
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502                    RANSOMWARE RECOVERY PRIORITIZATION                          \u2502
\u251c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2524
\u2502                                                                                \u2502
\u2502  PRIORITY 1: LIFE-CRITICAL (RTO: 15 minutes)                                   \u2502
\u2502  \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500                                     \u2502
\u2502  \u2022 Ventilators, Infusion Pumps, Patient Monitors: Verify isolation, not       \u2502
\u2502    infected; these devices rarely affected due to network segmentation        \u2502
\u2502  \u2022 BMS Life Safety: Verify manual override operational                        \u2502
\u2502  \u2022 If affected: Manual operation until replacement; do NOT attempt patching   \u2502
\u2502                                                                                \u2502
\u2502  PRIORITY 2: CARE-CRITICAL (RTO: 4 hours)                                      \u2502
\u2502  \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500                                        \u2502
\u2502  \u2022 EMR: Restore from immutable backup; verify database integrity              \u2502
\u2502  \u2022 Pharmacy System: Critical for medication safety; restore early             \u2502
\u2502  \u2022 Downtime procedures remain active during this phase                        \u2502
\u2502                                                                                \u2502
\u2502  PRIORITY 3: DIAGNOSTIC (RTO: 8 hours)                                         \u2502
\u2502  \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500                                             \u2502
\u2502  \u2022 Laboratory Information System: Enables electronic result reporting         \u2502
\u2502  \u2022 PACS/RIS: Imaging access; paper workflow continues if delayed              \u2502
\u2502                                                                                \u2502
\u2502  PRIORITY 4: OPERATIONAL (RTO: 24 hours)                                       \u2502
\u2502  \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500                                          \u2502
\u2502  \u2022 HIS (Scheduling, Billing): Administrative function; deferred              \u2502
\u2502  \u2022 Email: Communication backup exists; deferred                               \u2502
\u2502                                                                                \u2502
\u2502  PRIORITY 5: ADMINISTRATIVE (RTO: 48+ hours)                                   \u2502
\u2502  \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500                                     \u2502
\u2502  \u2022 Non-clinical systems restored last                                         \u2502
\u2502  \u2022 Rebuild from clean images; full forensic review before reconnection        \u2502
\u2502                                                                                \u2502
\u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
```

### No-Pay Policy Considerations

| **Consideration** | **Guidance** |
|:------------------|:-------------|
| **FBI/CISA Recommendation** | Do not pay ransom; payment funds criminal operations and does not guarantee recovery |
| **Hospital Reality** | If patient lives are immediately at risk and backups failed, executive decision may differ |
| **Preparation is Key** | Immutable backups, tested restoration, and clinical downtime procedures eliminate need to consider payment |
| **If Payment Considered** | Engage legal counsel, law enforcement, and cyber insurance carrier before any decision; understand OFAC sanctions risk |

---

## Appendix B: HIPAA Security Rule Crosswalk

| **HIPAA Standard** | **45 CFR Citation** | **Related CSF Subcategories** |
|:-------------------|:--------------------|:------------------------------|
| Security Management Process | §164.308(a)(1) | GV.RM-01, GV.RM-02, ID.RA-01 through ID.RA-10 |
| Assigned Security Responsibility | §164.308(a)(2) | GV.RR-01, GV.RR-02 |
| Workforce Security | §164.308(a)(3) | GV.RR-04, PR.AA-05 |
| Information Access Management | §164.308(a)(4) | PR.AA-05, PR.AA-06 |
| Security Awareness and Training | §164.308(a)(5) | PR.AT-01, PR.AT-02 |
| Security Incident Procedures | §164.308(a)(6) | RS.MA-01 through RS.MA-05, RS.AN-03 through RS.AN-08 |
| Contingency Plan | §164.308(a)(7) | RC.RP-01 through RC.RP-06, PR.DS-11 |
| Evaluation | §164.308(a)(8) | GV.OV-01, GV.OV-03, ID.IM-01 through ID.IM-03 |
| Business Associate Contracts | §164.308(b) | GV.SC-01 through GV.SC-07 |
| Facility Access Controls | §164.310(a) | PR.AA-06, DE.CM-02 |
| Workstation Use/Security | §164.310(b)/(c) | PR.PS-01, PR.PS-05 |
| Device and Media Controls | §164.310(d) | ID.AM-01, ID.AM-08, PR.PS-03 |
| Access Control | §164.312(a) | PR.AA-01 through PR.AA-05 |
| Audit Controls | §164.312(b) | PR.PS-04, DE.CM-01, DE.CM-03 |
| Integrity | §164.312(c) | PR.DS-01, PR.DS-02 |
| Person or Entity Authentication | §164.312(d) | PR.AA-01, PR.AA-02, PR.AA-03 |
| Transmission Security | §164.312(e) | PR.DS-02, PR.IR-01 |
| Policies and Procedures | §164.316 | GV.PO-01, GV.PO-02 |

---

## Appendix C: Priority Summary Dashboard

### High Priority Items by Function

| **Function** | **High Priority Subcategories** | **Total High Priority** |
|:-------------|:--------------------------------|:-----------------------:|
| **GOVERN** | GV.OC-01 through GV.OC-04, GV.RM-01 through GV.RM-03, GV.RR-01, GV.RR-02, GV.RR-04, GV.PO-01, GV.PO-02, GV.SC-01 through GV.SC-05 | 17 |
| **IDENTIFY** | ID.AM-01, ID.AM-02, ID.AM-03, ID.AM-05, ID.AM-07, ID.AM-08, ID.RA-01, ID.RA-03 through ID.RA-05, ID.RA-07, ID.RA-10 | 12 |
| **PROTECT** | PR.AA-01 through PR.AA-06, PR.AT-01, PR.AT-02, PR.DS-01, PR.DS-02, PR.DS-11, PR.PS-01, PR.PS-02, PR.PS-04, PR.PS-05, PR.IR-01, PR.IR-03, + IoMT/BMS controls | 25+ |
| **DETECT** | DE.CM-01, DE.CM-03, DE.CM-06, DE.CM-09, DE.AE-02 through DE.AE-04, DE.AE-06, DE.AE-08 | 10 |
| **RESPOND** | RS.MA-01 through RS.MA-05, RS.AN-03, RS.AN-06 through RS.AN-08, RS.CO-02, RS.CO-03, RS.MI-01, RS.MI-02, + Downtime controls | 16+ |
| **RECOVER** | RC.RP-01 through RC.RP-05, RC.CO-03 | 6 |

### Implementation Roadmap

| **Phase** | **Timeline** | **Focus Areas** | **Key Deliverables** |
|:----------|:-------------|:----------------|:---------------------|
| **Phase 1: Foundation** | Months 1-6 | Governance, Asset Inventory, Policy Framework | Risk appetite statement, asset inventory, policy library, RACI matrix |
| **Phase 2: Protection** | Months 4-12 | Access Control, Encryption, Network Segmentation, IoMT Security | MFA deployment, encryption implementation, network segmentation, IoMT visibility |
| **Phase 3: Detection** | Months 8-14 | SIEM Deployment, Monitoring, Threat Intelligence | SIEM operational, use cases deployed, H-ISAC integration |
| **Phase 4: Response** | Months 10-18 | IRP Development, Downtime Procedures, Tabletop Exercises | IRP documented, downtime procedures tested, quarterly exercises |
| **Phase 5: Recovery** | Months 14-20 | Backup Enhancement, DR Testing, Recovery Validation | Immutable backups, annual DR test, recovery playbooks |
| **Phase 6: Continuous Improvement** | Ongoing | Metrics, Assessments, Program Maturation | KRI dashboard, annual assessment, Tier 3 achievement |

---

## Appendix D: Key Performance Indicators (KPIs)

| **Metric Category** | **KPI** | **Target** | **Measurement Frequency** |
|:--------------------|:--------|:-----------|:--------------------------|
| **Vulnerability Management** | Critical vulnerability remediation time | <14 days | Monthly |
| **Vulnerability Management** | Patch compliance rate (critical systems) | >95% | Monthly |
| **Access Control** | MFA adoption rate | 100% (EMR, remote access) | Monthly |
| **Access Control** | Privileged account review completion | 100% quarterly | Quarterly |
| **Awareness** | Security awareness training completion | >95% | Annually |
| **Awareness** | Phishing simulation click rate | <5% | Quarterly |
| **Detection** | Mean Time to Detect (MTTD) | <24 hours | Monthly |
| **Response** | Mean Time to Respond (MTTR) | <4 hours | Monthly |
| **Response** | Incident response plan test completion | Annual | Annually |
| **Recovery** | Backup restoration test success rate | 100% | Quarterly |
| **Recovery** | RTO achievement (EMR) | <4 hours | Per incident |
| **Vendor Management** | Critical vendor security assessment completion | 100% annually | Annually |
| **IoMT** | Medical device inventory accuracy | >98% | Quarterly |
| **IoMT** | IoMT vulnerability assessment coverage | >90% | Quarterly |

---

## Appendix E: Ransomware Tabletop Exercise Scenarios

> Conduct tabletop exercises quarterly to validate clinical downtime procedures and incident response capabilities. Each scenario should include clinical leadership (CMO, CNO), IT, Security, and department representatives.

### Scenario 1: Reception-Originating Ransomware

**Inject:** A receptionist in the main lobby clicked a phishing link disguised as a patient insurance verification portal. Ransomware has encrypted the reception workstation and is attempting lateral movement.

| **Phase** | **Inject** | **Discussion Questions** |
|:----------|:-----------|:-------------------------|
| **Detection (T+0)** | EDR alerts on encryption behavior; SOC sees SMB enumeration from reception workstation | How quickly can we isolate the reception VLAN? What's our automated response capability? |
| **Containment (T+15 min)** | Workstation isolated; scanning reveals 3 additional workstations in Zone 1 (Admin) encrypted | Did segmentation prevent spread to Zone 2 (Clinical)? Who authorizes broader network isolation? |
| **Assessment (T+1 hr)** | Forensics confirms ransomware contained to Zone 1; Zone 2/3/4/5/6 unaffected | Do we activate clinical downtime procedures? How do we communicate to clinical staff? |
| **Recovery (T+4 hrs)** | Zone 1 workstations being rebuilt from images; admin functions degraded but clinical operations normal | What's the priority for restoring reception functions? How do we handle scheduled patients? |
| **Lessons Learned** | Post-exercise debrief | Did network segmentation work as designed? Were there any lateral movement attempts that succeeded? What gaps need remediation? |

### Scenario 2: EMR Server Ransomware (Worst Case)

**Inject:** Attackers exploited a vulnerability in an internet-facing patient portal to gain access. They spent 2 weeks conducting reconnaissance and have now encrypted all EMR application servers and the primary database.

| **Phase** | **Inject** | **Discussion Questions** |
|:----------|:-----------|:-------------------------|
| **Detection (T+0)** | Multiple alerts: EMR application unresponsive; users reporting error messages; mass file encryption detected on server VLAN | How do we confirm this is ransomware vs. system failure? Who declares the incident? |
| **Immediate Response (T+15 min)** | Confirmed ransomware; ransom note demands $5M in Bitcoin | Do we activate full clinical downtime procedures? Who notifies the CMO and CEO? |
| **Clinical Impact (T+30 min)** | All EMR functions unavailable; ongoing surgeries need patient history; ED receiving trauma patient | How do surgeons access critical patient information? What's the downtime procedure for active traumas? |
| **Backup Assessment (T+2 hrs)** | Immutable backup from 1 hour ago confirmed clean; attackers did NOT reach backup infrastructure | Can we restore to a clean environment? What's the RTO for EMR restoration? |
| **Recovery Decision (T+4 hrs)** | Option A: Pay ransom (uncertain outcome). Option B: Restore from backup (4-8 hour RTO) | Who makes the final decision? What are the legal and ethical implications of each option? |
| **Clinical Continuity (T+4-12 hrs)** | Paper-based procedures in effect; data entry backlog growing; staff fatigue increasing | How do we manage staff through extended downtime? When do we cancel elective procedures? |
| **Restoration (T+12-24 hrs)** | EMR restored from backup; data reconciliation beginning | What's the process for entering 12-24 hours of paper documentation? How do we verify data integrity? |

### Scenario 3: Legacy IoMT Compromise

**Inject:** An older MRI machine running Windows XP was compromised. The attacker is using it as a pivot point to scan the IoMT VLAN.

| **Phase** | **Inject** | **Discussion Questions** |
|:----------|:-----------|:-------------------------|
| **Detection (T+0)** | Network behavior analytics detects unusual outbound connections from MRI; scanning activity on IoMT VLAN | How did we detect this given the device can't have EDR? What's our visibility into IoMT traffic? |
| **Containment (T+15 min)** | MRI isolated; scanning shows no other IoMT devices compromised | Can we safely take the MRI offline? What's the clinical impact of losing MRI capability? |
| **Assessment (T+1 hr)** | Forensics reveals attacker entered via vendor remote access left enabled after maintenance | How do we manage vendor access? Should vendor remote access be persistent or on-demand? |
| **Remediation (T+4 hrs)** | MRI reimaged by vendor; compensating controls strengthened | What compensating controls failed? Do we need to accelerate replacement of this legacy device? |
| **Patient Impact** | 15 MRI exams delayed; 3 patients required transfer to partner facility for urgent imaging | What's our mutual aid agreement with partner facilities? How do we communicate delays to patients? |

### Scenario 4: Extended Downtime (72+ Hours)

**Inject:** Sophisticated ransomware attack has encrypted primary systems AND compromised backup infrastructure. Full restoration will take 72+ hours.

| **Phase** | **Inject** | **Discussion Questions** |
|:----------|:-----------|:-------------------------|
| **Assessment (T+4 hrs)** | Backups encrypted; must rebuild from offline tape backup and bare-metal recovery | Do we have offline backups? When was the last test? |
| **Day 1** | Full paper operations; staff working extended shifts; medication error near-miss reported | How do we manage staff fatigue? What additional safety checks are needed? |
| **Day 2** | Elective surgeries cancelled; ED on divert for non-emergent patients; regional coalition activated | When do we divert patients? How do we coordinate with regional partners? |
| **Day 3** | Partial EMR functionality restored for read-only access to historical data; new documentation still paper | What's the priority for write functionality? How do we manage the transition? |
| **Day 4** | Full EMR restored; 72 hours of paper documentation to enter; regulatory reporting due | How do we prioritize data entry? What are our HHS/state notification obligations? |
| **Post-Incident** | Total impact: $15M (response, lost revenue, regulatory fines, litigation reserve) | What investments would have prevented or reduced this impact? What's the business case for enhanced controls? |

---

## Document Control

| **Version** | **Date** | **Author** | **Changes** |
|:------------|:---------|:-----------|:------------|
| 1.0 | January 2026 | Healthcare Cybersecurity Consulting Team | Initial Target Profile |

---

> **Disclaimer:** This Target Profile is a planning document and does not constitute legal advice. Organizations should consult with legal counsel and qualified cybersecurity professionals to ensure compliance with HIPAA and other applicable regulations. The specific implementation of controls should be tailored to the organization's unique environment, risk tolerance, and resources.
