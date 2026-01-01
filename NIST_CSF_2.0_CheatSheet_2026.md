# NIST CSF 2.0 Technical Cheat Sheet — 2026 Edition

> **Audience:** Senior Engineers, Security Architects, CISOs  
> **Framework Version:** NIST Cybersecurity Framework 2.0 (February 2024) + AI Profile Overlay (2025)  
> **Focus:** AI-Driven Threats, Supply Chain Risk Management (C-SCRM), GRC Alignment

---

## 1. Core Taxonomy Table — Functions, Categories & Technical Evidence

| **Function** | **Category (ID)** | **Purpose** | **Technical Evidence** |
|:-------------|:------------------|:------------|:-----------------------|
| **GOVERN (GV)** | GV.OC — Organizational Context | Define risk appetite, legal/regulatory scope | `risk_register.json`, Board-approved risk appetite statements, compliance matrix |
| | GV.RM — Risk Management Strategy | Establish risk tolerance thresholds | SIEM risk scoring configs, `$RiskScore = \frac{Likelihood \times Impact}{Mitigation}$` formulas in GRC platform |
| | GV.RR — Roles & Responsibilities | RACI matrices for security functions | IAM role definitions, `rbac_policies.yaml`, org chart in CMDB |
| | GV.PO — Policy | Security policy enforcement | Policy-as-code repos, `opa_policies/`, signed policy attestations |
| | GV.OV — Oversight | Continuous governance monitoring | GRC dashboard exports, audit trail logs (`/var/log/audit/`) |
| | GV.SC — Supply Chain Risk Management | Third-party risk governance | SBOM files (`sbom.spdx.json`), vendor risk assessments, C-SCRM tier classifications |
| **IDENTIFY (ID)** | ID.AM — Asset Management | Hardware/software/data inventory | CMDB exports, `nmap` scan results, cloud asset APIs (`aws ec2 describe-instances`) |
| | ID.RA — Risk Assessment | Threat modeling, vuln analysis | CVSS scores in vuln scanner DB, threat model diagrams (STRIDE/PASTA), `threat_model.tm7` |
| | ID.IM — Improvement | Post-incident lessons learned | PIR (Post-Incident Review) documents, JIRA tickets tagged `security-improvement` |
| **PROTECT (PR)** | PR.AA — Identity & Access | AuthN/AuthZ controls | IdP configs, MFA enforcement logs, `Conditional Access Policy` exports (Azure AD/Entra) |
| | PR.AT — Awareness & Training | Security training completion | LMS completion reports, phishing simulation results (`gophish` campaign data) |
| | PR.DS — Data Security | Encryption, DLP, classification | TLS configs (`ssl-cert.pem`), DLP policy triggers, data classification tags in metadata |
| | PR.PS — Platform Security | Hardening, patching, config mgmt | CIS Benchmark scan results, `ansible-playbook` hardening logs, SCAP reports |
| | PR.IR — Infrastructure Resilience | Redundancy, failover, BCP | Load balancer health checks, DR runbook execution logs, RTO/RPO test results |
| **DETECT (DE)** | DE.CM — Continuous Monitoring | Real-time threat detection | SIEM alert rules (`sigma/` rules), EDR telemetry, `osquery` scheduled queries |
| | DE.AE — Adverse Event Analysis | Alert triage, correlation | SOAR playbook execution logs, enriched alert JSON, TI feed correlation hits |
| **RESPOND (RS)** | RS.MA — Incident Management | Escalation, containment, eradication | IR ticket lifecycle (`ServiceNow` states), forensic image hashes, containment scripts |
| | RS.AN — Incident Analysis | Root cause analysis | Timeline artifacts (`plaso` output), memory dumps, MITRE ATT&CK mapping |
| | RS.CO — Incident Communication | Internal/external notifications | Breach notification templates, regulatory submission confirmations |
| | RS.MI — Incident Mitigation | Short-term remediation | Firewall block rules (`iptables -A INPUT -s <IOC_IP> -j DROP`), quarantine actions |
| **RECOVER (RC)** | RC.RP — Recovery Planning | Restoration procedures | Backup verification logs, recovery runbook, last successful backup timestamp |
| | RC.CO — Recovery Communication | Stakeholder updates | Status page updates, executive briefing decks, customer notification logs |

