---
id:
title:
version:
author:
effective_date:
type: "Index"
process:
requirements:
owner: "Regulatory Manager"
---
_This is a matrix that maps major laws, standards, MDCG guidance, and international frameworks to the types of evidence typically expected for ****safety****, ****performance****, and ****usability**** across the lifecycle. All sections are maintained in full for completeness and ongoing reference._

**Legend**  
S = Safety | P = Performance | U = Usability/HFE | L = Lifecycle & Change control (incl. Predetermined Change Control Plan, (PCCP)) | PM = Post‑market | D = Data governance/privacy | C = Cybersecurity | Artifacts = common dossier items you’d prepare

---

## 1) Core Medical‑Device Regulations & MDCG Guidance

| Instrument / Guidance                | Jurisdiction/Body | Scope & Role                                                                      | S                                                          | P                                                            | U                                       | L                                                     | PM                                               | D                                | C                                     | Typical Evidence Artifacts                                             |
| ------------------------------------ | ----------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ | --------------------------------------- | ----------------------------------------------------- | ------------------------------------------------ | -------------------------------- | ------------------------------------- | ---------------------------------------------------------------------- |
| **EU MDR 2017/745**                  | EU                | Legal backbone for medical devices; GSPR, CE conformity, clinical evaluation, PMS | Risk mgmt aligned to ISO 14971; safety claims tied to GSPR | Analytical/clinical performance via CER; state of the art    | IEC 62366 HFE, usability summative eval | SW lifecycle via IEC 62304; change control; tech docs | PMS/PMCF plan, PSUR, vigilance                   | GDPR alignment for personal data | Cyber requirements via MDCG/standards | GSPR checklist, CER, PMCF plan/report, RMF, HFE/UE report, SW V&V, IFU |
| **EU IVDR 2017/746**                 | EU                | IVD devices incl. AI‑driven diagnostics                                           | Risk mgmt; clinical safety                                 | Scientific validity, analytical & clinical performance (PER) | Usability for IVD workflows             | SW lifecycle; change control                          | PMS/PMCF incl. post‑market performance follow‑up | GDPR                             | Cyber via MDCG/standards              | PER, SV plan, RMF, SW V&V, IFU                                         |
| **MDCG 2019‑16**                     | EU/MDCG           | Cybersecurity for medical devices                                                 | Threat mitigation, secure design                           | —                                                            | —                                       | Secure updates, change mgmt                           | Vulnerability mgmt in PMS                        | GDPR linkage                     | Baseline security controls            | TARA, patch plan, SBOM, risk logs                                      |
| **MDCG 2020‑1 Rev.1**                | EU/MDCG           | Guidance on clinical evaluation (MDR)                                             | Safety endpoints & risk/benefit                            | Clinical evaluation methodology                              | —                                       | Lifecycle evidence integration                        | PMCF link                                        | —                                | —                                     | CER plan/report, equivalence analysis                                  |
| **MDCG 2021‑24**                     | EU/MDCG           | Classification of software as a medical device                                    | Clarifies qualification and classification                 | —                                                            | —                                       | Lifecycle integration                                 | PMS linkage                                      | —                                | —                                     | Classification justification document                                  |
| **MDCG 2022‑5**                      | EU/MDCG           | Guidance on borderline & classification of medical devices                        | Clarifies qualification boundaries                         | —                                                            | —                                       | Integration with GSPR evidence                        | PMS relevance                                    | —                                | —                                     | Classification memo, rationale document                                |
| **MDCG 2023‑1**                      | EU/MDCG           | Guidance on AI and machine learning in medical devices (draft)                    | Safety, transparency, bias mgmt                            | Performance metrics, robustness                              | Human oversight                         | Change control for adaptive AI                        | Continuous monitoring                            | Data representativeness          | Cyber implications                    | AI system description, validation plan                                 |
| **FDA 21 CFR 820 (QMSR)**            | USA/FDA           | Quality system aligned to ISO 13485                                               | Design controls, risk mgmt                                 | V&V, benefit‑risk                                            | Usability per FDA HFE guidance          | Config/change mgmt                                    | Complaint handling, CAPA                         | HIPAA linkage                    | Cyber guidance integration            | DHF/DTM/DMR, validation reports                                        |
| **PMD Act (Japan)**                  | Japan/PMDA        | Device regulation incl. SaMD                                                      | Risk mgmt; bias/robustness                                 | Clinical/performance expectations                            | HFE expectations                        | Lifecycle & change                                    | Re‑examination/post‑market                       | Privacy via APPI                 | Cyber via MHLW                        | Technical file, clinical summary, RMF                                  |
| **UK MHRA SaMD/AI Change Programme** | UK                | Roadmap for SaMD/AI regulation                                                    | Safety, transparency                                       | Generalisability evidence                                    | HFE focus                               | Adaptive AI, PCCP                                     | PMS strengthening                                | UK GDPR                          | Cyber alignment (NCSC, IEC 81001‑5‑1) | Tech file mapping                                                      |
| **Health Canada MLMD Guidance**      | Canada            | ML‑enabled devices incl. PCCP                                                     | Risk controls, bias mitigations                            | Performance across updates                                   | HFE                                     | Predetermined Change Control Plan                     | Vigilance, real‑world performance                | PIPEDA                           | Cyber per IEC 81001‑5‑1               | PCCP, RMF, SW V&V, clinical summary                                    |

