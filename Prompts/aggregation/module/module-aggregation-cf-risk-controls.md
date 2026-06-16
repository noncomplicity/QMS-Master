---
id:
title: "Aggregate CF Risk Controls to Service Requirements"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "module"
standard: "IEC 62304, ISO 14971"
clause: "5.2, 7.1, 7.2"
inputs: ["CF risk tables", "service architecture docs", "service requirements docs", "service verification docs"]
outputs: ["updated CF risk tables in cf-*.md files"]
software_class: "B,C"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../../Requirements/ISO_14971_Requirements.md), [IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Aggregate CF Risk Controls to Service Requirements

## Context

IEC 62304 clause 7 and ISO 14971 clause 7 require that risk controls are implemented and verified. In Platform24's documentation model, risks are identified at the **Core Functionality (CF)** level in p24-docs, while implementation happens in **software items (services)**. This prompt bridges the two by:

1. Parsing CF risk tables to extract risk controls
2. Matching risk controls to concrete software requirements in service documentation
3. Identifying verification evidence (tests) for each requirement
4. Producing a traceability chain: **CF risk → risk control → system requirement → service → verification**

This output feeds into:
- Risk Management Report (risk control verification)
- System test planning (which tests verify which risk controls)
- Gap analysis (risk controls without requirements or verification)

## Key Concepts

### Documentation Layers

| Layer | Location | Contains |
|-------|----------|----------|
| **CF risk tables** | `p24-docs/docs/modules/{group}/{module}/core-functionalities/cf-*.md` | Risk ID, sequence of events, hazardous situation, services, risk control description, probability, acceptability |
| **Service architecture** | `Assets/Triage24/services/{service}-architecture.md` | Components, interfaces, data flows |
| **Service requirements** | `Assets/Triage24/services/{service}-requirements.md` | ITEM-*-REQ-* requirements extracted from code |
| **Service verification** | `Assets/Triage24/services/{service}-verification.md` | ITEM-*-UT-*, ITEM-*-IT-* test cases |

### Risk Control Categories

The CF risk tables use three risk areas, each with a different control discipline:

| Risk Area | Column Header | Control Type | Standard |
|-----------|--------------|--------------|----------|
| **SW** (Software Failure) | Risk control (sw req.) | Software requirement | IEC 62304 |
| **CFG** (Configuration Failure) | Risk control (sw req.) | Configuration requirement | IEC 62304 |
| **UI** (Use Error / User Interface) | Risk control (UIS) | UI specification | IEC 62366 |

### Traceability Chain

```
CF Risk Entry (e.g., CF-TRI-01-SW-1)
  → Risk Control ("Validation, unit testing, integration testing and regression testing")
    → System Requirement (concrete, testable requirement in a service)
      → Service(s) implementing the requirement
        → Test case(s) verifying the requirement
          → System test (TestRail) confirming end-to-end behavior
```

## Inputs

### Required

- **CF risk table files** — Markdown files containing risk assessment tables per CF
  - Location: `p24-docs/docs/modules/{group}/{module}/core-functionalities/cf-*.md`
  - Format: Markdown tables under `### Software Failure`, `### Configuration Failure`, `### Use Error and User Interface Failure`

- **Service requirements documents** — IEC 62304 clause 5.2 extractions per service
  - Location: `Assets/Triage24/services/{service}-requirements.md`
  - Contains: ITEM-*-REQ-FUNC-*, ITEM-*-REQ-INT-*, ITEM-*-REQ-SEC-*, etc.

- **Service verification documents** — IEC 62304 clause 5.5-5.7 extractions per service
  - Location: `Assets/Triage24/services/{service}-verification.md`
  - Contains: ITEM-*-UT-*, ITEM-*-IT-* test cases with requirement traceability

### Optional (enriches output)

- **Service architecture documents** — For understanding component relationships
- **Hazardous situation definitions** — For severity context
- **TestRail test case mappings** — For system-level test traceability

### Configuration

- **module_id** — Module identifier (e.g., `triage24`)
- **module_name** — Human-readable name (e.g., `Triage24`)
- **parent_system** — System identifier (e.g., `RC-T24`)

## Instructions

### 1. Keep Granular Sequences of Events

Keep the existing granular rows from the CF risk tables. Each row represents a specific failure scenario that warrants its own risk control. Do not aggregate rows — the granularity allows each risk control to be specific and meaningful.

However, **improve** the existing rows:
- Rewrite vague sequence of events text (e.g., "Import/Export to Healthdata fails") into descriptive sentences that explain the failure scenario and its impact
- Correct the hazardous situation reference if wrong (e.g., "underestimated recommendation" failures should reference hs02, not hs01)
- Add new rows for CFG and UI risk areas where the service documentation reveals risks not currently captured

### 2. Define Concrete Risk Controls as System Requirements

For each row, replace the vague risk control text with a concrete, testable system requirement. Use **verbose, descriptive language** — do not reference requirement IDs (no ITEM-*-REQ-* references). The risk control should be readable and meaningful on its own, without needing to look up identifiers.

The requirement should:

a) **State what the system SHALL do** — in terms the risk control actually addresses
b) **Describe the concrete capabilities** that mitigate the risk, using plain language
c) **Be verifiable** — describe what would be tested, not test IDs

