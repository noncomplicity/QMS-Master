---
id:
title: "Code-as-Truth Documentation Orchestration"
version:
author:
effective_date:
type: "Specification"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Code-as-Truth Documentation Orchestration

This document defines the agent workflow for generating complete medical device software documentation from source code.

## Standards Coverage

| Standard | Scope | Prompts |
|----------|-------|---------|
| **ISO 14971:2019** | Risk Management | `extraction-14971-risks.md` |
| **IEC 62304:2006+A1:2015** | Software Lifecycle | `extraction-62304-*.md` |
| **IEC 82304-1:2016** | Health Software Products | `extraction-82304-*.md` |

## Data Flow Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         SOURCE CODE REPOSITORY                          │
├─────────────────────────────────────────────────────────────────────────┤
│  src/           package.json    README.md     .github/      tests/      │
│  configs/       Dockerfile      docs/         i18n/         ...         │
└───────────────────────────────────┬─────────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    ▼                               ▼
        ┌──────────────────────┐       ┌──────────────────────┐
        │  PHASE 1: PRODUCT    │       │  PHASE 1: RISK       │
        │  (IEC 82304-1)       │       │  (ISO 14971)         │
        ├──────────────────────┤       ├──────────────────────┤
        │ use-requirements.json│       │ risks.json           │
        │ ↓                    │       │ - hazards            │
        │ system-requirements  │       │ - hazardous sits     │
        │ .json                │       │ - risk controls      │
        └──────────┬───────────┘       └──────────┬───────────┘
                   │                               │
                   └───────────────┬───────────────┘
                                   ▼
        ┌──────────────────────────────────────────────────────┐
        │                  PHASE 2: SOFTWARE                    │
        │                  (IEC 62304)                          │
        ├──────────────────────────────────────────────────────┤
        │  requirements.json ← system-requirements.json         │
        │  architecture.json ← requirements.json                │
        │  detailed-design.json ← architecture.json (Class C)   │
        │  soup-list.json ← architecture.json                   │
        │  software-risk.json ← architecture.json + risks.json  │
        │  verification.json ← all above                        │
        │  change-control.json ← git history                    │
        └──────────────────────────────────────────────────────┘
                                   │
                                   ▼
        ┌──────────────────────────────────────────────────────┐
        │                  PHASE 3: VALIDATION                  │
        │                  (IEC 82304-1 Clause 6)               │
        ├──────────────────────────────────────────────────────┤
        │  validation.json ← use-requirements + verification    │
        └──────────────────────────────────────────────────────┘
                                   │
                                   ▼
        ┌──────────────────────────────────────────────────────┐
        │                  PHASE 4: GENERATION                  │
        │                  (All Standards)                      │
        ├──────────────────────────────────────────────────────┤
        │  Documents Generated:                                 │
        │  ├── Software Requirements Specification (62304)      │
        │  ├── Software Architecture Description (62304)        │
        │  ├── SOUP List (62304)                                │
        │  ├── Software Verification Report (62304)             │
        │  ├── Software Risk Management File (62304 + 14971)    │
        │  ├── Software Release Notes (62304)                   │
        │  ├── Instructions for Use (82304)                     │
        │  ├── Technical Description (82304)                    │
        │  └── Validation Report (82304)                        │
        └──────────────────────────────────────────────────────┘
```

## Execution Phases

### Phase 0: Initialization

**Purpose:** Prepare the extraction environment

**Steps:**
1. Clone/access target repository
2. Identify software safety class (default: Class C until determined)
3. Configure output directory
4. Validate repository structure

**Outputs:**
- `extraction-config.json` — Repository metadata and settings

---

### Phase 1: Product & Risk Analysis (Parallel)

**Purpose:** Extract product-level requirements and identify hazards

These can run in parallel as they have no interdependencies.

#### Phase 1A: Product Requirements (IEC 82304-1)

```
┌─────────────────────────────────────────┐
│ extraction-82304-use-requirements.md    │
│ Inputs: README, package.json, docs      │
│ Output: use-requirements.json           │
└────────────────────┬────────────────────┘
                     ▼
