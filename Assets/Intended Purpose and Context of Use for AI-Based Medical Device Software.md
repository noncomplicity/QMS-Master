---
id: 4168666
title: "Intended Purpose and Context of Use for AI Based Medical Device Software"
version: 4
author: "Jakob"
effective_date: 2026-03-04
type: "StandardOperatingProcedure"
process: "Clinical Management Process"
requirements:
owner: "Head of Clinical Management"
---
## 1. Purpose

The purpose of this procedure is to define a structured approach for determining the **intended purpose** and **context of use** of an AI-based medical device. This determination establishes the foundation for subsequent risk assessment and for defining the level and type of clinical evidence required to demonstrate safety, performance, and clinical validity.

This determination provides the foundation for the assessment of [[Pre-Market Clinical Data for AI-Based Medical Device Software]].

---

## 2. Define Intended Purpose

The intended purpose describes the **medical function** and **intended use** of the AI system as it will be presented to users and regulators. It shall be defined based on the manufacturer’s objective intent and documented in accordance with applicable regulatory frameworks (e.g., MDR Annex VIII, FDA 21 CFR 801.4).

The definition of intended purpose shall include:

- **Medical Function**: Clearly identify whether the AI is intended for **diagnosis**, **prognosis**, **screening**, **prediction**, **monitoring**, or **treatment support**.
- **Target Population**: Specify the patient group or clinical population for which the device is intended (e.g., adults with type 2 diabetes, pediatric cardiology patients).
- **Clinical Setting**: Identify the environment in which the device is intended to be used (e.g., primary care, hospital ward, outpatient clinic, home use).
- **Intended User**: Define the professional or lay user (e.g., physician, nurse, laboratory technician, patient).
- **Type of AI Output**: Specify the form of the output (e.g., classification, prediction, recommendation, alert, visualization).
- **Role of Human Oversight**: Describe whether the AI acts autonomously or under direct clinician supervision.

The output of this step is a **formal [[Intended Purpose Statements]]** that serves as the baseline for regulatory classification and risk analysis.

---

## 3. Assess Significance of Information

The significance of information describes the **impact of the AI output on clinical decision-making**. It determines how the information provided by the AI influences clinical action. The following three levels shall be used (based on IMDRF SaMD N12):

- **Informing Clinical Care** – The AI output provides information to support understanding or decision-making but does not determine clinical action.  
    _Example: AI visualization of retinal images for clinician interpretation._
- **Driving Clinical Management** – The AI output influences or directs clinical decisions or management strategies.  
    _Example: AI risk stratification tool guiding frequency of follow-up._
- **Treating or Diagnosing** – The AI output directly triggers a diagnosis or treatment intervention.  
    _Example: AI system that diagnoses arrhythmias and initiates therapy._

The higher the influence on clinical decision-making, the greater the expected level of risk and evidence required.

The selected level of significance shall be reflected directly in the **[[Intended Purpose Statements]]**, clarifying the AI system’s role in clinical decision-making and establishing a consistent foundation for regulatory and clinical evaluation.

---

## 4. Assess Healthcare Situation

The healthcare situation describes the **clinical condition** that the AI addresses. The condition shall be classified according to **IMDRF SaMD N12** as follows:

- **Critical** – Situations where accurate diagnosis or treatment is essential to prevent death, serious deterioration, or permanent impairment.  
    _Example: Detection of acute myocardial infarction._
- **Serious** – Situations where inaccurate information could lead to moderate deterioration, requiring medical intervention but not immediately life-threatening.  
    _Example: Glycemic control in diabetes management._
- **Non-Serious** – Situations where inaccuracies have low potential for significant harm or reversible effects.  
    _Example: Wellness monitoring or mild symptom tracking._

The combination of the **significance of information** and the **healthcare situation** defines the **IMDRF SaMD risk category** (I–IV), where:

| Healthcare Situation / Significance of Information | Informing | Driving | Treating/Diagnosing |
| -------------------------------------------------- | --------- | ------- | ------------------- |
| **Non-Serious**                                    | I         | II      | II                  |
| **Serious**                                        | II        | III     | III                 |
| **Critical**                                       | III       | III     | IV                  |

The assigned **SaMD risk category** shall be documented in the **[[Intended Purpose Statements]]** or its annex, ensuring that the classification rationale is traceable to the described clinical context.