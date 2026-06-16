---
id: 725af57
title: "_prompt schema"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Specification"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Code-as-Truth Prompt Schema

This document defines the standard structure for all extraction and generation prompts in the code-as-truth documentation framework.

## Purpose

Prompts instruct AI workers (Claude Code, GitHub Actions, or other automation) to:
1. **Extract** structured data from source code repositories
2. **Generate** regulatory documents from extracted data
3. **Aggregate** lower-level outputs into higher-level documentation

Consistent prompt structure ensures:
- Predictable AI worker behavior
- Consistent output formats
- Traceable regulatory compliance
- Maintainable prompt library
- **Proper hierarchical documentation** aligned with software architecture

## Critical Principle: Documentation vs Execution

> **This framework documents what EXISTS in code to satisfy regulatory requirements.**
> **This framework does NOT report execution results or runtime metrics.**

This distinction is fundamental to the framework's integrity:

### What Extraction/Aggregation DOES (Documentation)

| Regulatory Need | What We Document | Traceable To |
|-----------------|------------------|--------------|
| Document requirements (62304 5.2) | Requirements implemented in code | Source files, interfaces |
| Document architecture (62304 5.3) | Components, interfaces, data flows | Package structure, classes |
| Document SOUP (62304 8.1) | Third-party dependencies | pom.xml, package.json |
| Document verification (62304 5.5-5.7) | **Test cases that exist** | Test files, test methods |
| Document risk controls (14971 7) | Controls implemented in code | Validation logic, error handling |

### What This Framework Does NOT Do (Execution)

| NOT Our Scope | Belongs To | Why |
|---------------|------------|-----|
| Test pass/fail results | CI/CD pipeline | Execution happens at runtime |
| Coverage percentages | Coverage tools | Requires code execution |
| Execution timestamps | CI/CD logs | Runtime metadata |
| Anomaly/defect status | Issue trackers | Operational data |
| "Release readiness" | Release process | Requires execution evidence |

### Correct Terminology

**Use these (documentation):**
- `test_cases` — Test cases that exist
- `traceability_status: traced | partial | untraced` — Whether test cases exist
- `test_specification` — What the test is designed to verify
- `verification_gaps` — Missing test cases

**Avoid these (execution):**
- ~~`status: passed | failed`~~ — Execution result
- ~~`coverage: 72.5%`~~ — Requires execution
- ~~`verification_status: verified`~~ — Implies execution happened
- ~~`last_executed`~~ — Runtime timestamp

See `_documentation-vs-execution.md` for the complete rationale.

## Software Hierarchy Levels

A critical concept in this framework is that **a repository is not necessarily a complete system**. Documentation must be generated at the appropriate level of the software hierarchy:

```
┌─────────────────────────────────────────────────────────────────────┐
│ SYSTEM (Health Software Product)                                     │
│ IEC 82304-1 scope • Product-level risk management • Validation      │
│ Examples: Triage24, Platform24                                       │
├─────────────────────────────────────────────────────────────────────┤
│ MODULE (Deployable Software Component)                               │
│ May span multiple repositories • Integration boundary               │
│ Examples: Smart Care Plans, Booking Service                         │
├─────────────────────────────────────────────────────────────────────┤
│ SOFTWARE ITEM (Repository-level)                                     │
│ IEC 62304 scope • Single codebase • Unit/integration testing        │
│ Examples: health-manager, appointment-api, patient-portal           │
├─────────────────────────────────────────────────────────────────────┤
│ SOFTWARE UNIT (Class/Function level)                                 │
│ Smallest testable element • Internal to repository                  │
│ Examples: AppointmentService, HandoverController                    │
└─────────────────────────────────────────────────────────────────────┘
```

### Level Definitions

| Level | IEC 62304 Term | Scope | Repository Relationship |
|-------|----------------|-------|------------------------|
| **System** | Software System | Complete product | Multiple modules, possibly multiple repos |
| **Module** | Software Item (high-level) | Deployable component | One or more repos |
| **Software Item** | Software Item | Single codebase | **One repository = One software item** |
| **Software Unit** | Software Unit | Class/function | Within a repository |

### Documentation Ownership by Level

