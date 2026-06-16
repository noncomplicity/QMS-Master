# IEC 62304:2006+A1:2015 - Medical Device Software Life Cycle Processes

**Normative Requirements Only**

This document contains the normative requirements (Clauses 4-9) from IEC 62304:2006+A1:2015. Informative annexes, forewords, and introductory material have been removed.

---

## 3. Terms and Definitions

Key terms used throughout:

- **ACTIVITY** - a set of one or more interrelated or interacting TASKS
- **ANOMALY** - any condition that deviates from the expected based on requirements specifications, design documents, standards, etc.
- **ARCHITECTURE** - organizational structure of a SYSTEM or component
- **CHANGE REQUEST** - a documented specification of a change to be made to a MEDICAL DEVICE SOFTWARE
- **CONFIGURATION ITEM** - entity that can be uniquely identified at a given reference point
- **DELIVERABLE** - required result or output (includes documentation) of an ACTIVITY or TASK
- **EVALUATION** - a systematic determination of the extent to which an entity meets its specified criteria
- **HARM** - physical injury, damage, or both to the health of people or damage to property or the environment
- **HAZARD** - potential source of HARM
- **HAZARDOUS SITUATION** - circumstance in which people, property or the environment are exposed to one or more HAZARD(S)
- **LEGACY SOFTWARE** - MEDICAL DEVICE SOFTWARE which was legally placed on the market and is still marketed today but for which there is insufficient objective evidence that it was developed in compliance with the current version of this standard
- **MANUFACTURER** - natural or legal person with responsibility for designing, manufacturing, packaging, or labelling a MEDICAL DEVICE
- **MEDICAL DEVICE** - any instrument, apparatus, implement, machine, appliance, implant, software, material intended for diagnosis, prevention, monitoring, treatment or alleviation of disease, etc.
- **MEDICAL DEVICE SOFTWARE** - SOFTWARE SYSTEM that has been developed for the purpose of being incorporated into the MEDICAL DEVICE being developed or that is intended for use as a MEDICAL DEVICE
- **PROBLEM REPORT** - a record of actual or potential behaviour of a MEDICAL DEVICE SOFTWARE that a user or other interested person believes to be unsafe, inappropriate for the intended use or contrary to specification
- **PROCESS** - a set of interrelated or interacting ACTIVITIES that transform inputs into outputs
- **REGRESSION TESTING** - the testing required to determine that a change to a SYSTEM component has not adversely affected functionality, reliability or performance
- **RELEASE** - particular VERSION of a CONFIGURATION ITEM that is made available for a specific purpose
- **RESIDUAL RISK** - RISK remaining after RISK CONTROL measures have been taken
- **RISK** - combination of the probability of occurrence of HARM and the severity of that HARM
- **RISK ANALYSIS** - systematic use of available information to identify HAZARDS and to estimate the RISK
- **RISK CONTROL** - PROCESS in which decisions are made and RISKS are reduced to, or maintained within, specified levels
- **RISK ESTIMATION** - PROCESS used to assign values to the probability of occurrence of HARM and the severity of that HARM
- **RISK EVALUATION** - PROCESS of comparing the estimated RISK against given RISK criteria to determine the acceptability of the RISK
- **RISK MANAGEMENT** - systematic application of management policies, procedures, and practices to the TASKS of analyzing, evaluating, and controlling RISK
- **RISK MANAGEMENT FILE** - set of records and other documents that are produced by a RISK MANAGEMENT PROCESS
- **SAFETY** - freedom from unacceptable RISK
- **SECURITY** - protection of information and data so that unauthorized persons or systems cannot read or modify them and authorized persons or systems are not denied access to them
- **SERIOUS INJURY** - injury or illness that is life threatening, results in permanent impairment, or necessitates medical or surgical intervention to prevent permanent impairment
- **SOFTWARE DEVELOPMENT LIFE CYCLE MODEL** - conceptual structure spanning the life of the software from definition of its requirements to its release
- **SOFTWARE ITEM** - any identifiable part of a computer program (source code, object code, control code, control data, or a collection of these items)
- **SOFTWARE SYSTEM** - integrated collection of SOFTWARE ITEMS organized to accomplish a specific function or set of functions
- **SOFTWARE UNIT** - SOFTWARE ITEM that is not subdivided into other items
- **SOUP** - software of unknown provenance; SOFTWARE ITEM that is already developed and generally available and that has not been developed for the purpose of being incorporated into the MEDICAL DEVICE (also known as "off-the-shelf software")
- **SYSTEM** - integrated composite consisting of one or more of the PROCESSES, hardware, software, facilities, and people
- **TASK** - a single piece of work that needs to be done
- **TRACEABILITY** - degree to which a relationship can be established between two or more products of the development PROCESS
- **VERIFICATION** - confirmation through provision of objective evidence that specified requirements have been fulfilled
- **VERSION** - identified instance of a CONFIGURATION ITEM

---

## 4. General Requirements

### 4.1 Quality Management System

