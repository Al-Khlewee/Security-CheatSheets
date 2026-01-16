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

### 6.3 Recovery Time and Recovery Point Objectives

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

## Appendix A: HIPAA Security Rule Crosswalk

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

## Appendix B: Priority Summary Dashboard

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

## Appendix C: Key Performance Indicators (KPIs)

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

## Document Control

| **Version** | **Date** | **Author** | **Changes** |
|:------------|:---------|:-----------|:------------|
| 1.0 | January 2026 | Healthcare Cybersecurity Consulting Team | Initial Target Profile |

---

> **Disclaimer:** This Target Profile is a planning document and does not constitute legal advice. Organizations should consult with legal counsel and qualified cybersecurity professionals to ensure compliance with HIPAA and other applicable regulations. The specific implementation of controls should be tailored to the organization's unique environment, risk tolerance, and resources.
