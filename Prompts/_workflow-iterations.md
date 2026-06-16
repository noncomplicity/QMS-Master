---
id: 725af57
title: "_workflow iterations"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Specification"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Code-as-Truth Workflow Iterations

## Critical Principle: Documentation vs Execution

> **This framework documents what EXISTS in code to satisfy regulatory requirements.**
> **It does NOT report execution results, pass/fail status, or coverage metrics.**

When you see "verification" in this workflow:
- ✅ Document test cases that exist
- ✅ Document traceability (test → requirement → control)
- ❌ Do NOT report pass/fail status
- ❌ Do NOT report coverage percentages

See `_documentation-vs-execution.md` for the complete rationale.

## The Bidirectional Model

Code-as-truth does NOT mean "extract everything from code and ignore requirements." It means:

1. **Requirements define intent** — What the system SHOULD do
2. **Code defines implementation** — What the system DOES do
3. **Gap analysis reveals** — What's missing, what's undocumented, what's diverged
4. **Iteration closes gaps** — Either update requirements or fix implementation

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         BIDIRECTIONAL FLOW                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  TOP-DOWN                              BOTTOM-UP                         │
│  (Intent)                              (Reality)                         │
│                                                                          │
│  Intended Use ──────────────────────────────────────────┐                │
│       │                                                 │                │
│       ▼                                                 ▼                │
│  Use Requirements ◄─────── GAP ANALYSIS ───────► Extracted Capabilities │
│       │                         │                       │                │
│       ▼                         ▼                       │                │
│  System Requirements ◄──── ITERATION ──────────► Item Requirements      │
│       │                         │                       │                │
│       ▼                         ▼                       │                │
│  Risk Acceptability ◄───── ITERATION ──────────► Item Risk Contribution │
│       │                         │                       │                │
│       ▼                         ▼                       │                │
│  Verification Plan ◄────── ITERATION ──────────► Item Verification      │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

## Iteration Phases

### Phase 0: System Definition (Top-Down)

**Input:** Intended use, regulatory requirements, clinical needs
**Output:** System-level requirements and constraints

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 0: SYSTEM DEFINITION                                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  1. Define intended use (IEC 82304-1 4.1)                               │
│     - What health problem does it solve?                                 │
│     - Who are the users?                                                 │
│     - What is the operational environment?                               │
│                                                                          │
│  2. Define use requirements (IEC 82304-1 4.2)                           │
│     - Functional capabilities needed                                     │
│     - User interface requirements                                        │
│     - Security and privacy requirements                                  │
│                                                                          │
│  3. Define system requirements (IEC 82304-1 4.5)                        │
│     - System-level functional requirements                               │
│     - Platform requirements                                              │
│     - Interoperability requirements                                      │
│                                                                          │
│  4. Define risk acceptability criteria (ISO 14971 4.4)                  │
│     - Severity levels                                                    │
│     - Probability levels                                                 │
│     - Risk acceptability matrix                                          │
│     - ALARP policy                                                       │
│                                                                          │
│  OUTPUT:                                                                 │
│  - system/defined/use-requirements.json                                  │
│  - system/defined/system-requirements.json                               │
│  - system/defined/risk-acceptability-criteria.json                       │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Phase 1: Item Extraction (Bottom-Up)

**Input:** Source code repositories
**Output:** Item-level extractions showing what's implemented

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 1: ITEM EXTRACTION (per repository)                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  For each software item:                                                 │
│                                                                          │
│  1. Extract item requirements                                            │
│     - What capabilities are implemented?                                 │
│     - What interfaces exist?                                             │
│     - What data is handled?                                              │
│                                                                          │
│  2. Extract item architecture                                            │
│     - What components exist?                                             │
│     - How do they interact?                                              │
│     - What external dependencies?                                        │
│                                                                          │
│  3. Extract item SOUP                                                    │
│     - What third-party libraries?                                        │
│     - What versions?                                                     │
│     - What are they used for?                                            │
│                                                                          │
│  4. Extract item verification (test case documentation)                  │
│     - What test cases exist? (traceable to test files)                   │
│     - What do they verify? (requirements, controls)                      │
│     - What's missing? (gaps in test coverage)                            │
│     NOTE: Document test CASES, not execution results                     │
│                                                                          │
│  5. Extract item risk contribution                                       │
│     - What hazards can this item cause?                                  │
│     - What controls are implemented?                                     │
│     - What failure modes exist?                                          │
│                                                                          │
│  OUTPUT:                                                                 │
│  - items/{item}/extracted/item-requirements.json                         │
│  - items/{item}/extracted/item-architecture.json                         │
│  - items/{item}/extracted/item-soup.json                                 │
│  - items/{item}/extracted/item-verification.json                         │
│  - items/{item}/extracted/item-risk-contribution.json                    │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Phase 2: Gap Analysis (Comparison)