| Level | Owns | Aggregates From |
|-------|------|-----------------|
| **System** | Use Requirements, System Requirements, Validation Report, IFU, Technical Description, Product Risk File (RMP, RMR) | Module outputs |
| **Module** | Module Architecture, Module Integration Tests, Module Risk Contribution | Software Item outputs |
| **Software Item** | Item Requirements, Item Architecture, Item SOUP, Item Verification, Item Risk Contribution | Code analysis |
| **Software Unit** | Unit tests, code coverage | N/A |

## Prompt Categories

| Category | Purpose | Input | Output |
|----------|---------|-------|--------|
| `extraction` | Extract structured data from code | Source code, configs, git history | JSON file |
| `generation` | Generate documents from data | JSON files from extraction | Markdown document |
| `aggregation` | Combine lower-level outputs | Multiple JSON files | Aggregated JSON or Markdown |
| `validation` | Validate extraction/generation output | JSON or Markdown | Validation report |
| `compliance` | Check regulatory compliance | Multiple inputs | Compliance report |

### Prompt Level Classification

Every prompt must declare which hierarchy level it operates at:

| Level | Extraction | Generation | Aggregation |
|-------|------------|------------|-------------|
| `system` | Use requirements, system requirements | IFU, Technical Description, Validation Report, RMP, RMR | Aggregate module outputs |
| `module` | Module interfaces, integration points | Module Architecture Overview | Aggregate item outputs |
| `item` | Requirements, architecture, SOUP, verification, risks | Item SRS, Item SAD, Item SVR, Item SOUP List, Item Risk Contribution | N/A |
| `unit` | (Covered by item-level verification) | N/A | N/A |

## Prompt File Structure

All prompts follow this structure:

```markdown
---
[YAML Frontmatter]
---

# [Title]

## Context
[Why this prompt exists, regulatory background]

## Inputs
[What the prompt needs to work with]

## Instructions
[Step-by-step instructions for the AI worker]

## Output Schema
[Exact format of expected output]

## Compliance Mapping
[How output maps to standard clauses]

## Examples
[Input/output examples]

## Validation Criteria
[Checklist to verify output quality]
```

## YAML Frontmatter Schema

### Required Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `id` | string | Auto-populated document ID | (leave empty) |
| `title` | string | Auto-populated from filename | (leave empty) |
| `version` | string | Auto-populated version | (leave empty) |
| `author` | string | Auto-populated last author | (leave empty) |
| `effective_date` | string | Auto-populated date | (leave empty) |
| `type` | string | Always "Prompt" | `"Prompt"` |
| `category` | string | Prompt category | `"extraction"`, `"generation"`, or `"aggregation"` |
| `level` | string | **Hierarchy level** | `"system"`, `"module"`, or `"item"` |
| `standard` | string | Primary regulatory standard | `"IEC 62304"` |
| `clause` | string | Standard clause(s) covered | `"5.2"` or `"5.2, 5.3"` |
| `inputs` | array | Required input artifacts | `["source code", "package.json"]` |
| `outputs` | array | Output artifact names | `["requirements.json"]` |
| `software_class` | string | Applicable safety classes | `"all"` or `"B,C"` or `"C"` |
| `process` | string | Link to process canvas | `"[Process Name](path)"` |
| `requirements` | string | Link to requirements doc | (optional) |
| `owner` | string | Link to responsible role | `"[Role](path)"` |

### Frontmatter Example (Item-Level Prompt)

```yaml
---
id: 725af57
title: "_prompt schema"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304"
clause: "5.2"
inputs: ["source code", "type definitions", "API specs", "config files"]
outputs: ["item-requirements.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---
```

### Frontmatter Example (System-Level Aggregation Prompt)

```yaml
---
id: 725af57
title: "_prompt schema"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "aggregation"
level: "system"
standard: "ISO 14971"
clause: "4.5"
inputs: ["module-a/item-risks.json", "module-b/item-risks.json", "system-requirements.json"]
outputs: ["product-risk-file.json", "RMR.md"]
software_class: "all"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---
```

## Section Specifications

### Context Section

**Purpose:** Explain why this prompt exists and its regulatory significance.

**Required Content:**
- Regulatory standard and clause reference
- Purpose of the extraction/generation
- Relationship to other prompts (if applicable)
- Code-as-truth paradigm explanation

**Example:**
```markdown
## Context

IEC 62304 clause 5.2 requires manufacturers to document software requirements.
In a code-as-truth paradigm, requirements are extracted from implemented code,
ensuring documentation reflects actual product capabilities.

This prompt analyzes source code to identify and document functional,
interface, and non-functional requirements that exist in the implementation.
```