---

## 2) Global Convergence for SaMD

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**IMDRF SaMD N12/N23/N41 (+updates)**|IMDRF|Risk categorisation, QMS application, clinical evaluation|Risk‑based safety categorisation|Clinical evaluation framework|Usability implicit via context of use|Lifecycle processes for SaMD|Post‑market principles|Data quality concepts|Security referenced|SaMD categorisation rationale, clinical eval plan/report|

---

## 3) AI Governance & Risk Management

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**EU AI Act**|EU|Horizontal AI obligations; high‑risk AI incl. medical|Risk mgmt system; transparency; bias|Performance, accuracy, robustness|Human oversight|QMS for AI; technical docs; logs|Incident reporting & monitoring|Data governance for training/validation|Security & resilience|AI technical documentation, risk mgmt file, logs|
|**ISO/IEC 42001**|ISO/IEC|AI management system (AIMS)|Governing safety objectives|Performance objectives & KPIs|Human‑AI interaction controls|Processes for model change|Continuous monitoring|Data controls|Security controls|AIMS manual, policies, procedures, audit records|
|**ISO/IEC 23894**|ISO/IEC|AI risk mgmt guidance|Hazard/harms analysis (incl. bias)|Performance/robustness risks|HAI risks|Risk controls across lifecycle|Monitoring plans|Data risk controls|Security risks tied to AI|AI RMF, risk register, control evidence|
|**NIST AI RMF 1.0 (incl. GenAI profiles)**|NIST|Practical risk/governance framework|Safety & trustworthiness characteristics|Measurement & evaluation|Usability/UX risk|Change control|Continuous improvement|Data mgmt|Security & resilience|Risk profiles, measurement plans, evaluation reports|
|**ISO/IEC 23053**|ISO/IEC|Framework for ML‑based AI systems|Safety by design in pipeline|Performance across lifecycle|UX considerations|MLOps processes|Monitoring of drift|Data pipeline controls|Security in ML ops|MLOps design history, pipeline specs|

---