---

## 2. Implementation Tiers — Maturity Progression

| **Attribute** | **Tier 1: Partial** | **Tier 2: Risk-Informed** | **Tier 3: Repeatable** | **Tier 4: Adaptive** |
|:--------------|:--------------------|:--------------------------|:-----------------------|:---------------------|
| **Risk Management** | Ad-hoc, reactive | Risk-aware but inconsistent | Formal policy, consistent execution | Dynamic, real-time risk adjustment |
| **Governance Integration** | Siloed security team | Some executive awareness | Board-level reporting | Risk integrated into business decisions |
| **Threat Intelligence** | Minimal/none | Subscribed feeds, manual review | Automated TI ingestion | **AI-driven predictive threat modeling** |
| **Detection Capability** | Signature-based AV | SIEM with static rules | Behavioral analytics (UEBA) | **ML/LLM-augmented anomaly detection** |
| **Response Posture** | Manual, delayed | Documented playbooks | SOAR automation | **Autonomous containment with human oversight** |
| **Supply Chain Visibility** | Unknown dependencies | Vendor questionnaires | SBOM ingestion, continuous monitoring | **Real-time SBOM drift detection, AI-scored vendor risk** |
| **Technical Evidence** | Sparse audit logs | Periodic vuln scans | Continuous compliance dashboards | Autonomous remediation logs, AI decision audit trails |

### Tier Transition Formula

$$\text{Tier Score} = \frac{\sum_{i=1}^{n} (\text{Category}_i \times \text{Weight}_i)}{\text{Total Categories}} \quad \text{where } \text{Weight} \in [1, 4]$$

**Tier 3 → 4 Prerequisites:**
- [ ] AI/ML models for threat detection validated and monitored for drift
- [ ] Autonomous response guardrails defined and tested
- [ ] Continuous SBOM analysis pipeline operational
- [ ] Real-time risk score integration with business systems

---

## 3. The 2026 "AI Profile" Overlay — NIST Cyber AI Profile

> **Reference:** NIST AI RMF 1.0 + CSF 2.0 AI Profile (Released Q4 2025)  
> **Scope:** LLM, RAG, Agentic AI, and Autonomous Systems in Enterprise Environments

### Core Focus Areas: **Secure • Detect • Thwart**

| **Focus Area** | **Objective** | **Technical Controls** | **Evidence Artifacts** |
|:---------------|:--------------|:-----------------------|:-----------------------|
| **SECURE** | Harden AI/ML pipelines against adversarial attacks | Model signing (`sigstore`), training data provenance, prompt injection filters | `model_manifest.json`, data lineage DAGs, input validation logs |
| **DETECT** | Identify AI-specific threats (model poisoning, data exfiltration via prompts) | LLM output monitoring, embedding drift detection, RAG retrieval auditing | Prompt/response logs, vector DB query logs, anomaly scores |
| **THWART** | Mitigate AI-enabled attacks (deepfakes, automated exploits) | AI-generated content watermarking, LLM guardrails, rate limiting on generative endpoints | Watermark verification logs, guardrail trigger events, API throttle metrics |

### LLM & RAG-Specific Control Matrix

| **Threat Vector** | **CSF Category Mapping** | **Control Implementation** |
|:------------------|:-------------------------|:---------------------------|
| **Prompt Injection** | PR.DS, DE.CM | Input sanitization layer, system prompt isolation, `[INST]` boundary enforcement |
| **Training Data Poisoning** | ID.RA, PR.DS | Data provenance attestation, differential privacy, training data hash verification |
| **Model Inversion/Extraction** | PR.AA, DE.AE | Query rate limiting, output perturbation, model access logging |
| **RAG Context Manipulation** | PR.DS, DE.CM | Document source verification, retrieval confidence thresholds, context window auditing |
| **Agentic Tool Abuse** | GV.PO, RS.MI | Tool permission boundaries, action approval workflows, execution sandboxing |
| **Hallucination-Induced Risk** | GV.OV, RS.AN | Fact-checking pipelines, confidence scoring, human-in-the-loop for critical outputs |