### Inputs Section

**Purpose:** Define what artifacts the AI worker needs.

**Structure:**
```markdown
## Inputs

### Required
- **Artifact name**: Description of what it contains

### Optional (enriches output)
- **Artifact name**: Description and how it improves output

### From Previous Extraction
- `filename.json` — Description of dependency
```

**Guidelines:**
- List all required inputs explicitly
- Distinguish required from optional
- Reference outputs from other prompts when there are dependencies
- Include file patterns where applicable (e.g., `src/**/*.ts`)

### Instructions Section

**Purpose:** Step-by-step guidance for the AI worker.

**Structure:**
```markdown
## Instructions

### 1. [First Major Step] (Clause Reference)

[Detailed instructions]

- Sub-step a
- Sub-step b

### 2. [Second Major Step] (Clause Reference)

[Detailed instructions]
```

**Guidelines:**
- Number major steps
- Reference standard clauses in step headers
- Be specific about what to look for in code
- Include code patterns to search for
- Specify how to handle missing or ambiguous information

### Output Schema Section

**Purpose:** Define the exact JSON or Markdown structure expected.

**For Extraction Prompts:**
```markdown
## Output Schema

Generate a JSON file with this structure:

\`\`\`json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0",
    "standard": "<standard-name>"
  },

  "section_name": [
    {
      "id": "<PREFIX-seq>",
      "field1": "<description>",
      "field2": "<description>",
      "source": {
        "files": ["<file-path>:<line-range>"]
      }
    }
  ]
}
\`\`\`
```

**For Generation Prompts:**
```markdown
## Output Schema

Generate a markdown document with this structure:

\`\`\`markdown
---
[frontmatter]
---

# Document Title

## Section 1
[content instructions]

## Section 2
[content instructions]
\`\`\`
```

**Guidelines:**
- Use consistent field naming across prompts
- Always include `extraction_metadata` for extraction prompts
- Always include `source` references for traceability
- Use consistent ID prefixes (see ID Conventions below)
- Include placeholder descriptions in angle brackets

### Compliance Mapping Section

**Purpose:** Show how output elements satisfy regulatory requirements.

**Structure:**
```markdown
## Compliance Mapping

| Output Element | Standard Clause | Evidence Provided |
|----------------|-----------------|-------------------|
| `field_name` | X.X.X | Description of compliance evidence |
```

**Guidelines:**
- Map every significant output field to a clause
- Use JSON path notation for nested fields (e.g., `requirements[].type`)
- Describe what compliance evidence the field provides

### Examples Section

**Purpose:** Provide concrete input/output examples.

**Structure:**
```markdown
## Examples

### Input: [Artifact Type]

\`\`\`[language]
[example input content]
\`\`\`

### Output: [Artifact Type] (partial)

\`\`\`[language]
[example output content]
\`\`\`
```

**Guidelines:**
- Use realistic examples from health software domain
- Show both input and corresponding output
- Mark partial examples as "(partial)"
- Include edge cases if relevant

### Validation Criteria Section

**Purpose:** Checklist for verifying output quality.

**Structure:**
```markdown
## Validation Criteria

- [ ] Criterion 1 (relates to clause X.X) 🆔 26b1Vc
- [ ] Criterion 2 🆔 JjofkS
- [ ] Criterion 3 🆔 gOAIq4
```

**Guidelines:**
- Use checkbox format for easy verification
- Reference standard clauses where applicable
- Cover completeness, accuracy, and traceability
- Include both extraction quality and compliance criteria

## ID Conventions

### Hierarchical ID Structure

IDs must include the hierarchy level to ensure uniqueness across the system:

```
<LEVEL>-<ITEM>-<TYPE>-<SEQ>

Examples:
  ITEM-HM-REQ-FUNC-001     (health-manager item requirement)
  ITEM-HM-ARCH-001         (health-manager architecture component)
  MOD-SCP-INT-001          (smart-care-plans module interface)
  SYS-P24-USE-001          (platform24 system use requirement)
```

### Item-Level IDs (Repository Scope)