The MANUFACTURER of MEDICAL DEVICE SOFTWARE shall demonstrate the ability to provide MEDICAL DEVICE SOFTWARE that consistently meets customer requirements and applicable regulatory requirements.

*NOTE: Demonstration can be by use of a quality management system that complies with ISO 13485, a national quality management system standard, or a quality management system required by national regulation.*

### 4.2 Risk Management

The MANUFACTURER shall apply a RISK MANAGEMENT PROCESS complying with ISO 14971.

### 4.3 Software Safety Classification

a) The MANUFACTURER shall assign to each SOFTWARE SYSTEM a software safety class (A, B, or C) according to the RISK of HARM to the patient, operator, or other people resulting from a HAZARDOUS SITUATION to which the SOFTWARE SYSTEM can contribute in a worst-case-scenario.

**Class A**: The SOFTWARE SYSTEM cannot contribute to a HAZARDOUS SITUATION; or can contribute to a HAZARDOUS SITUATION which does not result in unacceptable RISK after consideration of RISK CONTROL measures external to the SOFTWARE SYSTEM.

**Class B**: The SOFTWARE SYSTEM can contribute to a HAZARDOUS SITUATION which results in unacceptable RISK after consideration of RISK CONTROL measures external to the SOFTWARE SYSTEM and the resulting possible HARM is non-SERIOUS INJURY.

**Class C**: The SOFTWARE SYSTEM can contribute to a HAZARDOUS SITUATION which results in unacceptable RISK after consideration of RISK CONTROL measures external to the SOFTWARE SYSTEM and the resulting possible HARM is death or SERIOUS INJURY.

In determining the software safety classification:
- Probability of a software failure shall be assumed to be 1
- Only RISK CONTROL measures not implemented within (external to) the SOFTWARE SYSTEM shall be considered

c) The MANUFACTURER shall document the software safety class assigned to each SOFTWARE SYSTEM in the RISK MANAGEMENT FILE.

d) When a SOFTWARE SYSTEM is decomposed into SOFTWARE ITEMS, such SOFTWARE ITEMS shall inherit the software safety classification of the original SOFTWARE ITEM (or SOFTWARE SYSTEM) unless the MANUFACTURER documents a rationale for classification into a different software safety class.

e) The MANUFACTURER shall document the software safety class of each SOFTWARE ITEM if that class is different from the class of the SOFTWARE ITEM from which it was created by decomposition.

f) For compliance with this standard, when applying this standard to a group of SOFTWARE ITEMS, the MANUFACTURER shall use the PROCESSES and TASKS which are required by the classification of the highest-classified SOFTWARE ITEM in the group unless the MANUFACTURER documents in the RISK MANAGEMENT FILE a rationale for using a lower classification.

g) For each SOFTWARE SYSTEM, until a software safety class is assigned, Class C requirements shall apply.

### 4.4 Legacy Software

#### 4.4.1 General

As an alternative to applying Clauses 5 through 9, compliance of LEGACY SOFTWARE may be demonstrated as indicated in 4.4.2 to 4.4.5.

#### 4.4.2 Risk Management Activities

In accordance with 4.2, the MANUFACTURER shall:

a) assess any feedback, including post-production information, on LEGACY SOFTWARE regarding incidents and/or near incidents;

b) perform RISK MANAGEMENT ACTIVITIES associated with continued use of the LEGACY SOFTWARE, considering:
   - integration of the LEGACY SOFTWARE in the overall MEDICAL DEVICE architecture
   - continuing validity of RISK CONTROL measures implemented as part of the LEGACY SOFTWARE
   - identification of HAZARDOUS SITUATIONS associated with continued use
   - identification of potential causes of the LEGACY SOFTWARE contributing to a HAZARDOUS SITUATION
   - definition of RISK CONTROL measures for each potential cause

#### 4.4.3 Gap Analysis

Based on the software safety class, the MANUFACTURER shall perform a gap analysis of available DELIVERABLES against those required according to 5.2, 5.3, 5.7, and Clause 7.

c) The MANUFACTURER shall assess the continuing validity of available DELIVERABLES.

d) Where gaps are identified, the MANUFACTURER shall EVALUATE the potential reduction in RISK resulting from the generation of the missing DELIVERABLES.

e) The MANUFACTURER shall determine the DELIVERABLES to be created. The minimum DELIVERABLE shall be SOFTWARE SYSTEM test records (see 5.7.5).

#### 4.4.4 Gap Closure Activities

a) The MANUFACTURER shall establish and execute a plan to generate the identified DELIVERABLES.

b) The plan shall address the use of the problem resolution PROCESS for handling problems detected in the LEGACY SOFTWARE.

c) Changes to the LEGACY SOFTWARE shall be performed in accordance with Clause 6.

#### 4.4.5 Rationale for Use of Legacy Software

The MANUFACTURER shall document the VERSION of the LEGACY SOFTWARE together with a rationale for the continued use based on the outputs of 4.4.

---

## 5. Software Development Process

### 5.1 Software Development Planning