Read the service requirements and verification documents to understand what capabilities exist, then express the risk control in terms of those capabilities without citing IDs.

**Example transformation:**

- **Vague:** "Validation, unit testing, integration testing and regression testing"
- **Concrete:** "The interview execution pipeline shall validate all service interactions and handle failures gracefully. The interview service shall return meaningful error responses when dependent services are unavailable. The patient UI shall display an error state when the interview cannot proceed. The actions service shall validate interview state before executing downstream workflows. Verified through unit tests covering error paths, integration tests covering service interactions, and end-to-end regression testing of the complete triage flow."

### 3. Handle Multi-Service Risk Controls

Many risk entries reference multiple services. For these:

a) Express the risk control as a **system-level requirement** that spans the services
b) The requirement should describe the **expected system behavior**, not individual service behavior
c) List all implementing services in the Service(s) column
d) Describe each service's contribution to the control in plain language

### 4. Handle UI Risk Controls

For UI risk area entries (risk_area = UI):

a) Aggregate UI failure scenarios the same way — one or two per CF
b) Express the risk control as a UI specification requirement describing the expected user experience
c) Describe the specific UI behaviors that mitigate the risk (e.g., progress indication, confirmation dialogs, error feedback)
d) Reference the usability engineering process (UEF) as the design basis
e) Note that verification includes both usability evaluation and UI testing

### 5. Review for New Risks

After reviewing the service documentation, identify risks that are **not currently in the CF tables**:

- Services with safety-critical functionality that have no corresponding CF risk entry
- Cross-service interactions that could fail silently
- Configuration-dependent behavior without CFG risk entries
- UI flows without UI risk entries

Add new rows only when the risk is clearly within the CF's scope and represents a meaningful failure scenario. Prefer adding to an existing aggregated row's scope over creating a new row.

### 6. Identify Gaps

Flag as gaps requiring separate action:

| Gap Type | Description | Priority |
|----------|-------------|----------|
| `no_requirement` | Risk control cannot be mapped to any service requirement | High |
| `no_verification` | Service requirement exists but no test covers it | High |
| `missing_service_doc` | Referenced service has no requirements document | High |
| `cross_service_gap` | Multi-service control lacks integration verification | Medium |
| `ui_no_uef` | UI control has no UEF reference | Medium |
| `no_system_test` | Verified at unit/integration level but no system test | Low |

Document gaps as comments in the CF file below the risk tables, or in a separate gaps section if there are many.

## Output Format

The output is **updated risk tables in the CF files themselves** (`cf-*.md`). Each CF file's risk assessment tables are rewritten in place.

### Target Table Format

The risk tables follow the column schema from `_risk-definitions.md`:

| # | Column | Description |
|---|--------|-------------|
| 1 | **ID** | Unique row ID. Format: `CF-{MODULE}-{NNN}-{area}-{n}` |
| 2 | **Sequence of events** | What can go wrong (keep existing text or map to predefined SOE) |
| 3 | **Hazardous situation** | Link to hs01 or hs02 (keep existing) |
| 4 | **Service(s)** | Links to the software services that implement the risk control. Use markdown links to swrepos, e.g. `[interviewer4](../../../swrepos/interviewer4.md)` |
| 5 | **Risk control (sw req.)** | **Concrete system requirement** — replace vague text with a testable requirement statement. Reference service requirement IDs where possible. |
| 6 | **P** | Probability after risk control (1-5, keep existing) |
| 7 | **Acceptable** | Yes/No (keep existing) |
| 8 | **Rationale** | Acceptability rationale (keep existing) |

