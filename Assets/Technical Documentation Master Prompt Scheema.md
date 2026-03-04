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
## Deliverables Overview and Prompt Canvas

The table below defines all identified technical and regulatory deliverables represented in the module documentation dependency diagram. Each deliverable constitutes an independent documentation artefact with defined upstream and downstream dependencies. The table also defines ownership, authoritative sources, lifecycle state, and the controlled AI prompt associated with each deliverable.

| Deliverable                  | Description / Scope                                                                  | Primary Inputs (Dependencies)                  | Primary Outputs (Used By)                   | Primary Information Holder              | Authoritative Sources                    | Lifecycle State / Version | Prompt (controlled) |
| ---------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------- | --------------------------------------- | ---------------------------------------- | ------------------------- | ------------------- |
| Intended Use                 | Defines the medical purpose, target population, intended users, and use environment. | Regulatory strategy; product definition.       | User Intent; Regulatory Assessment Report.  | Product Management / Regulatory Affairs | Product definition; regulatory strategy. | Draft                     | See prompt below    |
| User Intent                  | Describes what the user intends to achieve when using the system.                    | Intended Use.                                  | Core Functionalities.                       | Human Factors / Product Management      | Intended Use statement.                  | Draft                     | See prompt below    |
| Core Functionalities         | High-level system capabilities needed to fulfill user intent.                        | User Intent.                                   | Task Analysis; Risk Analysis; Requirements. | System Engineering                      | Product requirements; system concept.    | Draft                     | See prompt below    |
| Task Analysis                | User tasks and task sequences for intended use.                                      | Core Functionalities.                          | UI Specification; Usability Plans.          | Human Factors Engineering               | Use scenarios; user profiles.            | Draft                     | See prompt below    |
| User Interface Specification | UI elements, navigation, and interaction logic.                                      | Task Analysis.                                 | Usability Evaluation Reports.               | UX / UI Design                          | Design system; UI assets.                | Draft                     | See prompt below    |
| Formative Evaluation Plan    | Plan for formative usability evaluation activities.                                  | Task Analysis; UI Specification.               | Formative Evaluation Report.                | Human Factors Engineering               | Usability engineering procedure.         | Draft                     | See prompt below    |
| Formative Evaluation Report  | Results of formative usability evaluations.                                          | Formative Evaluation Plan; UI Specification.   | Design updates; Risk Analysis updates.      | Human Factors Engineering               | Test records; observation logs.          | Draft                     | See prompt below    |
| Summative Evaluation Plan    | Plan for summative usability validation.                                             | Task Analysis; UI Specification.               | Summative Evaluation Report.                | Human Factors Engineering               | Usability engineering procedure.         | Draft                     | See prompt below    |
| Summative Evaluation Report  | Evidence of usability validation.                                                    | Summative Evaluation Plan; UI Specification.   | Regulatory Assessment Report.               | Human Factors Engineering               | Validation records.                      | Draft                     | See prompt below    |
| Regulatory Assessment Report | Regulatory conformity assessment of usability and use-related risk.                  | Intended Use; Summative Evaluation Report.     | Management Review; Market Access.           | Regulatory Affairs                      | MDR checklist; GSPR mapping.             | Draft                     | See prompt below    |
| Risk Analysis                | Identification of hazards and hazardous situations.                                  | Core Functionalities; Intended Use.            | Failure Sequences; Requirements.            | Risk Management                         | Risk management file.                    | Draft                     | See prompt below    |
| Failure contributing to risk | Sequences of events leading to hazardous situations.                                 | Risk Analysis.                                 | Requirements; Software Design.              | Risk Management                         | Hazard analysis tables.                  | Draft                     | See prompt below    |
| Requirement                  | Verifiable system/software requirements including risk controls.                     | Risk Analysis; Failure sequences; Jira issues. | Verification; Validation.                   | System / Software Engineering           | Requirements repository.                 | Draft                     | See prompt below    |
| Architecture Documentation   | System and software architectural design.                                            | Requirements.                                  | Software Detailed Design.                   | Software Architecture                   | Architecture diagrams; ADRs.             | Draft                     | See prompt below    |
| Software Detailed Design     | Detailed software design elements and interfaces.                                    | Architecture Documentation; Failure sequences. | Software Service.                           | Software Engineering                    | Design specifications.                   | Draft                     | See prompt below    |
| Software Service             | Implemented software unit or service.                                                | Software Detailed Design.                      | Verification; Data Analysis Plan.           | Software Engineering                    | Source code repository.                  | Draft                     | See prompt below    |
| Code Review                  | Static verification of implementation.                                               | Software Service.                              | Verification evidence.                      | Software Engineering                    | Review records; PRs.                     | Draft                     | See prompt below    |
| Unit Test                    | Verification of individual software units.                                           | Software Service.                              | Verification evidence.                      | Software Engineering                    | Test code; reports.                      | Draft                     | See prompt below    |
| System Test                  | Verification of integrated system behavior.                                          | Software Service; Requirements.                | Validation basis.                           | Verification / QA                       | System test reports.                     | Draft                     | See prompt below    |
| Validation                   | Confirmation of fulfillment of user needs and intended use.                          | System Tests; Requirements.                    | Regulatory evidence.                        | Quality / Regulatory Affairs            | Validation protocol; report.             | Draft                     | See prompt below    |
| Data Analysis Plan           | Plan for operational data monitoring and analysis.                                   | Requirements; Software Service.                | Statistics.                                 | PMS / Clinical Affairs                  | PMS plan; monitoring strategy.           | Draft                     | See prompt below    |
| Statistics                   | Statistical outputs supporting PMS and risk review.                                  | Data Analysis Plan.                            | PMS inputs; Risk updates.                   | PMS / Data Analytics                    | Operational datasets.                    | Draft                     | See prompt below    |
| Jira Checklist               | Process-enforcing checklist for Jira.                                                | QMS procedures.                                | Jira Issues.                                | Quality Management                      | QMS procedures.                          | Draft                     | See prompt below    |
| Jira Issue                   | Controlled work item implementing requirements or risk controls.                     | Jira Checklist.                                | Implementation evidence.                    | Project / Engineering                   | Jira system.                             | Draft                     | See prompt below    |