**Input:** System requirements + Item extractions
**Output:** Gap report identifying misalignments

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 2: GAP ANALYSIS                                                    │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Compare system requirements vs item extractions:                        │
│                                                                          │
│  REQUIREMENTS GAPS                                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐│
│  │ System Requirement    │ Item Implementation │ Gap                   ││
│  ├───────────────────────┼────────────────────┼──────────────────────┤│
│  │ SYS-REQ-001: Patient  │ ITEM-HM-FUNC-001   │ ✅ Implemented        ││
│  │ can view appointments │                     │                      ││
│  ├───────────────────────┼────────────────────┼──────────────────────┤│
│  │ SYS-REQ-002: Two-     │ Not found          │ ❌ Not implemented    ││
│  │ factor authentication │                     │                      ││
│  ├───────────────────────┼────────────────────┼──────────────────────┤│
│  │ Not specified         │ ITEM-HM-FUNC-018   │ ⚠️ Undocumented       ││
│  │                       │ AI note assist     │ capability            ││
│  └─────────────────────────────────────────────────────────────────────┘│
│                                                                          │
│  RISK GAPS                                                               │
│  ┌─────────────────────────────────────────────────────────────────────┐│
│  │ Identified Hazard              │ Control Status                     ││
│  ├────────────────────────────────┼────────────────────────────────────┤│
│  │ HAZ-001: Wrong patient data    │ ✅ Controlled (authorization)      ││
│  │ HAZ-002: Data corruption       │ ⚠️ Partial (validation exists,     ││
│  │                                │    but no integrity checks)        ││
│  │ HAZ-003: Unauthorized access   │ ❌ System control needed (2FA)     ││
│  └─────────────────────────────────────────────────────────────────────┘│
│                                                                          │
│  VERIFICATION GAPS (test case existence, not execution)                  │
│  ┌─────────────────────────────────────────────────────────────────────┐│
│  │ Requirement          │ Test Cases   │ Gap                          ││
│  ├──────────────────────┼──────────────┼──────────────────────────────┤│
│  │ Handover workflow    │ 12 tests     │ Missing: deny + cancel paths ││
│  │ Authorization        │ 8 tests      │ Missing: cross-provider edge ││
│  │ Data erasure         │ 2 tests      │ ❌ Insufficient test cases    ││
│  └─────────────────────────────────────────────────────────────────────┘│
│                                                                          │
│  OUTPUT:                                                                 │
│  - system/analysis/requirements-gap-analysis.json                        │
│  - system/analysis/risk-gap-analysis.json                                │
│  - system/analysis/verification-gap-analysis.json                        │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Phase 3: Iteration (Close Gaps)

