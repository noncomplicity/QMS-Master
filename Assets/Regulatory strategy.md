---
id:
title:
version:
author:
effective_date:
type:
process:
requirements:
owner:
---
**Regulatory Strategy for [Software System Name]**

### 1. Purpose and Scope

The purpose of this document is to define the regulatory strategy for achieving and maintaining compliance of the [Software System Name] under Regulation (EU) 2017/745 on medical devices (MDR) and other applicable regulations. The strategy outlines how the system as a whole — consisting of multiple interconnected software components, modules, or applications — will be evaluated, classified, and managed to ensure conformity with applicable regulatory requirements. It addresses the overall architecture, dependencies, and shared services across the system rather than focusing on an individual product.

The scope covers all lifecycle activities related to the software system, including development, maintenance, integration with clinical workflows, and post-market activities. This strategy serves as a guiding framework for qualification, risk classification, regulatory documentation, and interactions with regulatory authorities.

---

### 2. System Description and Intended Purpose



#### Intended Purpose


#### Users


#### Functional Overview

Core functionalities include:

#### Implementation Context



---

### 3. Modularization and Architecture

The [Software System Name] is composed of distinct yet interoperable modules that together form a cohesive system. Modularization ensures scalability, regulatory clarity, and maintainability by defining explicit boundaries between functionalities. Each module is treated as a separate configuration item with clear interfaces, dependencies, and responsibilities.

#### Modular Structure

- **Core Platform Module:** Provides shared infrastructure, authentication, data management, and communication services.
    
- **Functional Modules:** Implement clinical or analytical features such as decision support, patient monitoring, or data visualization.
    
- **Integration Modules:** Manage data exchange with external systems such as EHRs, laboratory systems, or third-party APIs.
    

#### Regulatory Implications of Modularization

Each module undergoes qualification and classification to determine its regulatory relevance under MDR, GDPR, PDL, and the AI Act. The modular design allows separation of medical and non-medical components, reducing the regulatory scope for non-critical functions and supporting a maintainable compliance structure.

#### Interface Control and Change Management

Interfaces between modules are documented and version-controlled. Changes to shared components are assessed for their impact across modules, ensuring that updates maintain compliance and traceability. Configuration management procedures ensure consistent deployment and verification across environments.

#### Benefits of Modularization

- Enables focused conformity assessment per module.
    
- Supports incremental validation and release cycles.
    
- Reduces re-certification overhead for unaffected components.
    
- Facilitates cybersecurity and data protection segregation.
    

---

### 4. Management of Specific Implementations

The [Software System Name] may be implemented in various clinical contexts, each potentially altering the regulatory landscape depending on how the system is used and the type of data it processes. When a specific implementation involves **processing medical information for medical purposes directed toward an individual**, such use may qualify as a medical device under MDR, High risk AI under the AI act or have other implications related healthcare regulations.

#### Implementation Assessment

For every specific implementation, a **Regulatory Implementation Assessment (RIA)** shall be conducted to determine:

- Whether the implementation introduces medical purposes in software and to what extent.
    
- Whether the implementation introduces processing of medical information and to what extent.
    
- Whether the implementation introduces High risk AI.
    
- Which modules or configurations are involved and how their function aligns with medical device definitions.
    
- The applicable regulatory frameworks (MDR, GDPR, PDL, AI Act) and risk implications.
    

#### Implementation Control and Documentation

Each clinical implementation shall be documented through an **Implementation Control Record (ICR)** capturing:

- Implementation scope and clinical setting.
    
- The modules deployed and their configured purposes.
    
- Data types processed, including personal and patient data.
    
- Links to relevant RARs, RCPs, and conformity evidence.
    

If the implementation introduces or extends medical use, the associated modules must undergo qualification and conformity review to ensure compliance with MDR and related data protection and AI regulations.

#### Responsibilities and Oversight