#### 5.1.1 Software Development Plan

The MANUFACTURER shall establish a software development plan for conducting the ACTIVITIES of the software development PROCESS. The SOFTWARE DEVELOPMENT LIFE CYCLE MODEL shall either be fully defined or be referenced in the plan. The plan shall address:

a) the PROCESSES to be used in the development of the SOFTWARE SYSTEM;

b) the DELIVERABLES (includes documentation) of the ACTIVITIES and TASKS;

c) TRACEABILITY between SYSTEM requirements, software requirements, SOFTWARE SYSTEM test, and RISK CONTROL measures implemented in software;

d) software configuration and change management, including SOUP CONFIGURATION ITEMS and software used to support development; and

e) software problem resolution for handling problems detected in the MEDICAL DEVICE SOFTWARE, DELIVERABLES and ACTIVITIES at each stage of the life cycle.

**[Class A, B, C]**

#### 5.1.2 Keep Software Development Plan Updated

The MANUFACTURER shall update the plan as development proceeds as appropriate.

**[Class A, B, C]**

#### 5.1.3 Software Development Plan Reference to System Design and Development

a) As inputs for software development, SYSTEM requirements shall be referenced in the software development plan.

b) The MANUFACTURER shall include or reference procedures for coordinating the software development with the system development necessary to satisfy 4.1.

**[Class A, B, C]**

#### 5.1.4 Software Development Standards, Methods and Tools Planning

The MANUFACTURER shall include or reference in the software development plan:

a) standards,
b) methods, and
c) tools

associated with the development of SOFTWARE ITEMS of class C.

**[Class C]**

#### 5.1.5 Software Integration and Integration Testing Planning

The MANUFACTURER shall include or reference in the software development plan, a plan to integrate the SOFTWARE ITEMS (including SOUP) and perform testing during integration.

**[Class B, C]**

#### 5.1.6 Software Verification Planning

The MANUFACTURER shall include or reference in the software development plan the following VERIFICATION information:

a) DELIVERABLES requiring VERIFICATION;
b) the required VERIFICATION TASKS for each life cycle ACTIVITY;
c) milestones at which the DELIVERABLES are VERIFIED; and
d) the acceptance criteria for VERIFICATION of the DELIVERABLES.

**[Class A, B, C]**

#### 5.1.7 Software Risk Management Planning

The MANUFACTURER shall include or reference in the software development plan, a plan to conduct the ACTIVITIES and TASKS of the software RISK MANAGEMENT PROCESS, including the management of RISKS relating to SOUP.

**[Class A, B, C]**

#### 5.1.8 Documentation Planning

The MANUFACTURER shall include or reference in the software development plan information about the documents to be produced during the software development life cycle. For each identified document:

a) title, name or naming convention;
b) purpose;
d) procedures and responsibilities for development, review, approval and modification.

**[Class A, B, C]**

#### 5.1.9 Software Configuration Management Planning

The MANUFACTURER shall include or reference software configuration management information in the software development plan:

a) the classes, types, categories or lists of items to be controlled;
b) the software configuration management ACTIVITIES and TASKS;
c) the organization(s) responsible for performing software configuration management ACTIVITIES;
d) their relationship with other organizations, such as software development or maintenance;
e) when the items are to be placed under configuration control; and
f) when the problem resolution PROCESS is to be used.

**[Class A, B, C]**

#### 5.1.10 Supporting Items to be Controlled

The items to be controlled shall include tools, items or settings, used to develop the MEDICAL DEVICE SOFTWARE, which could impact the MEDICAL DEVICE SOFTWARE.

**[Class B, C]**

#### 5.1.11 Software Configuration Item Control Before Verification

The MANUFACTURER shall plan to place CONFIGURATION ITEMS under configuration management control before they are VERIFIED.

**[Class B, C]**

#### 5.1.12 Identification and Avoidance of Common Software Defects

The MANUFACTURER shall include or reference in the software development plan a procedure for:

g) identifying categories of defects that may be introduced based on the selected programming technology; and

h) documenting evidence that demonstrates that these defects do not contribute to unacceptable RISK.

**[Class B, C]**

### 5.2 Software Requirements Analysis

#### 5.2.1 Define and Document Software Requirements from System Requirements

For each SOFTWARE SYSTEM of the MEDICAL DEVICE, the MANUFACTURER shall define and document SOFTWARE SYSTEM requirements from the SYSTEM level requirements.

**[Class A, B, C]**

#### 5.2.2 Software Requirements Content

As appropriate to the MEDICAL DEVICE SOFTWARE, the MANUFACTURER shall include in the software requirements:

a) functional and capability requirements (e.g., performance, timing, physical characteristics, computing environment, compatibility);

b) SOFTWARE SYSTEM inputs and outputs (e.g., data characteristics, ranges, limits, defaults);

c) interfaces between the SOFTWARE SYSTEM and other SYSTEMS;

d) software-driven alarms, warnings, and operator messages;