### Before (vague controls):

```markdown
| CF-TRI-01-SW-1 | Patient cant continue flow due to software item failure | [hs01-...] | [triage-ui](...), [interviewer4](...), [actions](...) | Validation, unit testing, integration testing and regression testing | 2 | Yes | ... |
| CF-TRI-01-SW-2 | Import/Export to Healthdata fails | [hs01-...] | [interviewer4](...), [healthdata](...) | Controls implemented in code with validation and testing | 2 | Yes | ... |
```

### After (same rows, concrete controls, improved descriptions, corrected HS references):

```markdown
| CF-TRI-01-SW-1 | Patient cannot continue the triage flow due to an unhandled software failure in the interview pipeline | [hs01-...] | [triage-ui](...), [interviewer4](...), [actions](...) | The interview service shall handle internal exceptions and return structured error responses rather than failing silently. The triage UI shall detect error responses and present a clear error state to the patient. The actions service shall validate interview state before executing downstream workflows. Verified through unit tests covering error paths, integration tests for service interactions, and end-to-end regression testing. | 2 | Yes | Triage24 is never the only entrance to care; patients can seek care through phone, physical visit, or 1177. |
| CF-TRI-01-SW-2 | The interview service fails to exchange health data with the healthdata service, preventing the interview from incorporating patient measurements or history | [hs01-...] | [interviewer4](...), [healthdata](...) | The interview service shall handle healthdata service unavailability gracefully, with error handling on the import/export interface. The healthdata service shall validate data integrity before returning health data items. Verified through integration tests covering the interviewer-to-healthdata data exchange and error scenarios. | 2 | Yes | Risk lowered through code-level error handling on the integration interface; benefit of health data in triage outweighs risk. |
```

### Key Principles

1. **Replace vague controls with concrete requirements** — "Validation, unit testing..." becomes specific SHALL statements referencing service requirement IDs
2. **Keep Service(s) column as service links** — These identify WHICH services implement the control
3. **Risk control column becomes the system requirement** — This is what gets verified through system testing
4. **Preserve existing risk metadata** — Don't change ID, sequence of events, hazardous situation, P, Acceptable, or Rationale unless there's a clear error
5. **Add new risk rows** — If the service documentation reveals risks not currently in the table, add new rows
6. **UI risk controls** — Reference UEF and usability engineering process with specific UI specification requirements

### For UI risk tables

The column header changes from "Risk control (sw req.)" to "Risk control (UIS)":

```markdown
| CF-TRI-08-UI-1 | Patient abandons or misnavigates the triage interview due to unclear UI, resulting in no recommendation | [hs01-...] | [triage-ui](...) | The triage UI shall provide clear progress indication throughout the interview, prevent accidental exit through confirmation dialogs, and guide the patient back to the interview if they navigate away. The UI shall be designed following the usability engineering process documented in the UEF, including formative evaluation with representative users. Verified through usability evaluation sessions and automated UI tests covering navigation flows. | 2 | Yes | ... |
```

## Compliance Mapping

| Output Element | Standard Clause | Evidence Provided |
|----------------|----------------|-------------------|
| Risk control extraction | ISO 14971 7.1 | Risk control measures identified |
| System requirements | IEC 62304 5.2 | Requirements implementing risk controls |
| Requirement-to-test tracing | IEC 62304 5.7 | Verification of risk control implementation |
| UI controls | IEC 62366 5.7 | Usability risk controls identified |
| Traceability matrix | ISO 14971 7.2, IEC 62304 7.3.3 | Complete risk control traceability |
| Gap analysis | ISO 14971 7.4 | Residual risk evaluation inputs |

## Validation Criteria

- [ ] All CF risk entries are parsed and included 🆔 Nxsphj
- [ ] Each risk control is categorized by type 🆔 OAMmlD
- [ ] Each SW/CFG risk control maps to at least one system requirement 🆔 jibEFM
- [ ] Each system requirement references implementing service(s) 🆔 QJqGG0
- [ ] Each system requirement has verification evidence (or gap flagged) 🆔 5QPY3U
- [ ] UI risk controls are separated and linked to UEF 🆔 F5Q6iO
- [ ] Multi-service controls have cross-service verification 🆔 JVDMQR
- [ ] Gaps are identified with type and priority 🆔 NxoUfc
- [ ] Traceability matrix is complete 🆔 k4sOTK
- [ ] Summary statistics are accurate 🆔 4Uf2oV
