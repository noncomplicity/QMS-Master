Process: [[Clinical Management Process.canvas|Clinical Management Process]]
Requirements: [[]]
Owner: [[Head of Clinical Management]]
Type: #StandardOperatingProcedure 

#### 1. Purpose

The purpose of this document is to provide guidance on the generation of pre-market clinical data for medical device AI. It reflects current progress as it reflects Medical Device AI in local and global best practices, helping stakeholders understand the foundational regulatory landscape, identify applicable standards, determine what clinical data are appropriate, apply suitable methods for generating that data, and integrate the resulting evidence into a coherent validation framework that demonstrates safety, performance, and usability prior to market access.

The relation and interaction of this procedure, its related procedures and the resulting output is visually represented in the [[Clinical Management Process.canvas|Clinical Management Process]]

---

#### 2. Legal and Regulatory Baseline

Across jurisdictions, the regulatory expectation is that clinical evidence must demonstrate that the device—including any AI functionality—achieves its intended purpose safely and effectively in the target population and clinical context.

**EU (MDR 2017/745):** Articles 61 and Annex XIV define clinical evaluation as the ongoing process of assessing clinical data to verify safety, performance, and benefit–risk. Clinical data can be derived from clinical investigations, literature, or experience with equivalent devices. MDCG 2020-1, 2022-5, and 2022-10 clarify that clinical evaluation must include performance verification and validation using representative datasets aligned with the software’s intended purpose.

**US (FDA):** 21 CFR 860 and IMDRF guidance adopted by the FDA (e.g., _SaMD: Clinical Evaluation_, _Good Machine Learning Practice_) require clinical evidence demonstrating reasonable assurance of safety and effectiveness, validated against clinically relevant data. The Predetermined Change Control Plan (PCCP) highlights that pre-market data must justify anticipated model updates.

**IMDRF (SaMD N41, N57):** Defines the three-level evidence model (analytical validation → clinical association → clinical validation), now widely adopted.

---

#### 3. Relevant Standards

- [[EU MDR 2017_745 Article 61]]    
- [[EU MDR 2017_745 Annex XIV Clinical evaluation]]
- [[EU MDR 2017_745 Annex VIII Classification rules]]
- [[EU MDR 2017_745 Annex I General Safety and Performance Requirements]]
- [[ISO 14155_2020 Good Clinical Practice for medical device investigations]]
- [[ISO_TR 20416_2020 Post-market surveillance continuity with pre-market data]]
- [[ISO 13485_2016 QMS integration of clinical investigation processes]]
- [[ISO_IEC 23053_2022 AI system lifecycle processes]]
- [[ISO_IEC 24029-1 and -2 Bias and robustness evaluation in AI]]
- [[IEC 62304 Software lifecycle including verification and validation traceability]]
- [[IEC 62366-1 Usability engineering for medical devices]]
- [[ISO_TR 62366-2 Guidance on the application of usability engineering]]
- [[ISO 14971 Risk management for medical devices]]
- [[ISO 20916_2019 In vitro diagnostic clinical performance studies]]
- [[IMDRF SaMD N41_N57 Framework for clinical evaluation of SaMD]]
- [[FDA 21 CFR 860 Medical Device Classification Procedures]]
- [[FDA SaMD Clinical Evaluation guidance]]
- [[FDA GMLP Good Machine Learning Practice]]
- [[FDA PCCP Predetermined Change Control Plan]]
- [[MDCG 2020-1 Clinical evaluation of medical device software]]
- [[MDCG 2022-5 Guidance on clinical evaluation equivalence and sufficient clinical evidence]]
- [[MDCG 2022-10 Additional clinical evaluation clarification]]    

---

#### 4. Determining Appropriate Clinical Data

This section represents an **instructional activity** within the validation framework. The purpose of this activity is to guide the determination of what constitutes appropriate clinical data for an AI-based medical device prior to market access.

The appropriateness of clinical data depends on **intended purpose**, **risk classification**, and **claim type**. Each sub-area below functions as a distinct decision point, linking to detailed procedural guidance.