## 4) Software Lifecycle & Risk Management

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**ISO 13485**|ISO|Device QMS|Safety via QMS controls|Process & product validation|Usability integration in design|Change control, config mgmt|CAPA, PMS|Data in QMS records|Security interfaces|QMS procedures, records, internal audits|
|**IEC 62304**|IEC|Medical software lifecycle|Risk‑based SW safety classes|Verification/validation|—|Configuration & release mgmt|Problem resolution|—|Links to cyber stds|SW dev plan, V&V, SOUP, traceability|
|**ISO 14971 / IEC TR 80002‑1**|ISO/IEC|Risk mgmt for devices/software|Hazard analysis incl. AI‑specific harms|Risk acceptability linked to performance|Use‑error linkage|Risk of changes|Post‑market risk review|—|Cyber linkage|RMF, hazard logs, FMEAs|
|**IEC 62366‑1**|IEC|Usability engineering|Use errors and mitigations|Performance impact|Summative testing|UE file maintenance|Field feedback|—|—|UE file, summative test report|

---

## 5) Clinical & Real‑World Evidence

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**ISO 14155:2020**|ISO|GCP for clinical investigations|Participant safety|Clinical endpoints & stats|HFE in protocol where relevant|Protocol amendments|Safety reporting|Data protection|—|Protocol, SAP, CSR|
|**NICE ESF**|NICE (UK)|Evidence standards framework|Safety requirements per tier|Effectiveness & economic evidence|UX & accessibility|Evidence roadmaps|Real‑world monitoring|Data standards|—|Evidence reports, eval plans|

---

## 6) Cybersecurity

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**FDA Premarket Cyber Guidance (2023)**|FDA|Secure‑by‑design, SBOM, threat modeling|Safety‑impacting threats|Performance under attack|—|Secure update processes|Coordinated disclosure|PHI linkage|Core cyber reqs|SBOM, threat model, pen test|
|**IEC 81001‑5‑1**|IEC|Health SW security in lifecycle|Security risk to safety|Resilience testing|—|Integration into dev lifecycle|Vulnerability mgmt|Data protection|Security controls|Security plan, verification evidence|
|**MDCG 2019‑16**|EU/MDCG|Cyber for devices|Safety & performance|—|—|Secure updates|PMS for vulnerabilities|GDPR linkage|Baseline controls|Cyber checklist, patch plan|

---

## 7) Data Protection & Data Sharing

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**EU GDPR**|EU|Lawful basis, DPIA, rights, safeguards|Privacy risk controls|Data quality|Accessibility|Records of processing|Breach notification|DPIA, DPA|Security by design|ROPA, DPIA, consent records|
|**EU Data Act / DGA**|EU|Data access & reuse|Governance & safety|Data portability|—|Contractual controls|—|Data intermediaries|Security & IP safeguards|Data‑sharing agreements|
|**US HIPAA**|US|PHI privacy & security|Minimum necessary|Data quality|—|BAAs|Breach rules|Privacy/security|Safeguards|Policies, logs, BAAs|

---

## 8) PCCPs (Adaptive AI)

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**FDA‑HC‑MHRA PCCP Principles**|Regulators|Common principles for planned model changes|Risk‑based guardrails|Pre‑specified metrics|User impact|Change protocol|RWE triggers|Data mgmt|Secure updates|PCCP, rollback plan|
|**Health Canada PCCP Framework**|Health Canada|PCCP submission & review|As above|As above|As above|Templates & review|Monitoring triggers|Data lineage|Secure deployment|PCCP, logs, monitoring plan|

---

## 9) Ethical & Policy Guidance

|Instrument|Body|Scope & Role|S|P|U|L|PM|D|C|Artifacts|
|---|---|---|---|---|---|---|---|---|---|---|
|**WHO AI in Health**|WHO|Ethics, transparency, accountability|Bias, explainability|Validity across populations|Human oversight|Governance model|Real‑world monitoring|Data equity|Security & resilience|Model cards, impact assessment|
|**OECD AI Principles (2024)**|OECD|International AI policy|Trustworthy AI|Reliability, accuracy|Human oversight|Continuous improvement|Monitoring|Fairness, transparency|Resilience|Policy statements, alignment checklist|

---

> Version note: 0.2 — Maintains all sections in full; added MDCG 2022‑5 and expanded detailed regulatory mappings across all sections.