### AI Risk Quantification

$$\text{AI Risk} = P(\text{Adversarial Success}) \times I(\text{Business Impact}) \times (1 - E(\text{Control Efficacy}))$$

Where:
- $P$ = Probability of successful attack (derived from red team exercises)
- $I$ = Impact score (1-5 scale aligned with enterprise risk taxonomy)
- $E$ = Control efficacy (measured via continuous validation)

---

## 4. Profiles & Gap Analysis — 4-Step Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  STEP 1: SCOPE          STEP 2: CURRENT         STEP 3: TARGET              │
│  ───────────────        ───────────────         ────────────────            │
│  Define boundaries      Assess existing         Define desired              │
│  (BU, system, tier)     control state           outcomes + tier             │
│                                                                             │
│                              ↓                        ↓                     │
│                         ┌─────────────────────────────────┐                 │
│                         │   STEP 4: GAP ANALYSIS          │                 │
│                         │   ────────────────────          │                 │
│                         │   Delta = Target - Current      │                 │
│                         │   Prioritize by risk score      │                 │
│                         │   Map to Informative References │                 │
│                         └─────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Step-by-Step Execution

| **Step** | **Action** | **Output Artifact** | **Key Tools** |
|:---------|:-----------|:--------------------|:--------------|
| **1. Scope Definition** | Identify organizational units, critical systems, regulatory drivers | Scope Document (`scope.md`), System boundary diagrams | CMDB queries, data flow diagrams |
| **2. Current Profile Assessment** | Map existing controls to CSF Categories; document implementation status | Current Profile Matrix (spreadsheet/JSON) | Control questionnaires, automated config audits |
| **3. Target Profile Development** | Define desired outcomes per Category; set target Tier | Target Profile Matrix, Risk tolerance thresholds | Stakeholder workshops, regulatory requirements |
| **4. Gap Analysis & Roadmap** | Calculate delta; prioritize gaps; map remediation to references | Gap Analysis Report, Prioritized Backlog | Risk scoring, reference mapping tables |

### Informative References Mapping

| **CSF 2.0 Category** | **ISO 27001:2022** | **NIST SP 800-53 Rev. 5** | **CIS Controls v8** |
|:---------------------|:-------------------|:--------------------------|:--------------------|
| GV.RM (Risk Mgmt Strategy) | A.5.1, A.5.2 | RA-1, RA-3, PM-9 | Control 1 |
| ID.AM (Asset Mgmt) | A.5.9, A.5.10 | CM-8, PM-5 | Controls 1, 2 |
| PR.AA (Identity & Access) | A.5.15, A.5.16, A.8.2 | AC-1, AC-2, IA-1, IA-2 | Controls 5, 6 |
| PR.DS (Data Security) | A.5.33, A.8.10, A.8.24 | SC-8, SC-13, SC-28 | Control 3 |
| DE.CM (Continuous Monitoring) | A.8.15, A.8.16 | SI-4, AU-6 | Controls 8, 13 |
| RS.MA (Incident Mgmt) | A.5.24, A.5.26 | IR-1, IR-4, IR-6 | Control 17 |
| RC.RP (Recovery Planning) | A.5.29, A.5.30 | CP-1, CP-2, CP-10 | Control 11 |

### Gap Prioritization Formula

$$\text{Priority Score} = \frac{\text{Risk Impact} \times \text{Likelihood}}{\text{Remediation Effort}} \times \text{Regulatory Weight}$$

---