| Category | Prefix Pattern | Example |
|----------|----------------|---------|
| Item Requirement | `ITEM-<item>-REQ-<type>-` | `ITEM-HM-REQ-FUNC-001` |
| Item Architecture | `ITEM-<item>-ARCH-` | `ITEM-HM-ARCH-001` |
| Item SOUP | `ITEM-<item>-SOUP-` | `ITEM-HM-SOUP-001` |
| Item Test | `ITEM-<item>-TC-` | `ITEM-HM-TC-UNIT-001` |
| Item Hazard Contribution | `ITEM-<item>-HAZ-` | `ITEM-HM-HAZ-001` |
| Item Risk Control | `ITEM-<item>-RC-` | `ITEM-HM-RC-001` |
| Item Gap | `ITEM-<item>-GAP-` | `ITEM-HM-GAP-001` |

### Module-Level IDs (Multi-Repository Scope)

| Category | Prefix Pattern | Example |
|----------|----------------|---------|
| Module Interface | `MOD-<mod>-INT-` | `MOD-SCP-INT-001` |
| Module Integration Test | `MOD-<mod>-IT-` | `MOD-SCP-IT-001` |
| Module Risk | `MOD-<mod>-RISK-` | `MOD-SCP-RISK-001` |
| Cross-Item Gap | `MOD-<mod>-GAP-` | `MOD-SCP-GAP-001` |

### System-Level IDs (Product Scope)

| Category | Prefix Pattern | Example |
|----------|----------------|---------|
| Use Requirement | `SYS-<sys>-USE-` | `SYS-P24-USE-001` |
| System Requirement | `SYS-<sys>-SYS-` | `SYS-P24-SYS-INT-001` |
| Product Hazard | `SYS-<sys>-HAZ-` | `SYS-P24-HAZ-001` |
| Hazardous Situation | `SYS-<sys>-HS-` | `SYS-P24-HS-001` |
| System Risk Control | `SYS-<sys>-RC-` | `SYS-P24-RC-001` |
| Residual Risk | `SYS-<sys>-RR-` | `SYS-P24-RR-001` |
| Validation Activity | `SYS-<sys>-VAL-` | `SYS-P24-VAL-001` |

### Document IDs by Level

| Level | Document Type | ID Pattern | Example |
|-------|---------------|------------|---------|
| **Item** | Item Requirements | `ITEM-REQ-<item>-<ver>` | `ITEM-REQ-health-manager-1.0` |
| **Item** | Item Architecture | `ITEM-SAD-<item>-<ver>` | `ITEM-SAD-health-manager-1.0` |
| **Item** | Item SOUP List | `ITEM-SOUP-<item>-<ver>` | `ITEM-SOUP-health-manager-1.0` |
| **Item** | Item Verification | `ITEM-SVR-<item>-<ver>` | `ITEM-SVR-health-manager-1.0` |
| **Item** | Item Risk Contribution | `ITEM-RISK-<item>-<ver>` | `ITEM-RISK-health-manager-1.0` |
| **Module** | Module Architecture | `MOD-SAD-<mod>-<ver>` | `MOD-SAD-smart-care-plans-1.0` |
| **Module** | Module Integration Report | `MOD-INT-<mod>-<ver>` | `MOD-INT-smart-care-plans-1.0` |
| **System** | System SRS | `SYS-SRS-<sys>-<ver>` | `SYS-SRS-platform24-2.0` |
| **System** | Risk Management Plan | `SYS-RMP-<sys>-<ver>` | `SYS-RMP-platform24-2.0` |
| **System** | Risk Management Report | `SYS-RMR-<sys>-<ver>` | `SYS-RMR-platform24-2.0` |
| **System** | Validation Report | `SYS-VAL-<sys>-<ver>` | `SYS-VAL-platform24-2.0` |
| **System** | Instructions for Use | `SYS-IFU-<sys>-<ver>` | `SYS-IFU-platform24-2.0` |
| **System** | Technical Description | `SYS-TD-<sys>-<ver>` | `SYS-TD-platform24-2.0` |

## Extraction Metadata Standard

All extraction outputs must include this metadata block with **hierarchy context**:

```json
{
  "extraction_metadata": {
    "repository": "<git remote URL or local path>",
    "commit": "<full commit hash>",
    "extracted_at": "<ISO-8601 timestamp with timezone>",
    "extractor_version": "1.0",
    "standard": "<standard name and version>",
    "prompt_version": "<prompt file version if available>",

    "hierarchy": {
      "level": "item | module | system",
      "item_id": "<software-item-identifier>",
      "item_name": "<human-readable name>",
      "parent_module": "<module-identifier or null>",
      "parent_system": "<system-identifier or null>"
    }
  }
}
```

