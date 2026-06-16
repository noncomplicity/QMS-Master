---
id: 4168666
title: "System risks in AI Based Medical Device Software"
version: 3
author: "Jakob"
effective_date: 2026-03-04
type: "StandardOperatingProcedure"
process: "Risk Management Process"
requirements: "ISO 14971 Risk management for medical devices"
owner: "Head of Risk Management"
---
## 1. Purpose

Determine medical device AI risk by classifying the device and defining the required level of clinical evidence. The classification depends on intended purpose, impact on healthcare decisions, and the health condition addressed. The outcome of the determination of the Medical Device AI Risk is a documented determination of the AI system’s inherent clinical risk level, its alignment with regulatory classification, and the corresponding clinical evaluation plan.

This determination provides the foundation for the assessment of [Pre-Market Clinical Data for AI-Based Medical Device Software](Pre-Market%20Clinical%20Data%20for%20AI-Based%20Medical%20Device%20Software.md).

---

## 2. Risk Determination Process

The risk determination process applies **ISO 14971** to identify hazards and hazardous situations relevant to the intended clinical use of the AI system. It establishes the **inherent clinical risk level** of the device to determine the extent of required clinical evidence, without extending into design risk control.

This process is conducted **after** the definition of intended purpose, significance of information, and healthcare condition (defined in companion documents). It supports the identification of **safety-related characteristics** and provides the input for classification and evidence planning.

### Step 1 — Identify Characteristics Related to Safety

Identify characteristics that could influence patient safety within the clinical context of use. This includes how the AI interacts with users, its influence on clinical decision-making, and the degree of human oversight required.

### Step 2 — Identify Hazards and Hazardous Situations

Identify hazards and hazardous situations within the **clinical workflow** where AI is used. Focus on potential **clinical harms** rather than technical failure modes.

Examples include:

- Misinterpretation or misuse of AI output in clinical decisions.
- Delay in intervention caused by over-reliance on AI recommendations.
- Missed detection or inappropriate prioritization of patients.
- Use outside the intended user group or clinical environment.

Each identified hazard should be linked to a **hazardous situation** in the clinical use scenario and to potential **harms** affecting patients, users, or the healthcare process.

### Step 3 — Determine Risk Level for Clinical Evaluation

Use the identified hazards and their potential clinical consequences to determine the **overall risk level** of the AI system in its intended use. The objective is to assess the **clinical relevance** of possible errors or misuse and determine whether the clinical performance must be supported by **prospective** or **retrospective** data.

The output of this process provides the basis for **IMDRF SaMD risk categorization** and subsequent **EU MDR class alignment**. The results shall be documented in the [System Risk Analyses](System%20Risk%20Analyses.md), ensuring that the identified AI-related clinical risks are integrated into the overall [Risk Management Framework for Medical Device Software](Risk%20Management%20Framework%20for%20Medical%20Device%20Software.md).

---

## 3. Align with Regulatory Classification

Following risk identification and IMDRF categorization, align the results with **EU MDR Annex VIII** classification rules, particularly **Rule 11** for software. This ensures regulatory consistency between internal risk determination and external classification. The determined risk category should be mapped to the MDR class based on the clinical impact and condition severity. The resulting classification and justification shall be documented in the [Regulatory Conformity Plans](Regulatory%20Conformity%20Plans.md) as stipulated by the [Regulatory strategy](Regulatory%20strategy.md), ensuring traceability between risk determination, regulatory classification, and conformity assessment planning.

---

## 4. Determine Evidence Level

The determination of the required level of clinical evidence shall be based on the **identified AI risk level**, the **intended clinical function**, and the **credibility of the validation approach**. The focus is to establish the type and depth of evidence needed to demonstrate that the AI system performs safely and effectively in its intended clinical context.

### Criteria for Determining Evidence Requirements

1. **Clinical impact and use scenario:** Assess the consequence of incorrect AI outputs on patient outcomes and clinical decision-making. Higher potential impact requires stronger validation to confirm reliability in real clinical workflows.
2. **Evidence representativeness:** Evaluate whether available data sources (analytical, retrospective, or simulated) adequately represent the intended clinical use population, conditions, and workflows.
3. **Validation method robustness:** Use analytical validation, retrospective studies, and simulated clinical evaluations to demonstrate performance, reliability, and human-AI interaction safety under realistic conditions.
4. **Prospective validation need:** Apply prospective clinical investigations when the clinical workflow, user interaction, or environmental factors cannot be reliably replicated through retrospective or simulated data.
5. **Lifecycle and adaptability:** For adaptive or continuously learning AI systems, combine pre-market validation with a structured post-market performance monitoring plan to ensure sustained safety and effectiveness.

The determination integrates these elements into a documented rationale, recorded in the [Clinical Evaluation Plans](Clinical%20Evaluation%20Plans.md) and reflected in the [Clinical Evaluation Reports](Clinical%20Evaluation%20Reports.md), showing that the selected validation strategy—prospective, retrospective, analytical, or hybrid—is proportionate to the AI system's risk level and sufficient to demonstrate its clinical safety, performance, and usability.