e) SECURITY requirements (e.g., sensitive information protection, authentication, authorization, audit trail, communication integrity, malware protection);

f) user interface requirements implemented by software;

g) data definition and database requirements;

h) installation and acceptance requirements;

i) requirements related to methods of operation and maintenance;

j) requirements related to IT-network aspects;

k) user maintenance requirements; and

l) regulatory requirements.

**[Class A, B, C]**

#### 5.2.3 Include Risk Control Measures in Software Requirements

The MANUFACTURER shall include RISK CONTROL measures implemented in software in the requirements as appropriate.

**[Class B, C]**

#### 5.2.4 Re-evaluate Medical Device Risk Analysis

The MANUFACTURER shall re-EVALUATE the MEDICAL DEVICE RISK ANALYSIS when software requirements are established and update it as appropriate.

**[Class A, B, C]**

#### 5.2.5 Update Requirements

The MANUFACTURER shall ensure that existing requirements, including SYSTEM requirements, are re-EVALUATED and updated as appropriate as a result of the software requirements analysis ACTIVITY.

**[Class A, B, C]**

#### 5.2.6 Verify Software Requirements

The MANUFACTURER shall verify and document that the software requirements:

a) implement SYSTEM requirements including those relating to RISK CONTROL;
b) do not contradict one another;
c) are expressed in terms that avoid ambiguity;
d) are stated in terms that permit establishment of test criteria and performance of tests;
e) can be uniquely identified; and
f) are traceable to SYSTEM requirements or other source.

**[Class A, B, C]**

### 5.3 Software Architectural Design

#### 5.3.1 Transform Software Requirements into an Architecture

The MANUFACTURER shall transform the requirements for the MEDICAL DEVICE SOFTWARE into a documented ARCHITECTURE that describes the software's structure and identifies the SOFTWARE ITEMS.

**[Class B, C]**

#### 5.3.2 Develop an Architecture for the Interfaces of Software Items

The MANUFACTURER shall develop and document an ARCHITECTURE for the interfaces between the SOFTWARE ITEMS and the components external to the SOFTWARE ITEMS (both software and hardware), and between the SOFTWARE ITEMS.

**[Class B, C]**

#### 5.3.3 Specify Functional and Performance Requirements of SOUP Item

If a SOFTWARE ITEM is identified as SOUP, the MANUFACTURER shall specify functional and performance requirements for the SOUP item that are necessary for its intended use.

**[Class B, C]**

#### 5.3.4 Specify System Hardware and Software Required by SOUP Item

If a SOFTWARE ITEM is identified as SOUP, the MANUFACTURER shall specify the SYSTEM hardware and software necessary to support the proper operation of the SOUP item.

**[Class B, C]**

#### 5.3.5 Identify Segregation Necessary for Risk Control

The MANUFACTURER shall identify any segregation between SOFTWARE ITEMS that is necessary for RISK CONTROL, and state how to ensure that such segregation is effective.

**[Class C]**

#### 5.3.6 Verify Software Architecture

The MANUFACTURER shall verify and document that:

a) the ARCHITECTURE of the software implements SYSTEM and software requirements including those relating to RISK CONTROL;

b) the software ARCHITECTURE is able to support interfaces between SOFTWARE ITEMS and between SOFTWARE ITEMS and hardware; and

c) the MEDICAL DEVICE ARCHITECTURE supports proper operation of any SOUP items.

**[Class B, C]**

### 5.4 Software Detailed Design

#### 5.4.1 Subdivide Software into Software Units

The MANUFACTURER shall subdivide the software until it is represented by SOFTWARE UNITS.

**[Class B, C]**

#### 5.4.2 Develop Detailed Design for Each Software Unit

The MANUFACTURER shall document a design with enough detail to allow correct implementation of each SOFTWARE UNIT.

**[Class C]**

#### 5.4.3 Develop Detailed Design for Interfaces

The MANUFACTURER shall document a design for any interfaces between the SOFTWARE UNIT and external components (hardware or software), as well as any interfaces between SOFTWARE UNITS, detailed enough to implement each SOFTWARE UNIT and its interfaces correctly.

**[Class C]**

#### 5.4.4 Verify Detailed Design

The MANUFACTURER shall verify and document that the software detailed design:

a) implements the software ARCHITECTURE; and
b) is free from contradiction with the software ARCHITECTURE.

**[Class C]**

### 5.5 Software Unit Implementation

#### 5.5.1 Implement Each Software Unit

The MANUFACTURER shall implement each SOFTWARE UNIT.

**[Class A, B, C]**

#### 5.5.2 Establish Software Unit Verification Process

The MANUFACTURER shall establish strategies, methods and procedures for verifying the SOFTWARE UNITS. Where VERIFICATION is done by testing, the test procedures shall be EVALUATED for adequacy.

**[Class B, C]**

#### 5.5.3 Software Unit Acceptance Criteria

The MANUFACTURER shall establish acceptance criteria for SOFTWARE UNITS prior to integration into larger SOFTWARE ITEMS as appropriate, and ensure that SOFTWARE UNITS meet acceptance criteria.