- **Implementing Organization:** responsible for ensuring the implementation is used in line with the approved intended purpose.
    
- **Manufacturer:** responsible for verifying that configurations remain within the validated and compliant scope.
    
- **Regulatory Lead:** ensures assessments, documentation, and regulatory notifications are updated accordingly.
    

---

### 5. Conformity Assessment Strategy

The conformity assessment will be applied at two levels:

1. **System-Level Conformity:** ensuring that the platform infrastructure, security, and interoperability controls align with MDR and other applicable standards (e.g., IEC 62304, ISO 27001, IEC 82304-1).
    
2. **Module-Level Conformity:** ensuring that each module applies the complete regulatory qualification and classification process, including the relevant conformity assessment route (MDR Annex IX/X/XI or applicable AI Act, GDPR, or PDL requirements).
    

Each module’s conformity assessment integrates the full regulatory evaluation lifecycle: qualification, classification, documentation, verification, validation, and review of risk and performance evidence. This ensures that conformity is maintained at the module level while the system-level controls provide consistent oversight.

A Notified Body will be engaged for **high‑risk AI systems** and **MDR Class IIa or higher** modules. The conformity route and applicable evidence will be defined per module, while shared controls (e.g., QMS, cybersecurity, and risk management) will remain centralized where justified.

---

### 6. Qualification, Classification, and Module Regulatory Planning

The qualification and classification process shall be performed **at the module level** rather than at the overall system level. Each module within the [Software System Name] will undergo a dedicated **Regulatory Assessment** to determine its qualification, classification, and applicable conformity route.

#### Regulatory Assessment

A **Regulatory Assessment Report (RAR)** will be produced for each module, documenting:

- The module’s intended use and user group.
    
- Whether the module qualifies as a medical device under MDR Article 2(1) or falls under other regulatory frameworks (GDPR, PDL, AI Act).
    
- Applicable classification rule(s) (e.g., MDR Annex VIII Rule 11 for software).
    
- Justification of the classification outcome and relevant risk factors.
    
- Determination of applicable standards and regulatory controls.
    

#### Regulatory Conformity Plan

Following the regulatory assessment, a **Regulatory Conformity Plan (RCP)** will be created for each qualified module. The RCP defines the activities necessary to achieve and maintain conformity, including:

- Applicable conformity assessment route (Annex IX/X/XI for MDR, AI Act, or data protection obligations as relevant).
    
- Verification and validation activities.
    
- Required technical documentation and evidence.
    
- Notified body involvement (if applicable).
    
- Cross-dependencies with other modules or system components.
    

#### Inter‑module Dependencies

While each module is assessed independently, interactions between modules are reviewed to identify indirect regulatory implications. Shared functionalities, such as data processing or AI‑driven analysis, may impact classification or conformity requirements and will be documented accordingly.

This modular approach ensures clarity in regulatory responsibilities and facilitates targeted compliance updates without affecting the entire system.

---

### 7. Applicable Regulatory and Standard Requirements

The regulatory framework for the [Software System Name] encompasses four primary domains: **Medical Devices**, **Data Protection**, **Artificial Intelligence**, and **National Medical Informatics (NMI)**. Each area imposes distinct but interrelated obligations that must be harmonized through the system’s governance, design, and documentation.

#### Medical Device Regulations

Applicable under Regulation (EU) 2017/745 (MDR) and, where relevant, Regulation (EU) 2017/746 (IVDR). These govern the qualification, classification, conformity assessment, and lifecycle management of modules that meet the definition of a medical device. Supporting standards include:

- ISO 13485: Quality Management Systems for Medical Devices
    
- ISO 14971: Risk Management for Medical Devices
    
- IEC 62304: Software Lifecycle Processes
    
- IEC 82304-1: Health Software — Product Safety and Lifecycle Requirements
    
- IEC 62366-1: Usability Engineering for Medical Devices
    

#### Data Protection Regulations