**Input:** Gap analysis
**Output:** Updated requirements OR implementation changes

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 3: ITERATION                                                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  For each gap, decide:                                                   │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐│
│  │ Gap Type                      │ Resolution Options                  ││
│  ├───────────────────────────────┼─────────────────────────────────────┤│
│  │ Requirement not implemented   │ A) Implement the feature            ││
│  │                               │ B) Remove/defer the requirement     ││
│  │                               │ C) Document as known limitation     ││
│  ├───────────────────────────────┼─────────────────────────────────────┤│
│  │ Implementation not in reqs    │ A) Add to system requirements       ││
│  │                               │ B) Remove the feature               ││
│  │                               │ C) Document as derived requirement  ││
│  ├───────────────────────────────┼─────────────────────────────────────┤│
│  │ Risk not controlled           │ A) Implement control in code        ││
│  │                               │ B) Implement control at system      ││
│  │                               │ C) Accept risk with justification   ││
│  │                               │ D) Add to IFU (user responsibility) ││
│  ├───────────────────────────────┼─────────────────────────────────────┤│
│  │ Missing test cases            │ A) Add test cases                   ││
│  │                               │ B) Document inspection evidence     ││
│  │                               │ C) Accept with risk justification   ││
│  └─────────────────────────────────────────────────────────────────────┘│
│                                                                          │
│  After iteration:                                                        │
│  - Re-run affected extractions                                           │
│  - Re-run gap analysis                                                   │
│  - Repeat until gaps are acceptable                                      │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Phase 4: Aggregation (After Gaps Closed)

**Input:** Aligned item extractions + system requirements
**Output:** Module and system-level aggregations

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 4: AGGREGATION                                                     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Module aggregation:                                                     │
│  - Combine item architectures → module architecture                      │
│  - Combine item SOUP → module SOUP (deduplicated)                        │
│  - Combine item risks → module risks (+ cross-item risks)                │
│  - Combine item verification → module verification                       │
│                                                                          │
│  System aggregation:                                                     │
│  - Combine module outputs → system outputs                               │
│  - Add system-level risks (cross-module)                                 │
│  - Create product risk file                                              │
│  - Create traceability matrices                                          │
│                                                                          │
│  OUTPUT:                                                                 │
│  - modules/{module}/aggregated/module-*.json                             │
│  - system/aggregated/product-risk-file.json                              │
│  - system/aggregated/system-soup.json                                    │
│  - system/aggregated/system-verification.json                            │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Phase 5: Generation (Final Documents)