**[Class B, C]**

#### 5.5.4 Additional Software Unit Acceptance Criteria

When present in the design, the MANUFACTURER shall include additional acceptance criteria as appropriate for:

a) proper event sequence;
b) data and control flow;
c) planned resource allocation;
d) fault handling (error definition, isolation, and recovery);
e) initialisation of variables;
f) self-diagnostics;
g) memory management and memory overflows; and
h) boundary conditions.

**[Class C]**

#### 5.5.5 Software Unit Verification

The MANUFACTURER shall perform the SOFTWARE UNIT VERIFICATION and document the results.

**[Class B, C]**

### 5.6 Software Integration and Integration Testing

#### 5.6.1 Integrate Software Units

The MANUFACTURER shall integrate the SOFTWARE UNITS in accordance with the integration plan (see 5.1.5).

**[Class B, C]**

#### 5.6.2 Verify Software Integration

The MANUFACTURER shall verify that the SOFTWARE UNITS have been integrated into SOFTWARE ITEMS and/or the SOFTWARE SYSTEM in accordance with the integration plan and retain records of the evidence.

**[Class B, C]**

#### 5.6.3 Software Integration Testing

The MANUFACTURER shall test the integrated SOFTWARE ITEMS in accordance with the integration plan and document the results.

**[Class B, C]**

#### 5.6.4 Software Integration Testing Content

For software integration testing, the MANUFACTURER shall address whether the integrated SOFTWARE ITEM performs as intended.

**[Class B, C]**

*Examples to consider: required functionality, implementation of RISK CONTROL measures, specified timing and other behaviour, specified functioning of internal and external interfaces, testing under abnormal conditions including foreseeable misuse.*

#### 5.6.5 Evaluate Software Integration Test Procedures

The MANUFACTURER shall EVALUATE the integration test procedures for adequacy.

**[Class B, C]**

#### 5.6.6 Conduct Regression Tests

When software items are integrated, the MANUFACTURER shall conduct REGRESSION TESTING appropriate to demonstrate that defects have not been introduced into previously integrated software.

**[Class B, C]**

#### 5.6.7 Integration Test Record Contents

The MANUFACTURER shall:

a) document the test result (pass/fail and a list of ANOMALIES);
b) retain sufficient records to permit the test to be repeated; and
c) identify the tester.

**[Class B, C]**

#### 5.6.8 Use Software Problem Resolution Process

The MANUFACTURER shall enter ANOMALIES found during software integration and integration testing into a software problem resolution PROCESS.

**[Class B, C]**

### 5.7 Software System Testing

#### 5.7.1 Establish Tests for Software Requirements

a) The MANUFACTURER shall establish and perform a set of tests, expressed as input stimuli, expected outcomes, pass/fail criteria and procedures, for conducting SOFTWARE SYSTEM testing, such that all software requirements are covered.

b) The MANUFACTURER shall EVALUATE the adequacy of VERIFICATION strategies and test procedures.

**[Class A, B, C]**

#### 5.7.2 Use Software Problem Resolution Process

The MANUFACTURER shall enter ANOMALIES found during software system testing into a software problem resolution PROCESS.

**[Class A, B, C]**

#### 5.7.3 Retest After Changes

When changes are made during SOFTWARE SYSTEM testing, the MANUFACTURER shall:

a) repeat tests, perform modified tests or perform additional tests, as appropriate, to verify the effectiveness of the change in correcting the problem;

b) conduct testing appropriate to demonstrate that unintended side effects have not been introduced; and

c) perform relevant RISK MANAGEMENT ACTIVITIES as defined in 7.4.

**[Class A, B, C]**

#### 5.7.4 Evaluate Software System Testing

The MANUFACTURER shall EVALUATE the appropriateness of VERIFICATION strategies and test procedures.

The MANUFACTURER shall verify that:

a) all software requirements have been tested or otherwise VERIFIED;
b) the TRACEABILITY between software requirements and tests or other VERIFICATION is recorded; and
c) test results meet the required pass/fail criteria.

**[Class A, B, C]**

#### 5.7.5 Software System Test Record Contents

In order to support the repeatability of tests, the MANUFACTURER shall document:

a) a reference to test case procedures showing required actions and expected results;
b) the test result (pass/fail and a list of ANOMALIES);
c) the version of software tested;
d) relevant hardware and software test configurations;
e) relevant test tools;
f) date tested; and
g) the identity of the person responsible for executing the test and recording the test results.

**[Class A, B, C]**

### 5.8 Software Release

#### 5.8.1 Ensure Software Verification is Complete

The MANUFACTURER shall ensure that all software VERIFICATION ACTIVITIES have been completed and the results have been EVALUATED before the software is released.

**[Class A, B, C]**

#### 5.8.2 Document Known Residual Anomalies

The MANUFACTURER shall document all known residual ANOMALIES.

**[Class A, B, C]**

#### 5.8.3 Evaluate Known Residual Anomalies

