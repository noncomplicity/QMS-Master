## Introduction

This document describes a structured, two-level approach to risk management for standalone medical device software, aligned with the requirements and principles of **ISO 14971**, **IEC 62304**, and **IEC 82304-1**. The objective of this framework is to establish a cohesive methodology that links high-level safety and clinical risk analysis with detailed causal and design-level risk assessments, providing full traceability from system-level hazards to component-level contributing factors.

---

# 1. Framework Overview

## System Risk Analysis

The first level of risk analysis is conducted at the system or product level and is primarily driven by the principles in **ISO 14971** and **IEC 82304-1**. It addresses the software product as a complete system, analysing its intended use, context of deployment, user interactions, and clinical purpose. The focus is on identifying hazards, hazardous situations, and potential harms that could arise from the use of the software in its operational environment.

System-level analysis is intended to answer questions such as:

- What hazardous situations can occur during use of the software system?
    
- What clinical or patient harms could result from those situations?
    
- What safety characteristics must be defined to mitigate those hazards?
    

This level provides the foundational understanding of the software's risk profile and sets the scope for deeper investigation at the contributing factor level.

  
This connection forms part of the System Risk Analysis and should be interpreted in connection with related procedures and guidance such as:

- [[System risks in AI-Based Medical Device Software]]
- and other complementary process documents that address intended purpose, clinical context, and evidence determination.

## Contributing Factor Analysis

The second level of analysis decomposes the identified hazardous situations into their underlying causes and contributing factors. This activity is closely aligned with **IEC 62304** requirements on software risk control and causal analysis. It focuses on the specific software behaviours, design decisions, or environmental conditions that could lead to the hazardous situations identified in the system-level analysis.

At this level, risk is examined through causal chains and sequences of events, ensuring that each contributing factor is understood, controlled, and verified. This approach creates a direct trace from the clinical consequence level to the implementation detail level.

---

# 2. Core Risk Management Deliverables

The framework is supported by a set of structured documents that together form the risk management file:

### Risk Management Procedure

Defines the overall process, responsibilities, criteria for risk acceptability, and the lifecycle integration of risk management activities.

### Risk Management Plan

Establishes the scope, context, intended use, and specific deliverables for the software product’s risk management activities.

### System Risk Analysis

Documents hazards, hazardous situations, and harms at the software system level. This analysis considers intended use, reasonably foreseeable misuse, user profiles, and the clinical environment.

### Contributing Factor Analysis

Provides a detailed examination of the causal mechanisms behind hazardous situations, identifying software-related, environmental, and process-related factors. This analysis includes the definition of risk control measures and verification activities.

### Risk Management Report

Summarises the outcomes of risk management activities, demonstrates that risk acceptability criteria are met, and provides a consolidated overview of residual risk.

---

# 3. Contributing Factor Categories

Breaking down contributing factors into specific categories increases clarity, traceability, and regulatory confidence. The following categories are recommended:
## Software Contributing Factors

Software design and architecture can be a primary contributing factor where faults or deficiencies in implementation, data handling, logic, or internal communication can lead to hazardous situations. This section examines how software-level behaviours—including algorithmic execution, timing, and control flow—can initiate or propagate risks identified at the system level. Controls in this area typically include software verification, architecture-based segregation, error detection, and safe-state handling.
## User Interface Contributing Factors

User interface design and interaction mechanisms often play a central role in risk realisation. The analysis should assess how presentation errors, ambiguous workflows, or inadequate feedback can lead to hazardous situations. Usability engineering activities in accordance with **IEC 62366-1** should be directly linked to this category.

## Configuration Contributing Factors

Configuration errors are a frequent cause of safety-related incidents in software systems. This category evaluates how misconfigurations, invalid parameter settings, or lack of validation mechanisms could lead to hazardous outcomes. Controls may include validation rules, warnings, safe defaults, and guided configuration workflows.

## Cybersecurity Contributing Factors

Cybersecurity-related events can directly impact patient safety by compromising data integrity, availability, or confidentiality. This analysis examines how security vulnerabilities, unauthorised access, or malicious actions could contribute to hazardous situations. Alignment with **IEC 81001-5-1** and relevant cybersecurity guidance should be demonstrated.

## Additional Contributing Factor Domains

Depending on the product’s architecture and functionality, other contributing factor categories may be included, such as:

- **Algorithmic Contributing Factors** – errors in algorithm logic or decision-making.
- **Data Quality Contributing Factors** – incorrect or incomplete data leading to unsafe outputs.
- **Interoperability Contributing Factors** – failures in data exchange or system integration.

---

# 4. Benefits of the Two-Level Approach

This layered methodology provides several strategic and regulatory benefits:

- **Traceability:** Direct linkage from hazardous situations to contributing factors and risk controls.
    
- **Clarity:** Clear separation of clinical-level risks and implementation-level causes improves reviewer understanding.
    
- **Maintainability:** Easier to update analyses when new features or risk sources emerge.
    
- **Integration:** Facilitates alignment between safety, usability, cybersecurity, and lifecycle processes.