## Controlled Prompts per Deliverable

### Intended Use

You are a regulatory and clinical affairs specialist operating under EU MDR 2017/745. Using only the authoritative sources listed for Intended Use, generate a clear and bounded Intended Use statement. The output shall define medical purpose, target population, intended users, use environment, and clinical context. Do not introduce design solutions, performance claims, or risk controls. Ensure terminology is consistent with MDR Annex I and suitable for notified body review.

### User Intent

You are a human factors engineer under IEC 62366-1. Based strictly on the Intended Use, describe the user intent in operational terms, focusing on what users aim to accomplish, not how the system is designed. Avoid UI descriptions and implementation details. Ensure traceability to the Intended Use.

### Core Functionalities

You are a system engineer under IEC 62304. Using the User Intent as sole driver, describe the core system functionalities required to fulfill that intent. Do not derive requirements yet. The output shall remain solution-agnostic and suitable as input to task analysis and risk analysis.

### Task Analysis

You are a usability engineer under IEC 62366-1. Based on Core Functionalities, perform a task analysis describing user tasks, task sequences, and critical interactions. Focus on normal and reasonably foreseeable use. Do not assess risk or usability yet. Structure the output to support usability evaluation planning.

### User Interface Specification

You are a usability and UI specification author. Using the Task Analysis, specify UI elements, navigation, interaction logic, and constraints at a level sufficient for usability evaluation. Do not justify design choices or assess usability. Ensure traceability to tasks.

### Formative Evaluation Plan

You are a usability engineer. Based on Task Analysis and UI Specification, define a formative evaluation plan including objectives, methods, participants, scenarios, and acceptance criteria. The plan shall be suitable for iterative design improvement and compliant with IEC 62366-1.

### Formative Evaluation Report

You are a usability engineer documenting evidence. Using the executed Formative Evaluation Plan, summarize methods, observations, identified use-related issues, and design feedback. Do not claim validation or risk acceptability. Ensure findings can feed back into risk analysis.

### Summative Evaluation Plan

You are a usability validation planner. Based on Task Analysis and UI Specification, define a summative evaluation plan demonstrating that use-related risks are adequately controlled. The plan shall align with IEC 62366-1 and MDR expectations.

### Summative Evaluation Report

You are documenting usability validation evidence. Using the Summative Evaluation Plan and results, demonstrate that the user interface supports safe and effective use for the intended users. Do not introduce new risks or design changes.

### Regulatory Assessment Report

You are a regulatory specialist. Based on Intended Use and Summative Evaluation Report, assess conformity with applicable regulatory requirements related to usability and use-related risk. The output shall support management review and notified body assessment.

### Risk Analysis

You are a risk management specialist under ISO 14971. Using Core Functionalities and Intended Use, identify hazards, hazardous situations, and initial risk estimates. Do not yet define risk controls. Ensure terminology and structure follow ISO 14971.

### Failure Contributing to Risk (Sequence of Events)

You are a safety analyst. Based on the Risk Analysis, describe failure modes and sequences of events leading to hazardous situations. The output shall support derivation of risk control requirements.

### Requirement

You are a system/software requirements engineer. Using Risk Analysis, failure sequences, and referenced Jira issues, define clear, verifiable requirements including risk control requirements. Each requirement shall be traceable and testable.

### Architecture Documentation

You are a software architect. Based on the Requirements, describe the system and software architecture, including decomposition and interfaces. Do not redefine requirements or risks.

### Software Detailed Design

You are a software designer. Using Architecture Documentation and failure considerations, define detailed design elements sufficient for implementation and verification. Maintain traceability to architecture and requirements.

### Software Service

You are documenting implemented software. Describe the implemented software service strictly based on Software Detailed Design and source code. Do not restate requirements or risks.

### Code Review

You are performing static verification. Based on the Software Service implementation, document code review objectives, scope, findings, and conclusions. Focus on compliance with design and coding standards.

### Unit Test

You are a verification engineer. Using the Software Service, document unit test cases, execution results, and pass/fail criteria demonstrating requirement coverage.

### System Test

You are a system verification engineer. Based on integrated Software Services and Requirements, document system test execution and results supporting validation readiness.

### Validation

You are a validation engineer. Using System Test results and Requirements, confirm that the system fulfills user needs and intended use. The output shall support regulatory submission.

### Data Analysis Plan

You are a PMS and monitoring specialist. Based on Requirements and Software Service behavior, define a data analysis plan describing metrics, data sources, and analysis methods.

### Statistics

You are generating monitoring outputs. Using the Data Analysis Plan, produce statistical results suitable for PMS and risk review. Do not reinterpret requirements.

### Jira Checklist

You are defining process control mechanisms. Based on QMS procedures, define a Jira checklist enforcing required process steps and evidence capture.

### Jira Issue

You are documenting a controlled work item. Using the Jira Checklist, describe the Jira issue content required to implement or verify requirements or risk controls with full traceability.