┌─────────────────────────────────────────┐
│ extraction-82304-system-requirements.md │
│ Inputs: use-requirements.json + code    │
│ Output: system-requirements.json        │
└─────────────────────────────────────────┘
```

#### Phase 1B: Risk Identification (ISO 14971)

```
┌─────────────────────────────────────────┐
│ extraction-14971-risks.md               │
│ Inputs: source code, safety controls    │
│ Output: risks.json                      │
└─────────────────────────────────────────┘
```

**Phase 1 Complete When:**
- `use-requirements.json` exists
- `system-requirements.json` exists
- `risks.json` exists

---

### Phase 2: Software Lifecycle (Sequential)

**Purpose:** Extract IEC 62304 software lifecycle artifacts

**Execution Order:** Sequential due to data dependencies

```
Step 2.1: Software Requirements
┌─────────────────────────────────────────┐
│ extraction-62304-requirements.md        │
│ Inputs: system-requirements.json,       │
│         source code, risks.json         │
│ Output: requirements.json               │
└────────────────────┬────────────────────┘
                     ▼
Step 2.2: Architecture
┌─────────────────────────────────────────┐
│ extraction-62304-architecture.md        │
│ Inputs: requirements.json, source code  │
│ Output: architecture.json               │
└────────────────────┬────────────────────┘
                     ▼
Step 2.3: SOUP Analysis
┌─────────────────────────────────────────┐
│ extraction-62304-soup.md                │
│ Inputs: architecture.json, manifests    │
│ Output: soup-list.json                  │
└────────────────────┬────────────────────┘
                     ▼
Step 2.4: Detailed Design (Class C only)
┌─────────────────────────────────────────┐
│ extraction-62304-detailed-design.md     │
│ Inputs: architecture.json, source code  │
│ Output: detailed-design.json            │
│ Condition: software_class == "C"        │
└────────────────────┬────────────────────┘
                     ▼
Step 2.5: Software Risk Analysis
┌─────────────────────────────────────────┐
│ extraction-62304-risk.md                │
│ Inputs: architecture.json, risks.json,  │
│         soup-list.json                  │
│ Output: software-risk.json              │
└────────────────────┬────────────────────┘
                     ▼
Step 2.6: Verification Evidence
┌─────────────────────────────────────────┐
│ extraction-62304-verification.md        │
│ Inputs: requirements.json, tests/,      │
│         software-risk.json              │
│ Output: verification.json               │
└────────────────────┬────────────────────┘
                     ▼
Step 2.7: Change Control
┌─────────────────────────────────────────┐
│ extraction-62304-change-control.md      │
│ Inputs: git history, issues, PRs        │
│ Output: change-control.json             │
└─────────────────────────────────────────┘
```

**Phase 2 Complete When:**
- `requirements.json` exists
- `architecture.json` exists
- `soup-list.json` exists
- `detailed-design.json` exists (if Class C)
- `software-risk.json` exists
- `verification.json` exists
- `change-control.json` exists

---

### Phase 3: Validation (IEC 82304-1 Clause 6)

**Purpose:** Validate product against use requirements

```
┌─────────────────────────────────────────┐
│ extraction-82304-validation.md          │
│ Inputs: use-requirements.json,          │
│         verification.json               │
│ Output: validation.json                 │
└─────────────────────────────────────────┘
```

---

### Phase 4: Document Generation (Parallel)

**Purpose:** Generate regulatory documents from extracted data

All generation prompts can run in parallel once their input data exists.

```
Parallel Generation:

┌─────────────────────────────────────────┐
│ generation-62304-srs.md                 │
│ Input: requirements.json                │
│ Output: SRS.md                          │
├─────────────────────────────────────────┤
│ generation-62304-sad.md                 │
│ Input: architecture.json, soup-list.json│
│ Output: SAD.md                          │
├─────────────────────────────────────────┤
│ generation-62304-soup-list.md           │
│ Input: soup-list.json                   │
│ Output: SOUP-List.md                    │
├─────────────────────────────────────────┤
│ generation-62304-verification-report.md │
│ Input: verification.json                │
│ Output: SVR.md                          │
├─────────────────────────────────────────┤
│ generation-62304-risk-file.md           │
│ Input: software-risk.json, risks.json   │
│ Output: SRMF.md                         │
├─────────────────────────────────────────┤
│ generation-62304-release-notes.md       │
│ Input: change-control.json, verification│
│ Output: Release-Notes.md                │
├─────────────────────────────────────────┤
│ generation-82304-ifu.md                 │
│ Input: use-requirements.json, docs      │
│ Output: IFU.md                          │
├─────────────────────────────────────────┤
│ generation-82304-technical-description  │
│ Input: system-requirements.json         │
│ Output: Technical-Description.md        │
├─────────────────────────────────────────┤
│ generation-82304-validation-report.md   │
│ Input: validation.json                  │
│ Output: Validation-Report.md            │
└─────────────────────────────────────────┘
```

---

## Agent Invocation

### Full Documentation Generation

```bash
# Run complete extraction and generation
claude "Execute the code-as-truth documentation workflow for this repository.
Follow the orchestration in Prompts/_orchestration.md.
Output extracted data to docs/extracted/
Output generated documents to docs/regulatory/"
```

### Incremental Update

```bash
# Update specific documents after code changes
claude "Run incremental documentation update:
1. Re-extract verification.json (tests changed)
2. Re-generate SVR.md
3. Update Release-Notes.md with new changes"
```

### Phase-Specific Execution

```bash
# Run only Phase 1 (product/risk)
claude "Execute Phase 1 of code-as-truth workflow:
Run extraction-82304-use-requirements.md
Run extraction-82304-system-requirements.md
Run extraction-14971-risks.md
Output to docs/extracted/"
```

---

## Data Dependencies Matrix

| Output | Required Inputs | Prompt |
|--------|-----------------|--------|
| `use-requirements.json` | README, package.json, docs | 82304-use-requirements |
| `system-requirements.json` | use-requirements.json, code | 82304-system-requirements |
| `risks.json` | source code, safety controls | 14971-risks |
| `requirements.json` | system-requirements.json, risks.json | 62304-requirements |
| `architecture.json` | requirements.json, source code | 62304-architecture |
| `soup-list.json` | architecture.json, manifests | 62304-soup |
| `detailed-design.json` | architecture.json (Class C) | 62304-detailed-design |
| `software-risk.json` | architecture.json, risks.json, soup-list.json | 62304-risk |
| `verification.json` | requirements.json, tests, software-risk.json | 62304-verification |
| `change-control.json` | git history | 62304-change-control |
| `validation.json` | use-requirements.json, verification.json | 82304-validation |

---

## Error Handling

### Missing Data

If a required input is missing:
1. Log warning with missing dependency
2. Skip dependent extractions
3. Add to gaps report
4. Continue with available extractions

### Extraction Failures

If extraction fails:
1. Log error with details
2. Save partial output if available
3. Continue with other extractions
4. Report in final summary

### Validation Failures

If validation criteria are not met:
1. Document failures in output
2. Generate gap analysis
3. Provide recommendations
4. Continue to generate documents (with warnings)

---

## Output Structure

```
docs/
├── extracted/                    # JSON extraction outputs
│   ├── use-requirements.json
│   ├── system-requirements.json
│   ├── risks.json
│   ├── requirements.json
│   ├── architecture.json
│   ├── soup-list.json
│   ├── detailed-design.json      # Class C only
│   ├── software-risk.json
│   ├── verification.json
│   ├── change-control.json
│   └── validation.json
│
├── regulatory/                   # Generated markdown documents
│   ├── SRS.md                    # Software Requirements Specification
│   ├── SAD.md                    # Software Architecture Description
│   ├── SOUP-List.md              # SOUP List
│   ├── SVR.md                    # Software Verification Report
│   ├── SRMF.md                   # Software Risk Management File
│   ├── Release-Notes.md          # Software Release Notes
│   ├── IFU.md                    # Instructions for Use
│   ├── Technical-Description.md  # Technical Description
│   └── Validation-Report.md      # Validation Report
│
└── reports/                      # Analysis reports
    ├── coverage-report.md        # Regulatory coverage analysis
    ├── gap-analysis.md           # Identified gaps
    └── traceability-matrix.md    # Cross-document traceability
```

---

## Maintenance Workflow

### On Code Change

```
1. Detect changed files (git diff)
2. Determine affected extractions:
   - src/ changes → requirements, architecture, software-risk, verification
   - package.json changes → soup-list
   - tests/ changes → verification
   - docs/ changes → use-requirements
3. Re-run affected extractions
4. Re-generate affected documents
5. Update Release-Notes.md
```

### On Release

```
1. Run full extraction (fresh baseline)
2. Generate all documents
3. Create release package
4. Archive in DHF
```

### Periodic Review

```
1. Re-run SOUP analysis (check for new vulnerabilities)
2. Re-run risk extraction (verify controls still effective)
3. Update gap analysis
4. Generate compliance report
```

---

## Integration with CI/CD

See `automation/github-actions-integration.md` for CI/CD workflow definitions.

### Recommended Triggers

| Trigger | Action |
|---------|--------|
| Push to main | Incremental extraction + generation |
| Pull request | Preview generation (no commit) |
| Release tag | Full extraction + generation + archive |
| Weekly schedule | SOUP vulnerability check |
| Manual dispatch | Full regeneration |