The processing of personal and patient data within the software system is subject to:

- Regulation (EU) 2016/679 (GDPR)
    
- The Swedish Patient Data Act (Patientdatalagen, PDL, 2008:355)
    

These frameworks require privacy‑by‑design and security‑by‑design principles, data minimization, lawful basis for processing, and traceable data subject rights management. Compliance is supported by:

- ISO/IEC 27001: Information Security Management Systems
    
- ISO/IEC 27701: Privacy Information Management
    
- NIST and ENISA cybersecurity guidance for health data systems
    

#### Artificial Intelligence Regulations

If the system includes AI‑based functionality that performs automated analysis, prediction, or decision support, it falls under the scope of the forthcoming **EU Artificial Intelligence Act (AI Act)**. The system will:

- Determine whether each AI module qualifies as an AI system under the AI Act.
    
- Classify AI modules according to risk category (e.g., high‑risk medical AI).
    
- Implement conformity assessment, transparency, and human oversight measures as required by the Act.
    
- Integrate AI governance into the QMS, ensuring alignment with MDR and data protection requirements.
    

#### National Medical Informatics (NMI) Regulations

For certain Swedish implementations, the **National Medical Informatics (NMI)** regulatory framework applies to systems or modules used for health data management and interoperability across national healthcare IT infrastructures. Compliance includes:

- Ensuring adherence to Swedish e‑health architecture and data exchange standards.
    
- Aligning with Inera and Swedish e‑Health Agency (E‑hälsomyndigheten) interoperability and security requirements.
    
- Maintaining traceability for data exchange between healthcare providers under national frameworks.
    
- Documenting compliance with national registries and health information exchange protocols.
    

Together, these frameworks establish the compliance architecture for the [Software System Name], ensuring that regulatory, ethical, and technical obligations are consistently met across its modules and shared infrastructure.

---

### 8. Clinical Evidence Strategy

Clinical evidence for the [Software System Name] will be developed and maintained in alignment with its modular architecture and regulatory obligations as defined in Sections 1–7. The strategy ensures that the generation and evaluation of evidence is appropriate to the intended purpose, risk class, and regulatory framework (MDR, AI Act, PDL, GDPR, and NMI) applicable to each module or implementation.

#### Evidence Framework

- **System-Level Evidence:** Demonstrates that shared services such as data handling, interoperability, cybersecurity, and usability perform safely and effectively, supporting all module-level functions.
    
- **Module-Level Evidence:** Each module qualifying as a medical device or high-risk AI system must provide clinical or performance evidence demonstrating compliance with MDR Annex XIV or AI Act Article 43 requirements, as applicable.
    
- **Implementation-Level Evidence:** For site-specific or clinical deployments, implementation assessments (RIAs) and related Implementation Control Records (ICRs) will capture real-world validation and performance confirmation within the intended context of use.
    

#### Evidence Sources

- **Literature and Scientific Evaluation:** Analysis of existing clinical data, scientific literature, and equivalent device performance to support safety and performance claims.
    
- **Clinical and Performance Studies:** Conducted when existing evidence is insufficient, ensuring conformance with ISO 14155 for clinical investigations and AI transparency and validation guidelines where applicable.
    
- **Post-Market Data:** Incorporation of real-world evidence through PMS activities to confirm ongoing performance and benefit–risk balance.
    

#### Documentation

All evidence will be compiled into a **Clinical Evaluation Plan (CEP)** and a **Clinical Evaluation Report (CER)** or equivalent AI/PDL evaluation documentation per module. These will maintain traceability to the Regulatory Assessment Reports (RARs), Regulatory Conformity Plans (RCPs), and applicable standards.

### 9. Risk Management Framework

Risk management will be applied across the system in alignment with ISO 14971 and IEC 62304. A unified risk framework will ensure that both system‑wide and module‑specific risks are captured, controlled, and monitored. The framework will include:

- System‑level hazard identification (e.g., cybersecurity, interoperability, data integrity)
    
- Module‑level risk analysis and control measures
    
- Aggregated benefit‑risk assessment across the system
    
- Continuous monitoring of risks through PMS data and updates
    

---

### 10. Management Systems

The [Software System Name] operates under an integrated management system framework designed to meet the combined compliance requirements of MDR, GDPR, PDL, and the AI Act. This framework aligns several interconnected management systems to ensure consistent governance, traceability, and accountability across all regulatory domains.

#### Quality Management System (QMS)

Compliant with ISO 13485, the QMS governs design control, development, verification, validation, and post‑market processes. It also integrates MDR and IVDR conformity assessment activities, ensuring continuous compliance through internal audits, CAPA, and management review.

#### Information Security Management System (ISMS)

Aligned with ISO/IEC 27001, the ISMS safeguards confidentiality, integrity, and availability of data across the system. It provides the foundation for GDPR and PDL compliance by incorporating risk‑based security measures, access controls, encryption, and incident management procedures.

#### Privacy Information Management System (PIMS)

Based on ISO/IEC 27701, the PIMS extends the ISMS to manage personal and patient data in line with GDPR and PDL. It includes governance for data subject rights, data minimization, consent management, and data protection impact assessments (DPIAs).

#### Artificial Intelligence Management System (AIMS)

Developed in alignment with the AI Act and emerging standards such as ISO/IEC 42001, the AIMS ensures responsible AI lifecycle management. It covers AI risk management, transparency, human oversight, dataset governance, and documentation for conformity assessments of AI‑based modules.

Together, these systems form a harmonized compliance architecture where shared processes—such as risk management, supplier control, configuration management, and internal auditing—support all regulatory obligations while maintaining distinct documentation and accountability per framework.

---

### 11. Technical Documentation and Traceability

Technical documentation will be structured to comply with MDR, AI Act, and associated data protection frameworks. Documentation will demonstrate conformity at both the system and module levels and maintain traceability across the regulatory domains.

#### Documentation Structure

- **System‑Level File:** covers platform architecture, shared components, cybersecurity, usability, AI governance controls, and data management. It includes documentation showing that system‑wide safety, performance, and transparency requirements are met.
    
- **Module‑Level Files:** include intended use, qualification and classification outcomes, risk management, usability, verification and validation results, clinical and performance evaluation (as applicable), and AI‑specific transparency and risk documentation per module (as applicable).
    
- **AI System Documentation:** for AI modules classified under the AI Act, the documentation will include the AI risk management file, technical file in accordance with AI Act Annex IV, human oversight measures, dataset governance, and transparency declarations.
    

#### Traceability and Integration

Traceability will be maintained from high‑level system requirements to individual module documentation, ensuring compliance with MDR Annexes II and III and AI Act technical documentation provisions. The traceability matrix will link requirements, risks, design controls, verification activities, and conformity evidence across MDR, AI Act, GDPR, and PDL obligations.

### 12. Post‑Market Surveillance and Vigilance

Post‑Market Surveillance (PMS) and vigilance activities for the [Software System Name] are designed to provide continuous monitoring of safety, performance, and regulatory compliance across MDR, AI Act, GDPR, PDL, and NMI domains. The PMS framework ensures proactive identification of emerging risks and data‑driven improvement of both system‑level and module‑level functionality.

#### PMS Framework

- **System‑Level Surveillance:** Aggregates data from all modules and implementations to monitor reliability, cybersecurity, interoperability, and AI performance across the entire platform.
    
- **Module‑Level Surveillance:** Each regulated module maintains its own PMS plan and reporting mechanism, ensuring post‑market data supports benefit–risk evaluation, technical documentation updates, and continued conformity.
    
- **Implementation‑Level Surveillance:** Site‑specific monitoring for deployed configurations, capturing clinical performance, AI behavior, and user feedback within the intended context of use.
    