The MANUFACTURER shall ensure that all known residual ANOMALIES have been EVALUATED to ensure that they do not contribute to an unacceptable RISK.

**[Class B, C]**

#### 5.8.4 Document Released Versions

The MANUFACTURER shall document the VERSION of the MEDICAL DEVICE SOFTWARE that is being released.

**[Class A, B, C]**

#### 5.8.5 Document How Released Software Was Created

The MANUFACTURER shall document the procedure and environment used to create the released software.

**[Class B, C]**

#### 5.8.6 Ensure Activities and Tasks are Complete

The MANUFACTURER shall ensure that all software development plan (or maintenance plan) ACTIVITIES and TASKS are complete along with the associated documentation.

**[Class B, C]**

#### 5.8.7 Archive Software

The MANUFACTURER shall archive:

a) the MEDICAL DEVICE SOFTWARE and CONFIGURATION ITEMS; and
b) the documentation

for at least the life time of the MEDICAL DEVICE SOFTWARE or a time specified by relevant regulatory requirements.

**[Class A, B, C]**

#### 5.8.8 Assure Reliable Delivery of Released Software

The MANUFACTURER shall establish procedures to ensure that the released MEDICAL DEVICE SOFTWARE can be reliably delivered to the point of use without corruption or unauthorised change. These procedures shall address:

- replication
- media labelling
- packaging
- protection
- storage
- delivery

**[Class A, B, C]**

---

## 6. Software Maintenance Process

### 6.1 Establish Software Maintenance Plan

The MANUFACTURER shall establish a software maintenance plan for conducting the ACTIVITIES and TASKS of the maintenance PROCESS. The plan shall address:

a) procedures for receiving, documenting, evaluating, resolving and tracking feedback arising after release;

b) criteria for determining whether feedback is considered to be a problem;

c) use of the software RISK MANAGEMENT PROCESS;

d) use of the software problem resolution PROCESS for analysing and resolving problems;

e) use of the software configuration management PROCESS for managing modifications; and

f) procedures to EVALUATE and implement upgrades, bug fixes, patches and obsolescence of SOUP.

**[Class A, B, C]**

### 6.2 Problem and Modification Analysis

#### 6.2.1 Document and Evaluate Feedback

##### 6.2.1.1 Monitor Feedback

The MANUFACTURER shall monitor feedback on MEDICAL DEVICE SOFTWARE released for intended use.

**[Class A, B, C]**

##### 6.2.1.2 Document and Evaluate Feedback

Feedback shall be documented and EVALUATED to determine whether a problem exists in a released MEDICAL DEVICE SOFTWARE. Any such problem shall be recorded as a PROBLEM REPORT. PROBLEM REPORTS shall include actual or potential adverse events, and deviations from specifications.

**[Class A, B, C]**

##### 6.2.1.3 Evaluate Problem Report's Effects on Safety

Each PROBLEM REPORT shall be EVALUATED to determine how it affects the SAFETY of MEDICAL DEVICE SOFTWARE released for intended use and whether a change to that software is needed.

**[Class A, B, C]**

#### 6.2.2 Use Software Problem Resolution Process

The MANUFACTURER shall use the software problem resolution PROCESS to address PROBLEM REPORTS.

**[Class A, B, C]**

#### 6.2.3 Analyse Change Requests

In addition to the analysis required by Clause 9, the MANUFACTURER shall analyse each CHANGE REQUEST for its effect on the organization, MEDICAL DEVICE SOFTWARE released for intended use, and SYSTEMS with which it interfaces.

**[Class A, B, C]**

#### 6.2.4 Change Request Approval

The MANUFACTURER shall EVALUATE and approve CHANGE REQUESTS which modify released MEDICAL DEVICE SOFTWARE.

**[Class A, B, C]**

#### 6.2.5 Communicate to Users and Regulators

The MANUFACTURER shall identify the approved CHANGE REQUESTS that affect released MEDICAL DEVICE SOFTWARE.

As required by local regulation, the MANUFACTURER shall inform users and regulators about:

a) any problem in released MEDICAL DEVICE SOFTWARE and the consequences of continued unchanged use; and

b) the nature of any available changes and how to obtain and install the changes.

**[Class A, B, C]**

### 6.3 Modification Implementation

#### 6.3.1 Use Established Process to Implement Modification

The MANUFACTURER shall identify and perform any Clause 5 ACTIVITIES that need to be repeated as a result of the modification.

**[Class A, B, C]**

#### 6.3.2 Re-release Modified Software System

The MANUFACTURER shall release modifications according to 5.8.

**[Class A, B, C]**

---

## 7. Software Risk Management Process

### 7.1 Analysis of Software Contributing to Hazardous Situations

#### 7.1.1 Identify Software Items That Could Contribute to a Hazardous Situation

The MANUFACTURER shall identify SOFTWARE ITEMS that could contribute to a hazardous situation identified in the MEDICAL DEVICE RISK ANALYSIS ACTIVITY of ISO 14971.

**[Class B, C]**

