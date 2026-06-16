---
id: 5edb1fc
title: "QMS Change Control Procedure"
version: 2
author: "Jakob"
effective_date: 2026-03-04
type: "StandardOperatingProcedure"
process: "Document and Record Control"
requirements: "Requirements/ISO 13485_2016 - 4.1.4 Requirements/ISO 13485_2016 - 4.2.4 Requirements/ISO 13485_2016 - 7.3.9 Requirements/ISO 13485_2016 - 8.5.6 Requirements/EU MDR_2017_745 - Article 10 Requirements/EU MDR_2017_745 - Annex IX"
owner: "Head of Quality Management"
---
### 1. Purpose

The purpose of this procedure is to ensure that all changes affecting the quality management system (QMS) documentation or technical documentation are identified, evaluated, approved, implemented, and verified in a controlled manner in accordance with ISO 13485:2016 and EU MDR 2017/745 requirements.

### 2. Scope

This procedure applies to all changes within the QMS and technical documentation that may impact product quality, safety, performance, or regulatory compliance. It includes but is not limited to:

- QMS documents, procedures, forms, and records.
- Technical documentation as defined under MDR Annex II and III.
- Design and development documentation and configuration items.
- Supplier and tool documentation relevant to technical documentation integrity. 

### 3. Definitions

- **Change Initiation (CI):** An initiation of a branch to modify a controlled item.
- Change Request (CR): A documented proposal to modify a controlled item.
- **Change Impact Assessment (CIA):** Evaluation of the effects of a proposed change.
- **Change Control Record (CCR):** The complete record of a change, combining the change request, impact assessment, review comments, approvals, and verification evidence maintained within the pull request and repository history.
- **Change Board (CB):** A cross-functional body responsible for evaluating and approving changes.
- **Change Owner:** The person who initiates, executes, and documents the change.

### 4. Responsibilities

- **Change Owner:** Submits the change request, coordinates implementation, ensures verification, and completes all related documentation.
- **Change Board:** Reviews and approves/rejects the change based on impact, risk, and regulatory implications.
- **Quality Assurance (QA):** Verifies compliance with regulatory and QMS requirements and ensures records are maintained.

### 5. Procedure
For detailed work instructions follow:
- [QMS Git Work Instruction — Developer CLI](QMS%20Git%20Work%20Instruction%20—%20Developer%20CLI.md)
- [QMS Git Work Instruction — GitHub Web Interface](QMS%20Git%20Work%20Instruction%20—%20GitHub%20Web%20Interface.md)

#### 5.1 Change Initiation

A change may be initiated by any employee or identified through audits, CAPAs, risk reviews, or management reviews. The Change Owner documents the initiation as a Change Initiation (CI).

Change initiation corresponds to creating a dedicated change branch in the documentation management system. The branch serves as the working area where the change is developed and traceability is maintained until formal change request and review and approval are initiated.

#### 5.2 Change Request

When the work in the change branch is complete, a formal Change Request (CR) is initiated by creating a pull request (or merge request) in the documentation repository. The pull request represents the formal submission of the change for review and approval and serves as the documented CR.

The Change Board and relevant reviewers assess the change within the pull request, including:

- Description and rationale.
- Affected documents or records.
- Reference to associated nonconformity, CAPA, or regulatory requirement.
- Effect on device classification, labeling, or UDI if applicable.
- Need for re-verification, re-validation, or re-submission to regulatory authorities.

The pull request functions as both the Change Request (CR) and the Change Impact Assessment (CIA). The review, discussion, and approvals recorded in the pull request correspond to the formal review and approval process. Approval shall be recorded with signatures and date in the Change Control Record (CCR). Once approved, the change is merged into the main documentation set, ensuring updates are verified, published, and traceable.

#### 5.3 Verification and Closure

The Change Board verifies that the change was implemented as planned and that all required activities were completed. The change is closed only when:

- Implementation is verified.
- All affected documents and systems are updated.
- Training is completed (if applicable).
- Verification/validation evidence is attached to the CCR.

#### 5.4 Post-Implementation Review

For significant or high-risk changes, a post-implementation review shall be conducted to confirm that the change achieved its intended outcome and did not introduce new issues. Any residual risks are documented and managed per the [Risk Management Procedure](Risk%20Management%20Procedure.md).

### 6. Records

All change records shall be maintained per the [Record Control Procedure](Record%20Control%20Procedure.md), including:

- Change Request (CR)
- Change Impact Assessment (CIA)
- Change Control Record (CCR)
- Verification and validation reports
- Meeting minutes and approvals