**Input:** Aggregated outputs + gap resolutions
**Output:** Regulatory documentation

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 5: GENERATION                                                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Generate item-level documents:                                          │
│  - Item-Requirements.md                                                  │
│  - Item-Architecture.md                                                  │
│  - Item-SOUP-List.md                                                     │
│  - Item-Verification.md                                                  │
│  - Item-Risk-Contribution.md                                             │
│                                                                          │
│  Generate system-level documents:                                        │
│  - SRS.md (Software Requirements Specification)                          │
│  - SAD.md (Software Architecture Description)                            │
│  - SOUP-List.md (consolidated)                                           │
│  - RMP.md (Risk Management Plan)                                         │
│  - Risk-Analysis.md                                                      │
│  - RMR.md (Risk Management Report)                                       │
│  - SVR.md (Software Verification Report)                                 │
│  - Validation-Report.md                                                  │
│  - IFU.md (Instructions for Use)                                         │
│  - Technical-Description.md                                              │
│                                                                          │
│  OUTPUT:                                                                 │
│  - items/{item}/generated/Item-*.md                                      │
│  - system/generated/*.md                                                 │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

## Complete Workflow Sequence

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      COMPLETE ITERATION WORKFLOW                         │
└─────────────────────────────────────────────────────────────────────────┘

PHASE 0: SYSTEM DEFINITION
│
│  Define intended use
│  Define use requirements  ──────────────────────────┐
│  Define system requirements ────────────────────────┤
│  Define risk acceptability criteria ────────────────┤
│                                                     │
▼                                                     ▼
PHASE 1: ITEM EXTRACTION ◄────────────────── INPUT: What to look for
│
│  Extract item requirements
│  Extract item architecture
│  Extract item SOUP
│  Extract item verification
│  Extract item risk contribution
│
▼
PHASE 2: GAP ANALYSIS
│
│  Compare system requirements ↔ item implementation
│  Compare risk acceptability ↔ item risk controls
│  Compare verification plan ↔ item test coverage
│
│  ┌────────────────────────────────────────────────┐
│  │ GAPS FOUND?                                    │
│  │                                                │
│  │  YES ──► PHASE 3: ITERATION                    │
│  │          │                                     │
│  │          ├─► Update system requirements?       │
│  │          ├─► Implement missing features?       │
│  │          ├─► Add risk controls?                │
│  │          ├─► Add tests?                        │
│  │          │                                     │
│  │          └─► Return to PHASE 1 ◄───────────────┤
│  │                                                │
│  │  NO ───► Continue to PHASE 4                   │
│  └────────────────────────────────────────────────┘
│
▼
PHASE 4: AGGREGATION
│
│  Module aggregation
│  System aggregation
│
▼
PHASE 5: GENERATION
│
│  Generate item documents
│  Generate system documents
│
▼
RELEASE
```

## Practical Example: Platform24

### Iteration 1: Initial Extraction

```
1. Define system requirements (from product management):
   - SYS-REQ-001: Patient can book appointments
   - SYS-REQ-002: Practitioner can manage patient queue
   - SYS-REQ-003: Appointments can be handed over
   - ...

2. Extract from health-manager:
   - ITEM-HM-FUNC-001: Appointment management ✅
   - ITEM-HM-FUNC-002: Handover workflow ✅
   - ITEM-HM-FUNC-018: AI note assist (NOT IN REQS!)

3. Gap analysis:
   - SYS-REQ-001 → ITEM-HM-FUNC-001 ✅
   - SYS-REQ-003 → ITEM-HM-FUNC-002 ✅
   - ITEM-HM-FUNC-018 → ??? (undocumented capability)
```

### Iteration 2: Close Gaps

```
4. Decision: Add AI note assist to system requirements
   - Create SYS-REQ-042: AI-assisted note generation
   - Define risk acceptability for AI features
   - Define verification requirements for AI

5. Decision: Identified uncontrolled risk
   - HAZ-005: AI hallucination could mislead clinician
   - Control needed: "AI suggestions must be reviewed by practitioner"
   - Implementation: Already exists (UI requires confirmation)
   - Verification: Add test case for confirmation workflow (document exists)

6. Re-extract with updated requirements context
```

### Iteration 3: Aligned State

```
7. Gap analysis shows:
   - All system requirements traced to implementation ✅
   - All implementations traced to requirements ✅
   - All risks have acceptable controls ✅
   - All requirements have test cases ✅ (traceability, not execution)

8. Proceed to aggregation and generation
   NOTE: Execution evidence (pass/fail) is maintained by CI/CD, not this framework
```

## Directory Structure with Iterations

```
Output/platform24/
├── system-manifest.json
│
├── system/
│   ├── defined/                          # PHASE 0: Top-down definitions
│   │   ├── use-requirements.json
│   │   ├── system-requirements.json
│   │   └── risk-acceptability-criteria.json
│   │
│   ├── analysis/                         # PHASE 2: Gap analysis
│   │   ├── requirements-gap-analysis.json
│   │   ├── risk-gap-analysis.json
│   │   └── verification-gap-analysis.json
│   │
│   ├── aggregated/                       # PHASE 4: Aggregation
│   │   ├── product-risk-file.json
│   │   ├── system-soup.json
│   │   └── system-verification.json
│   │
│   └── generated/                        # PHASE 5: Generation
│       ├── SRS.md
│       ├── RMP.md
│       ├── RMR.md
│       └── ...
│
├── modules/
│   └── smart-care-plans/
│       └── aggregated/
│
└── items/
    └── health-manager/
        ├── extracted/                    # PHASE 1: Bottom-up extraction
        │   ├── item-requirements.json
        │   ├── item-architecture.json
        │   └── ...
        └── generated/
            └── Item-*.md
```

## Prompts by Phase

| Phase | Prompts |
|-------|---------|
| **0: Definition** | Manual or `system-definition-*.md` (to create) |
| **1: Extraction** | `item-extraction-62304-*.md` |
| **2: Gap Analysis** | `gap-analysis-requirements.md` (to create) |
| **3: Iteration** | Manual decisions, code changes |
| **4: Aggregation** | `module-aggregation-*.md`, `system-aggregation-*.md` |
| **5: Generation** | `item-generation-*.md`, `generation-*.md` |

## How to Run

See `_how-to-run.md` for step-by-step execution instructions.

For automated orchestration, see `_master-orchestration.md`.