### Hierarchy Context Examples

**Software Item Extraction** (e.g., health-manager repository):
```json
{
  "extraction_metadata": {
    "repository": "https://gitlab.com/platform24/health-manager",
    "commit": "16e0273739eda65f73af5410323e1caf53362f16",
    "extracted_at": "2026-03-06T10:00:00Z",
    "extractor_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "item",
      "item_id": "health-manager",
      "item_name": "Health Manager Service",
      "parent_module": "smart-care-plans",
      "parent_system": "platform24"
    }
  }
}
```

**Module Aggregation** (aggregating multiple items):
```json
{
  "extraction_metadata": {
    "aggregated_at": "2026-03-06T12:00:00Z",
    "aggregator_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "module",
      "module_id": "smart-care-plans",
      "module_name": "Smart Care Plans Module",
      "parent_system": "platform24",
      "child_items": ["health-manager", "appointment-api", "notification-service"]
    },
    "source_extractions": [
      {"item_id": "health-manager", "commit": "16e027...", "extracted_at": "..."},
      {"item_id": "appointment-api", "commit": "abc123...", "extracted_at": "..."}
    ]
  }
}
```

**System Aggregation** (aggregating modules):
```json
{
  "extraction_metadata": {
    "aggregated_at": "2026-03-06T14:00:00Z",
    "aggregator_version": "2.0",
    "standard": "IEC 82304-1:2016",
    "hierarchy": {
      "level": "system",
      "system_id": "platform24",
      "system_name": "Platform24 Health Software",
      "child_modules": ["smart-care-plans", "booking-service", "patient-portal"]
    },
    "source_aggregations": [
      {"module_id": "smart-care-plans", "aggregated_at": "..."},
      {"module_id": "booking-service", "aggregated_at": "..."}
    ]
  }
}
```

## Source Reference Format

All extracted items should include source references:

```json
{
  "source": {
    "files": ["<path>:<start-line>-<end-line>"],
    "commits": ["<commit-hash>"],
    "prs": ["<pr-number>"],
    "issues": ["<issue-number>"]
  }
}
```

**Guidelines:**
- Always include at least one file reference
- Line ranges use format `path:start-end` (e.g., `src/auth.ts:45-67`)
- Commits, PRs, and issues are optional but valuable for traceability

## Traceability Requirements

Extraction outputs should include traceability sections:

```json
{
  "traceability": {
    "to_parent": [
      {
        "item_id": "<this item's ID>",
        "parent_ids": ["<parent item IDs>"]
      }
    ],
    "to_child": [
      {
        "item_id": "<this item's ID>",
        "child_ids": ["<child item IDs>"]
      }
    ],
    "gaps": [
      {
        "parent_id": "<unlinked parent ID>",
        "gap": "<description of missing link>",
        "recommendation": "<suggested action>"
      }
    ]
  }
}
```

## Gap Identification

All extraction prompts should identify gaps:

```json
{
  "gaps": [
    {
      "gap_id": "GAP-<seq>",
      "type": "<gap type>",
      "description": "<what is missing>",
      "affected_items": ["<item IDs>"],
      "clause": "<standard clause>",
      "recommendation": "<suggested remediation>",
      "priority": "high | medium | low"
    }
  ]
}
```

**Gap Types:**
- `missing_requirement` — Required element not found in code
- `untested` — Code exists but no test coverage
- `undocumented` — Code exists but no documentation
- `incomplete_traceability` — Missing links in trace chain
- `uncontrolled_hazard` — Risk without control measure

## Outputs by Hierarchy Level

### Item Level (Single Repository)

**What to extract:**
- Software requirements implemented in this codebase
- Architecture components within this codebase
- SOUP dependencies used by this codebase
- Tests and verification evidence from this codebase
- **Hazard contributions** — risks this item can cause or propagate

**What NOT to generate at item level:**
- ❌ Full Risk Management Report (RMR) — system level
- ❌ Full Risk Management Plan (RMP) — system level
- ❌ Instructions for Use (IFU) — system level
- ❌ Validation Report — system level
- ❌ Technical Description — system level

**Item-level outputs:**