## 5. Quick-Reference: C-SCRM (Cyber Supply Chain Risk Management)

> **Governing Category:** GV.SC (Govern → Supply Chain Risk Management)  
> **Reference:** NIST SP 800-161 Rev. 1

### 5 Critical C-SCRM Outcomes

| **#** | **Outcome** | **Description** | **Implementation Evidence** |
|:------|:------------|:----------------|:----------------------------|
| **1** | **Supplier Risk Tiering** | Classify suppliers by criticality and access level | Vendor tier matrix, `supplier_risk_scores.csv`, third-party access policies |
| **2** | **SBOM Lifecycle Management** | Ingest, validate, and continuously monitor software bills of materials | SBOM repository (`/sbom/`), automated SBOM validation pipelines, VEX documents |
| **3** | **Contractual Security Requirements** | Embed security clauses in procurement contracts | Contract templates with security annexes, right-to-audit clauses, SLA breach logs |
| **4** | **Continuous Supplier Monitoring** | Real-time visibility into supplier security posture | Threat intel integration (e.g., SecurityScorecard API), dark web monitoring alerts |
| **5** | **Incident Notification & Response** | Defined escalation paths for supplier-originated incidents | Supplier incident notification SLAs, joint IR playbooks, communication templates |

### C-SCRM Maturity Indicators

| **Maturity Level** | **Characteristics** | **Technical Evidence** |
|:-------------------|:--------------------|:-----------------------|
| **Basic** | Vendor list exists; ad-hoc assessments | Spreadsheet-based vendor tracking |
| **Developing** | Annual questionnaires; some SBOM collection | Periodic SBOM snapshots, vendor security questionnaire responses |
| **Defined** | Formal C-SCRM policy; automated SBOM ingestion | Policy documents, CI/CD SBOM generation (`syft`, `trivy`) |
| **Managed** | Continuous monitoring; risk-scored vendor dashboard | Real-time vendor risk API integrations, SBOM diff alerts |
| **Optimizing** | **AI-driven supplier risk prediction; automated contract enforcement** | ML risk models, smart contract audit logs, autonomous re-tiering |

---

## 6. Quick Reference Tables

### CSF 2.0 Function Mnemonics

| **Mnemonic** | **Functions** |
|:-------------|:--------------|
| **G-I-P-D-R-R** | **G**overn → **I**dentify → **P**rotect → **D**etect → **R**espond → **R**ecover |

### Risk Calculation Standard

$$\text{Risk} = \text{Likelihood} \times \text{Impact}$$

| **Likelihood** | **Score** | **Impact** | **Score** |
|:---------------|:----------|:-----------|:----------|
| Rare | 1 | Negligible | 1 |
| Unlikely | 2 | Minor | 2 |
| Possible | 3 | Moderate | 3 |
| Likely | 4 | Major | 4 |
| Almost Certain | 5 | Catastrophic | 5 |

**Risk Matrix Output:** $Risk \in [1, 25]$ → Mapped to Low (1-6), Medium (7-14), High (15-25)

---

## 7. 2026 Priority Checklist

- [ ] **AI Governance:** Establish GV.PO policies for LLM/RAG deployments
- [ ] **SBOM Automation:** Integrate SBOM generation into all CI/CD pipelines
- [ ] **Tier 4 Readiness:** Deploy ML-based detection with autonomous response guardrails
- [ ] **C-SCRM Tier 3+:** Implement continuous supplier monitoring with API integrations
- [ ] **AI Profile Alignment:** Complete "Secure • Detect • Thwart" control mapping
- [ ] **Zero Trust + AI:** Enforce microsegmentation for AI model endpoints
- [ ] **Regulatory Mapping:** Align CSF profiles with EU AI Act, SEC Cyber Rules, DORA

---

*Last Updated: January 2026 | Version: 2.0.1*  
*References: NIST CSF 2.0, NIST AI RMF 1.0, SP 800-53 Rev. 5, SP 800-161 Rev. 1, ISO 27001:2022*