#### 7.1.2 Identify Potential Causes of Contribution to a Hazardous Situation

The MANUFACTURER shall identify potential causes of the SOFTWARE ITEM contributing to a hazardous situation.

The MANUFACTURER shall consider potential causes including, as appropriate:

a) incorrect or incomplete specification of functionality;
b) software defects in the identified SOFTWARE ITEM functionality;
c) failure or unexpected results from SOUP;
d) hardware failures or other software defects that could result in unpredictable software operation; and
e) reasonably foreseeable misuse.

**[Class B, C]**

#### 7.1.3 Evaluate Published SOUP Anomaly Lists

If failure or unexpected results from SOUP is a potential cause, the MANUFACTURER shall EVALUATE any ANOMALY list published by the supplier of the SOUP item relevant to the VERSION used in the MEDICAL DEVICE to determine if any known ANOMALIES result in a sequence of events that could result in a hazardous situation.

**[Class B, C]**

#### 7.1.4 Document Potential Causes

The MANUFACTURER shall document in the RISK MANAGEMENT FILE potential causes of the SOFTWARE ITEM contributing to a hazardous situation.

**[Class B, C]**

### 7.2 Risk Control Measures

#### 7.2.1 Define Risk Control Measures

For each case documented in the RISK MANAGEMENT FILE where a SOFTWARE ITEM could contribute to a HAZARDOUS SITUATION, the MANUFACTURER shall define and document RISK CONTROL measures in accordance with ISO 14971.

**[Class B, C]**

#### 7.2.2 Risk Control Measures Implemented in Software

If a RISK CONTROL measure is implemented as part of the functions of a SOFTWARE ITEM, the MANUFACTURER shall:

a) include the RISK CONTROL measure in the software requirements;

b) assign to each software item that contributes to the implementation of a risk control measure a software safety class based on the risk that the risk control measure is controlling; and

c) develop the SOFTWARE ITEM in accordance with Clause 5.

**[Class B, C]**

### 7.3 Verification of Risk Control Measures

#### 7.3.1 Verify Risk Control Measures

The implementation of each RISK CONTROL measure documented in 7.2 shall be VERIFIED, and this VERIFICATION shall be documented. The MANUFACTURER shall review the RISK CONTROL measure and determine if it could result in a new HAZARDOUS SITUATION.

**[Class B, C]**

#### 7.3.3 Document Traceability

The MANUFACTURER shall document TRACEABILITY of software HAZARDS as appropriate:

a) from the hazardous situation to the SOFTWARE ITEM;
b) from the SOFTWARE ITEM to the specific software cause;
c) from the software cause to the RISK CONTROL measure; and
d) from the RISK CONTROL measure to the VERIFICATION of the RISK CONTROL measure.

**[Class B, C]**

### 7.4 Risk Management of Software Changes

#### 7.4.1 Analyse Changes to Medical Device Software with Respect to Safety

The MANUFACTURER shall analyse changes to the MEDICAL DEVICE SOFTWARE (including SOUP) to determine whether:

a) additional potential causes are introduced contributing to a hazardous situation; and
b) additional software RISK CONTROL measures are required.

**[Class A, B, C]**

#### 7.4.2 Analyse Impact of Software Changes on Existing Risk Control Measures

The MANUFACTURER shall analyse changes to the software, including changes to SOUP, to determine whether the software modification could interfere with existing RISK CONTROL measures.

**[Class B, C]**

#### 7.4.3 Perform Risk Management Activities Based on Analyses

The MANUFACTURER shall perform relevant RISK MANAGEMENT ACTIVITIES defined in 7.1, 7.2 and 7.3 based on these analyses.

**[Class B, C]**

---

## 8. Software Configuration Management Process

### 8.1 Configuration Identification

#### 8.1.1 Establish Means to Identify Configuration Items

The MANUFACTURER shall establish a scheme for the unique identification of CONFIGURATION ITEMS and their VERSIONS to be controlled according to the development and configuration planning specified in 5.1.

**[Class A, B, C]**

#### 8.1.2 Identify SOUP

For each SOUP CONFIGURATION ITEM being used, including standard libraries, the MANUFACTURER shall document:

a) the title,
b) the MANUFACTURER, and
c) the unique SOUP designator (e.g., VERSION, release date, patch number, or upgrade designation)

**[Class A, B, C]**

#### 8.1.3 Identify System Configuration Documentation

The MANUFACTURER shall document the set of CONFIGURATION ITEMS and their VERSIONS that comprise the SOFTWARE SYSTEM configuration.

**[Class A, B, C]**

### 8.2 Change Control

#### 8.2.1 Approve Change Requests

The MANUFACTURER shall change CONFIGURATION ITEMS only in response to an approved CHANGE REQUEST.

**[Class A, B, C]**

#### 8.2.2 Implement Changes

The MANUFACTURER shall implement the change as specified in the CHANGE REQUEST. The MANUFACTURER shall identify and perform any ACTIVITY that needs to be repeated as a result of the change, including changes to the software safety classification.