| Output | Purpose | Aggregates To |
|--------|---------|---------------|
| `item-requirements.json` | Requirements implemented in this item | Module/System SRS |
| `item-architecture.json` | Components, interfaces, dependencies | Module/System SAD |
| `item-soup.json` | Third-party dependencies | System SOUP List |
| `item-verification.json` | Unit tests, integration tests | System SVR |
| `item-risk-contribution.json` | Hazards, controls, failure modes | System Risk File |
| `Item-Requirements.md` | Human-readable requirements doc | — |
| `Item-Architecture.md` | Human-readable architecture doc | — |
| `Item-SOUP-List.md` | Human-readable SOUP doc | — |
| `Item-Verification.md` | Human-readable test summary | — |
| `Item-Risk-Contribution.md` | Human-readable risk contribution | — |

### Module Level (Multi-Repository Component)

**What to aggregate:**
- Item requirements from child repositories
- Item architectures showing inter-item interfaces
- Combined SOUP with deduplication
- Integration test evidence
- Combined risk contributions

**Module-level outputs:**

| Output | Purpose | Aggregates To |
|--------|---------|---------------|
| `module-architecture.json` | Inter-item interfaces, data flows | System Architecture |
| `module-integration.json` | Integration test results | System Validation |
| `module-risk.json` | Cross-item hazards, module-level controls | System Risk File |
| `Module-Architecture.md` | Module architecture overview | — |
| `Module-Integration-Report.md` | Integration test summary | — |

### System Level (Complete Product)

**What to own:**
- Use requirements (what users need)
- System requirements (product behavior)
- Product-level risk management (RMP, RMR)
- Validation against use requirements
- User-facing documentation (IFU, Technical Description)

**What to aggregate:**
- All module outputs
- All item outputs (if no module layer)

**System-level outputs:**

| Output | Purpose | Standard |
|--------|---------|----------|
| `use-requirements.json` | User needs | IEC 82304-1 4.2 |
| `system-requirements.json` | Product behavior | IEC 82304-1 4.5 |
| `product-risk-file.json` | Aggregated risk data | ISO 14971 |
| `validation-evidence.json` | Validation results | IEC 82304-1 6 |
| `System-SRS.md` | System requirements specification | IEC 82304-1 |
| `RMP.md` | Risk Management Plan | ISO 14971 4.4 |
| `Risk-Analysis.md` | Full risk analysis | ISO 14971 5-7 |
| `RMR.md` | Risk Management Report | ISO 14971 8-9 |
| `Validation-Report.md` | Product validation | IEC 82304-1 6.3 |
| `IFU.md` | Instructions for Use | IEC 82304-1 7.2.2 |
| `Technical-Description.md` | Technical description | IEC 82304-1 7.2.3 |

## Prompt Naming Conventions

**Item-Level Prompts:**
```
item-extraction-<standard>-<artifact>.md
item-generation-<standard>-<document>.md
```
Examples:
- `item-extraction-62304-requirements.md`
- `item-extraction-62304-risk-contribution.md`
- `item-generation-62304-item-srs.md`

**Module-Level Prompts:**
```
module-aggregation-<artifact>.md
module-generation-<document>.md
```
Examples:
- `module-aggregation-architecture.md`
- `module-aggregation-risks.md`
- `module-generation-integration-report.md`

**System-Level Prompts:**
```
system-extraction-<standard>-<artifact>.md
system-aggregation-<artifact>.md
system-generation-<standard>-<document>.md
```
Examples:
- `system-extraction-82304-use-requirements.md`
- `system-aggregation-risk-file.md`
- `system-generation-14971-rmr.md`
- `system-generation-82304-ifu.md`

**Index/Support Documents:**
```
_<name>.md
```
Examples:
- `_orchestration.md`
- `_unified-coverage-matrix.md`
- `_prompt-schema.md`

## Execution Modes

### Claude Code CLI

Prompts can be invoked as part of Claude Code sessions:
```bash
# Run extraction for a repository
claude "Execute extraction-62304-requirements.md against this repository.
Output to docs/extracted/requirements.json"

# Run generation from extracted data
claude "Execute generation-62304-srs.md using docs/extracted/requirements.json.
Output to docs/regulatory/SRS.md"
```

### GitHub Actions

Prompts can be triggered via workflows:
```yaml
on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

jobs:
  extract:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run extraction
        run: |
          # Invoke Claude with extraction prompt
```