#### AI Act Integration

- **Monitoring of AI Performance and Drift:** Continuous oversight of AI models to detect performance degradation, bias, or data drift.
    
- **Incident Reporting:** Reporting of serious incidents, including AI malfunctions or adverse decisions impacting health outcomes, in accordance with AI Act Article 62 and MDR vigilance requirements.
    
- **Corrective and Preventive Actions (CAPA):** Procedures for rapid mitigation of AI‑related risks, supported by retraining, recalibration, or deactivation of affected models.
    

#### Data Protection and NMI Oversight

PMS data collection and analysis must comply with GDPR and PDL principles for lawful processing, and when applicable, NMI interoperability and data exchange requirements. Feedback loops into the ISMS and PIMS ensure ongoing protection of patient and personal data.

#### Reporting and Feedback

- **PSUR or PMSR:** Periodic updates submitted according to risk class or AI system risk category.
    
- **Trend Analysis:** Identification of recurrent issues across modules, including AI errors or cybersecurity vulnerabilities.
    
- **Communication with Authorities:** Coordinated reporting to Notified Bodies, competent authorities, and relevant national agencies (e.g., E‑hälsomyndigheten for NMI systems).
    

This integrated PMS and vigilance approach ensures that the [Software System Name] continuously meets regulatory, ethical, and performance expectations throughout its lifecycle, maintaining transparency and safety for users, patients, and healthcare providers.---

### 13. Regulatory Governance and Timelines

Regulatory governance for the [Software System Name] ensures that the organization maintains structured oversight of all regulatory activities described in Sections 1–12. Governance activities coordinate system‑level, module‑level, and implementation‑specific responsibilities under MDR, AI Act, GDPR, PDL, and NMI to maintain continuous compliance and accountability.

#### Governance Structure

- **Regulatory Lead:** Oversees the entire regulatory strategy, ensuring that qualification, classification, and conformity processes are consistently applied across modules and implementations.
    
- **System Architect:** Responsible for ensuring that architectural design, modularization, and interoperability meet technical and regulatory requirements.
    
- **Quality and Compliance Manager:** Maintains and coordinates the integrated management systems (QMS, ISMS, PIMS, AIMS) and ensures alignment across MDR, AI Act, and data protection frameworks.
    
- **Clinical and AI Evidence Leads:** Manage ongoing generation and review of clinical and performance evidence, including AI transparency, bias monitoring, and validation documentation.
    
- **Implementation Coordinator:** Oversees implementation‑specific regulatory assessments (RIAs) and ensures proper configuration control and documentation under Section 4.
    
- **Module Owners:** Responsible for maintaining module‑specific RARs, RCPs, technical documentation, and PMS inputs.
    

#### Governance Processes

- **Regulatory Reviews:** Conducted periodically to ensure that all documentation (RARs, RCPs, CEPs, CERs, PMSRs) remains current and aligned with the latest regulatory guidance.
    
- **Change Control:** Ensures that system and module changes undergo documented impact assessment, addressing MDR, AI Act, GDPR, and NMI implications before release.
    
- **Training and Competence:** Staff involved in regulatory, clinical, and AI activities shall maintain up‑to‑date knowledge of applicable frameworks and standards.
    
- **Audits and Management Review:** Scheduled internal audits and management reviews assess compliance effectiveness across management systems, verifying traceability from regulatory requirements to evidence.
    

#### Planning and Timelines

The regulatory roadmap defines key milestones for achieving and maintaining conformity:

1. **Regulatory boundary definition and mapping** – Month 1
    
2. **Qualification and classification per module** – Month 2
    
3. **QMS, ISMS, PIMS, and AIMS implementation** – Month 3
    
4. **Clinical and AI evidence compilation** – Month 4
    
5. **Notified Body and authority engagement** – Month 5–6
    
6. **Post‑market monitoring and AI drift control activation** – Ongoing