**[Class A, B, C]**

#### 8.2.3 Verify Changes

The MANUFACTURER shall verify the change, including repeating any VERIFICATION that has been invalidated by the change.

**[Class A, B, C]**

#### 8.2.4 Provide Means for Traceability of Change

The MANUFACTURER shall maintain records of the relationships and dependencies between:

a) CHANGE REQUEST;
b) relevant PROBLEM REPORT; and
c) approval of the CHANGE REQUEST.

**[Class A, B, C]**

### 8.3 Configuration Status Accounting

The MANUFACTURER shall retain retrievable records of the history of controlled CONFIGURATION ITEMS including SYSTEM configuration.

**[Class A, B, C]**

---

## 9. Software Problem Resolution Process

### 9.1 Prepare Problem Reports

The MANUFACTURER shall prepare a PROBLEM REPORT for each problem detected in the MEDICAL DEVICE SOFTWARE. PROBLEM REPORTS shall include a statement of criticality (e.g., effect on performance, SAFETY, or SECURITY) as well as other information that may aid in resolution.

**[Class A, B, C]**

### 9.2 Investigate the Problem

The MANUFACTURER shall:

a) investigate the problem and if possible identify the causes;

b) EVALUATE the problem's relevance to SAFETY using the software RISK MANAGEMENT PROCESS;

c) document the outcome of the investigation and evaluation; and

d) create a CHANGE REQUEST(S) for actions needed to correct the problem, or document the rationale for taking no action.

**[Class A, B, C]**

### 9.3 Advise Relevant Parties

The MANUFACTURER shall advise relevant parties of the existence of the problem, as appropriate.

**[Class A, B, C]**

### 9.4 Use Change Control Process

The MANUFACTURER shall approve and implement all CHANGE REQUESTS, observing the requirements of the change control PROCESS (see 8.2).

**[Class A, B, C]**

### 9.5 Maintain Records

The MANUFACTURER shall maintain records of PROBLEM REPORTS and their resolution including their VERIFICATION.

The MANUFACTURER shall update the RISK MANAGEMENT FILE as appropriate.

**[Class A, B, C]**

### 9.6 Analyse Problems for Trends

The MANUFACTURER shall perform analysis to detect trends in PROBLEM REPORTS.

**[Class A, B, C]**

### 9.7 Verify Software Problem Resolution

The MANUFACTURER shall verify resolutions to determine whether:

a) problem has been resolved and the PROBLEM REPORT has been closed;
b) adverse trends have been reversed;
c) CHANGE REQUESTS have been implemented in the appropriate MEDICAL DEVICE SOFTWARE and ACTIVITIES; and
d) additional problems have been introduced.

**[Class A, B, C]**

### 9.8 Test Documentation Contents

When testing, retesting or REGRESSION TESTING following a change, the MANUFACTURER shall include in the test documentation:

a) test results;
b) ANOMALIES found;
c) the VERSION of software tested;
d) relevant hardware and software test configurations;
e) relevant test tools;
f) date tested; and
g) identification of the tester.

**[Class A, B, C]**

---

## Summary of Requirements by Software Safety Class

| Clause | Class A | Class B | Class C |
|--------|---------|---------|---------|
| **Clause 4** - All requirements | X | X | X |
| **5.1** - 5.1.1, 5.1.2, 5.1.3, 5.1.6, 5.1.7, 5.1.8, 5.1.9 | X | X | X |
| **5.1** - 5.1.5, 5.1.10, 5.1.11, 5.1.12 | | X | X |
| **5.1** - 5.1.4 | | | X |
| **5.2** - 5.2.1, 5.2.2, 5.2.4, 5.2.5, 5.2.6 | X | X | X |
| **5.2** - 5.2.3 | | X | X |
| **5.3** - 5.3.1, 5.3.2, 5.3.3, 5.3.4, 5.3.6 | | X | X |
| **5.3** - 5.3.5 | | | X |
| **5.4** - 5.4.1 | | X | X |
| **5.4** - 5.4.2, 5.4.3, 5.4.4 | | | X |
| **5.5** - 5.5.1 | X | X | X |
| **5.5** - 5.5.2, 5.5.3, 5.5.5 | | X | X |
| **5.5** - 5.5.4 | | | X |
| **5.6** - All requirements | | X | X |
| **5.7** - All requirements | X | X | X |
| **5.8** - 5.8.1, 5.8.2, 5.8.4, 5.8.7, 5.8.8 | X | X | X |
| **5.8** - 5.8.3, 5.8.5, 5.8.6 | | X | X |
| **Clause 6** - All requirements | X | X | X |
| **7.1** - All requirements | | X | X |
| **7.2** - All requirements | | X | X |
| **7.3** - All requirements | | X | X |
| **7.4** - 7.4.1 | X | X | X |
| **7.4** - 7.4.2, 7.4.3 | | X | X |
| **Clause 8** - All requirements | X | X | X |
| **Clause 9** - All requirements | X | X | X |