**Recommended Triggers:**
| Trigger | Action |
|---------|--------|
| `push` to main | Incremental extraction + generation |
| `pull_request` | Preview generation (no commit) |
| Release tag | Full extraction + generation + archive |
| Weekly schedule | SOUP vulnerability check |
| Manual dispatch | Full regeneration |

## Version Compatibility

Prompts should specify compatibility in inputs:

```yaml
inputs: ["requirements.json (v1.0+)"]
```

When output schemas change:
1. Increment `extractor_version` in metadata
2. Update dependent generation prompts
3. Document breaking changes in prompt

## Quality Checklist for New Prompts

Before adding a new prompt, verify:

- [ ] Frontmatter follows schema exactly 🆔 X944KV
- [ ] Context explains regulatory purpose 🆔 PtSf7G
- [ ] Inputs list all required artifacts 🆔 GPV8Dx
- [ ] Instructions are numbered and reference clauses 🆔 akyQ08
- [ ] Output schema is complete JSON/Markdown 🆔 OYEJcY
- [ ] Compliance mapping covers all significant fields 🆔 axfFeE
- [ ] Examples show realistic input/output 🆔 KDICYH
- [ ] Validation criteria are checkable 🆔 cda0aK
- [ ] ID prefixes follow conventions 🆔 dkSc2J
- [ ] Source references are specified 🆔 VjX5os
- [ ] Traceability section is included 🆔 HH5SfY
- [ ] Gap identification is specified 🆔 T10ikX
- [ ] File naming follows conventions 🆔 qxBBPu

## Item-Level Risk Contribution

A key concept for hierarchical documentation is that **software items contribute to system-level risks** but do not own the complete risk management process.

### What Item-Level Risk Extraction Captures

```json
{
  "item_risk_contribution": {
    "item_id": "health-manager",

    "hazard_contributions": [
      {
        "id": "ITEM-HM-HAZ-001",
        "description": "Appointment handover failure",
        "cause": "State machine error in handover workflow",
        "potential_harm": "Patient seen by wrong practitioner",
        "severity_contribution": "serious",
        "source": {"files": ["HandoverService.java:145-200"]}
      }
    ],

    "risk_controls_implemented": [
      {
        "id": "ITEM-HM-RC-001",
        "description": "Handover state validation",
        "control_type": "protective_measure",
        "implementation": "State machine validates all transitions",
        "verification_status": "verified",
        "source": {"files": ["HandoverStateMachine.java:50-80"]}
      }
    ],

    "risk_controls_required_upstream": [
      {
        "description": "User training on handover confirmation",
        "rationale": "Software cannot force user to read handover notes",
        "recommended_level": "system",
        "control_type": "information_for_safety"
      }
    ],

    "failure_modes": [
      {
        "id": "ITEM-HM-FM-001",
        "component": "NotificationClient",
        "failure_mode": "Notification delivery timeout",
        "effect": "Practitioner not notified of handover",
        "detection": "Timeout logged, retry attempted",
        "source": {"files": ["NotificationClient.java:89-120"]}
      }
    ],

    "interfaces_with_safety_impact": [
      {
        "interface": "NotifierService REST API",
        "direction": "outbound",
        "safety_impact": "Notification failure affects clinical awareness",
        "failure_handling": "Retry with exponential backoff, log failure"
      }
    ]
  }
}
```

### What Item-Level Risk Does NOT Include

- ❌ Overall residual risk acceptability (system decision)
- ❌ Benefit-risk analysis (system/clinical decision)
- ❌ Risk acceptability criteria (defined in system RMP)
- ❌ Risk management review conclusions (system RMR)
- ❌ Post-market surveillance plan (system scope)

### Aggregation to System Level

The system-level risk file aggregates item contributions:

```json
{
  "system_risk_file": {
    "system_id": "platform24",

    "risk_acceptability_criteria": { /* defined at system level */ },

    "hazards": [
      {
        "id": "SYS-P24-HAZ-001",
        "description": "Patient receives wrong care",
        "contributing_items": [
          {"item": "health-manager", "contribution": "ITEM-HM-HAZ-001"},
          {"item": "booking-service", "contribution": "ITEM-BS-HAZ-003"}
        ]
      }
    ],

    "risk_controls": [
      /* Aggregated from items + system-level controls */
    ],

    "residual_risk_evaluation": { /* System-level assessment */ },

    "benefit_risk_analysis": { /* System-level clinical justification */ }
  }
}
```