- [[Intended Purpose and Context of Use for AI-Based Medical Device Software]] — Define the [[Intended Purpose Statements]], including intended medical purpose, clinical task (diagnosis, triage, monitoring, prediction), target population, care setting, input modalities, users, and claims; map to indications for use and context-of-use requirements (EU MDR Art. 2 & 61; Annex XIV; IMDRF SaMD N41/N57; FDA SaMD Clinical Evaluation guidance).
- [[System risks in AI-Based Medical Device Software]] — Determine device classification and AI risk based on intended purpose, significance of information to the healthcare decision, and state of healthcare situation; link the outcome to regulatory class (EU MDR Annex VIII rules; IMDRF SaMD risk categorization) to scale the depth of clinical evidence and decide whether prospective investigation is required.
- [[Establishment of Evidence Hierarchy for AI Validation]] — Structure evidence across analytical validation → clinical association → clinical validation; pre-specify endpoints, performance metrics (e.g., sensitivity, specificity, AUC, calibration), acceptance criteria, and a statistical analysis plan with power justification (IMDRF SaMD N41/N57; ISO 14155; ISO 20916 for IVD contexts).
- [[Assessment of Generalisability and Representativeness]] — Demonstrate that training/validation data represent the target population and use conditions (geography, demographics, devices, acquisition protocols); include external validation and transportability analyses; pre-specify subgroup analyses and manage data drift risks (EU MDR Annex XIV; FDA GMLP; ISO/IEC 24029-1/-2; ISO/TR 20416 linkage to PMS planning).
- [[Bias and Fairness Evaluation in Clinical Data]] — Plan, execute, and report bias analyses (selection, spectrum, verification, and performance bias), subgroup performance and calibration; implement mitigation (e.g., re-sampling/re-weighting) and document impact on benefit–risk and clinical claims (ISO/IEC 24029-1/-2; FDA GMLP; IMDRF SaMD N41; EU MDR Annex I GSPR on risk/benefit).
- [[Human-AI Interaction and Usability Validation]] — Define user roles, information presentation, and workflow integration; conduct **formative** evaluations to refine design and **summative** usability validation to confirm safe and effective use; link use-related risk analysis to ISO 14971 and document reader-in-the-loop or task-performance studies where applicable (IEC 62366-1; ISO/TR 62366-2; ISO 14971).    

---

#### 5. Techniques for Producing Clinical Data

Each of the following areas represents an **instructional reference** for methods and techniques to generate clinical data in accordance with applicable regulations and standards. The linked instructions describe the detailed methodologies, study design considerations, and regulatory expectations for each approach. Explanations are retained to clarify context and purpose.

- [[Retrospective Clinical Validation Using Archived Datasets]] — Uses archived datasets from real-world environments, ensuring independence between training and validation sets, a locked model configuration, and well-documented data provenance. This approach aligns with IMDRF and MDR expectations for retrospective validation.
- [[Prospective Clinical Investigation Design under ISO 14155]] — Conducted under Good Clinical Practice (ISO 14155). May include single-arm performance validation (AI vs. reference standard), comparative studies (AI vs. clinician or standard of care), or hybrid retrospective-prospective designs. The method selection depends on device risk and intended clinical claims.
- [[Federated and Synthetic Data Generation for AI Validation]] — Applied when privacy laws or data residency constraints prevent data pooling. Techniques must demonstrate statistical equivalence, representativeness, and traceability to clinical reality, ensuring compliance with data governance frameworks.
- [[Simulation and Digital Twin-Based Clinical Evaluation]] — Supports validation in rare-condition or high-risk scenarios. Follows emerging methodologies under ISO/IEC 23053 and IMDRF recommendations, using model-based or in-silico clinical trials to complement empirical data.
- [[Formative and Summative User Interaction Evaluations under IEC 62366]] — As defined in IEC 62366-1 and ISO/TR 62366-2, these evaluations assess user interaction safety and effectiveness. Formative studies guide design iteration, while summative evaluations confirm usability in representative clinical environments.

---

#### 6. Integrating Clinical Data into the Validation Framework

The clinical data shall integrate in the the Validation Framework.

Clinical data generation should be part of the structured validation plan that aligns:

- **Analytical validation (algorithm performance, robustness, bias)** — Anchored in IMDRF SaMD N57 for evidence hierarchy, ISO/IEC 24029-1/-2 for robustness and bias testing, and IEC 62304 for lifecycle traceability between verification and validation. [[MDCG 2020-1 Clinical evaluation of medical device software]] complements these by translating the IMDRF framework into MDR expectations, clarifying how analytical validation connects to clinical evidence generation..
- **Clinical validation (real-world relevance, outcomes)** — Most standards such as [[ISO 14155_2020 Good Clinical Practice for medical device investigations]] and [[IMDRF SaMD N57 Framework for clinical evaluation of SaMD]] primarily address prospective clinical investigations or define general evaluation structures. They do not explicitly describe how retrospective data can serve clinical validation. It should be clarified that retrospective studies can also meet clinical validation requirements if they reproduce real-world clinical settings and endpoints. Appropriate MDCG guidance, such as [[MDCG 2020-1 Clinical evaluation of medical device software]] and [[MDCG 2022-5 Guidance on clinical evaluation equivalence and sufficient clinical evidence]], should be considered to ensure alignment with MDR expectations.
- **Usability validation (interaction, safety, decision integration)** — Guided by IEC 62366-1 and ISO/TR 62366-2 for formative and summative evaluations, integrated with ISO 14971 for use-related risk management and clinical workflow safety.

These elements collectively substantiate the [[Clinical Evaluation Reports]], ensuring the evidence package meets expectations for safety, performance, and usability.