## Current Prompt Inventory

### Item-Level Prompts (Repository Scope)

| Prompt | Level | Standard | Output |
|--------|-------|----------|--------|
| `item-extraction-62304-requirements.md` | item | IEC 62304 5.2 | `item-requirements.json` |
| `item-extraction-62304-architecture.md` | item | IEC 62304 5.3 | `item-architecture.json` |
| `item-extraction-62304-soup.md` | item | IEC 62304 8.1 | `item-soup.json` |
| `item-extraction-62304-verification.md` | item | IEC 62304 5.5-5.7 | `item-verification.json` |
| `item-extraction-62304-risk-contribution.md` | item | IEC 62304 7 | `item-risk-contribution.json` |
| `item-generation-62304-requirements.md` | item | IEC 62304 | Item-Requirements.md |
| `item-generation-62304-architecture.md` | item | IEC 62304 | Item-Architecture.md |
| `item-generation-62304-soup-list.md` | item | IEC 62304 | Item-SOUP-List.md |
| `item-generation-62304-verification.md` | item | IEC 62304 | Item-Verification.md |
| `item-generation-62304-risk-contribution.md` | item | IEC 62304 | Item-Risk-Contribution.md |

### Module-Level Prompts (Multi-Item Scope)

| Prompt | Level | Standard | Output |
|--------|-------|----------|--------|
| `module-aggregation-architecture.md` | module | IEC 62304 | `module-architecture.json` |
| `module-aggregation-soup.md` | module | IEC 62304 | `module-soup.json` |
| `module-aggregation-risks.md` | module | IEC 62304 7 | `module-risk.json` |
| `module-aggregation-verification.md` | module | IEC 62304 | `module-verification.json` |
| `module-generation-architecture.md` | module | IEC 62304 | Module-Architecture.md |
| `module-generation-integration-report.md` | module | IEC 62304 | Module-Integration.md |

### System-Level Prompts (Product Scope)

| Prompt | Level | Standard | Output |
|--------|-------|----------|--------|
| `system-extraction-82304-use-requirements.md` | system | IEC 82304-1 4.2 | `use-requirements.json` |
| `system-extraction-82304-system-requirements.md` | system | IEC 82304-1 4.5 | `system-requirements.json` |
| `system-aggregation-soup.md` | system | IEC 62304 8 | `system-soup.json` |
| `system-aggregation-risk-file.md` | system | ISO 14971 | `product-risk-file.json` |
| `system-aggregation-verification.md` | system | IEC 62304 | `system-verification.json` |
| `system-generation-82304-srs.md` | system | IEC 82304-1 | System-SRS.md |
| `system-generation-14971-rmp.md` | system | ISO 14971 4.4 | RMP.md |
| `system-generation-14971-rma.md` | system | ISO 14971 5-7 | Risk-Analysis.md |
| `system-generation-14971-rmr.md` | system | ISO 14971 8-9 | RMR.md |
| `system-generation-82304-validation.md` | system | IEC 82304-1 6 | Validation-Report.md |
| `system-generation-82304-ifu.md` | system | IEC 82304-1 7.2.2 | IFU.md |
| `system-generation-82304-technical-description.md` | system | IEC 82304-1 7.2.3 | Technical-Description.md |

### Legacy Prompts (Flat Structure - Deprecated)

The following prompts treat a repository as a complete system. They are retained for single-module products but should not be used for multi-module systems:

| Prompt | Status | Replacement |
|--------|--------|-------------|
| `extraction-62304-requirements.md` | deprecated | `item-extraction-62304-requirements.md` |
| `extraction-14971-risks.md` | deprecated | `item-extraction-62304-risk-contribution.md` + `system-aggregation-risk-file.md` |
| `generation-14971-rmr.md` | deprecated | `system-generation-14971-rmr.md` |

### Index Documents

| Document | Purpose |
|----------|---------|
| `_prompt-schema.md` | This document - prompt structure definition |
| `_orchestration.md` | Workflow phases and dependencies |
| `_unified-coverage-matrix.md` | Cross-standard coverage |
| `_62304-coverage-matrix.md` | IEC 62304 clause coverage |
| `_82304-coverage-matrix.md` | IEC 82304-1 clause coverage |
| `_14971-coverage-matrix.md` | ISO 14971 clause coverage |
| `_documentation-flow-diagram.md` | System/module/item hierarchy diagrams |
