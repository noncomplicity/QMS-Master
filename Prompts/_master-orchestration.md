---
id:
title: "Master Orchestration Prompt"
version:
author:
effective_date:
type: "Prompt"
category: "orchestration"
level: "system"
standard: "IEC 62304, ISO 14971, IEC 82304-1"
inputs: ["system-manifest.json", "source repositories"]
outputs: ["complete documentation hierarchy"]
software_class: "all"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Master Orchestration Prompt

This prompt coordinates the complete code-as-truth documentation workflow.

## Critical Principle

> **This framework documents what EXISTS in code.**
> **It does NOT report execution results, pass/fail status, or coverage metrics.**

See `_documentation-vs-execution.md` for details.

## Prerequisites

Before running this orchestration:

1. **System manifest exists** at `Output/<system-id>/system-manifest.json`
2. **Source repositories** are accessible at paths specified in manifest
3. **Output directories** will be created automatically

## Inputs

You must provide:
- **system_id**: The system identifier (e.g., "platform24")
- **manifest_path**: Path to system-manifest.json
- **Repository access**: Either local paths or git URLs

## Workflow Phases

```
PHASE 1: ITEM EXTRACTION (automated)
    For each item in manifest:
    ├── item-requirements.json
    ├── item-architecture.json
    ├── item-soup.json
    ├── item-verification.json
    └── item-risk-contribution.json
         └── software_item definition
         └── failure_modes with categories

PHASE 2: ITEM GENERATION (automated)
    For each item:
    ├── Item-Requirements.md
    ├── Item-Architecture.md
    ├── Item-SOUP-List.md
    ├── Item-Verification.md
    └── Item-Risk-Contribution.md

PHASE 3: MODULE AGGREGATION (automated)
    For each module:
    ├── module-architecture.json
    ├── module-soup.json
    ├── module-risk.json
    │    └── software_items[]
    │    └── core_functionalities[]
    │    └── failure_modes[] (enhanced)
    └── module-verification.json

PHASE 4: GAP ANALYSIS (automated → operator review)
    ├── risk-data-gaps.json
    │    └── What's extracted vs what's missing
    │    └── Templates for operator input
    └── risk-data-gaps.md (human-readable report)

PHASE 5: OPERATOR INPUT (manual)
    Output/<system>/system/defined/
    ├── intended-use.json        ← USE perspective
    ├── safety-characteristics.json ← ISO 14971 Annex C
    ├── risk-criteria.json       ← Severity/probability matrix
    └── hazard-assessments.json  ← Clinical risk assessments

PHASE 6: SYSTEM AGGREGATION (automated, requires Phase 5)
    ├── system-architecture.json
    ├── system-soup.json
    ├── system-risk-file.json
    │    └── safety_characteristics[]
    │    └── intended_uses[]
    │    └── user_intents[]
    │    └── Complete traceability chains
    └── system-verification.json

PHASE 7: SYSTEM GENERATION (optional)
    ├── RMP.md
    ├── RMR.md
    ├── System-SOUP-List.md
    └── System-Verification-Report.md
```

## Automation vs Manual Input

| Phase | Automated? | Input Required |
|-------|------------|----------------|
| 1. Item Extraction | Yes | Repository access |
| 2. Item Generation | Yes | Phase 1 outputs |
| 3. Module Aggregation | Yes | Phase 1 outputs |
| 4. Gap Analysis | Yes | Phases 1+3 outputs |
| 5. Operator Input | **No** | Clinical/regulatory expertise |
| 6. System Aggregation | Yes | Phases 3+5 outputs |
| 7. System Generation | Yes | Phase 6 outputs |

**Key insight:** Phases 1-4 can run fully automated. Phase 5 requires human expertise. Phase 6-7 resume automation after operator input.

## Instructions

### Phase 1: Item Extraction

For each software item defined in the system manifest:

1. **Read the item's repository** at the path specified in `items[].repository`

2. **Run 5 extraction prompts** (can run in parallel):

   a) `item-extraction-62304-requirements.md`
      - Analyze source code, interfaces, DTOs
      - Output: `items/<item>/extracted/item-requirements.json`

   b) `item-extraction-62304-architecture.md`
      - Analyze package structure, components, dependencies
      - Output: `items/<item>/extracted/item-architecture.json`

   c) `item-extraction-62304-soup.md`
      - Analyze dependency manifests (pom.xml, package.json, etc.)
      - Output: `items/<item>/extracted/item-soup.json`

   d) `item-extraction-62304-verification.md`
      - Analyze test files, document test cases
      - Output: `items/<item>/extracted/item-verification.json`
      - **IMPORTANT**: Document test cases, NOT execution results

   e) `item-extraction-62304-risk-contribution.md`
      - Analyze safety-critical code, error handling
      - Output: `items/<item>/extracted/item-risk-contribution.json`

3. **Populate hierarchy metadata** in each extraction:
   ```json
   {
     "hierarchy": {
       "level": "item",
       "item_id": "<from manifest>",
       "item_name": "<from manifest>",
       "parent_module": "<from manifest>",
       "parent_system": "<system_id>"
     }
   }
   ```

### Phase 2: Item Generation

For each item with completed extractions:

1. **Run 5 generation prompts**:

   a) `item-generation-62304-requirements.md`
      - Input: item-requirements.json
      - Output: `items/<item>/generated/Item-Requirements.md`

   b) `item-generation-62304-architecture.md`
      - Input: item-architecture.json
      - Output: `items/<item>/generated/Item-Architecture.md`

   c) `item-generation-62304-soup-list.md`
      - Input: item-soup.json
      - Output: `items/<item>/generated/Item-SOUP-List.md`

   d) `item-generation-62304-verification.md`
      - Input: item-verification.json
      - Output: `items/<item>/generated/Item-Verification.md`

   e) `item-generation-62304-risk-contribution.md`
      - Input: item-risk-contribution.json
      - Output: `items/<item>/generated/Item-Risk-Contribution.md`

### Phase 3: Module Aggregation

For each module defined in the system manifest:

1. **Collect item extractions** for all items in `modules[].items`

2. **Run 4 aggregation prompts**:

   a) `module-aggregation-architecture.md`
      - Inputs: item-architecture.json from each item
      - Output: `modules/<module>/aggregated/module-architecture.json`

   b) `module-aggregation-soup.md`
      - Inputs: item-soup.json from each item
      - Output: `modules/<module>/aggregated/module-soup.json`

   c) `module-aggregation-risks.md`
      - Inputs: item-risk-contribution.json from each item
      - Output: `modules/<module>/aggregated/module-risk.json`

   d) `module-aggregation-verification.md`
      - Inputs: item-verification.json from each item
      - Output: `modules/<module>/aggregated/module-verification.json`

### Phase 4: Gap Analysis

After module aggregation, identify what operator input is needed:

1. **Run gap analysis prompt**:

   `analysis/gap-analysis-risk-data.md`
   - Inputs: All item and module extractions
   - Outputs:
     - `system/gaps/risk-data-gaps.json` — Structured gap report with templates
     - `system/gaps/risk-data-gaps.md` — Human-readable report

2. **Gap analysis identifies**:
   - What was successfully extracted from code
   - What's missing and requires operator input:
     - Intended uses and user intents (USE perspective)
     - Safety characteristics (ISO 14971 Annex C)
     - Risk acceptability criteria
     - Severity/probability assessments
     - Core functionality → user intent mappings
   - Validation issues in existing extractions
   - Recommended actions prioritized by importance

3. **Review gap report** before proceeding to Phase 5

### Phase 5: Operator Input (MANUAL)

This phase requires human expertise and cannot be automated.

1. **Copy templates** from `Templates/risk-data-inputs/` to `Output/<system>/system/defined/`

2. **Fill required templates**:

   a) `intended-use.json` (from `intended-use-template.json`)
      - Product context and description
      - Intended uses (clinical purposes)
      - User intents (what users accomplish)
      - Core functionality mappings
      - **Requires:** Product owner, clinical expert

   b) `safety-characteristics.json` (from `safety-characteristics-template.json`)
      - ISO 14971 Annex C characteristic assessments
      - Applicability decisions with rationale
      - Hazards identified through each characteristic
      - **Requires:** Risk manager, clinical expert

   c) `risk-criteria.json` (from `risk-criteria-template.json`)
      - Severity levels (S1-S5) with definitions
      - Probability levels (P1-P5) with definitions
      - Risk matrix (acceptable/ALARP/unacceptable)
      - **Requires:** Risk manager, QA
      - **Note:** Can be reused across projects in same organization

   d) `hazard-assessments.json` (from `hazard-assessments-template.json`)
      - Clinical context for each hazard
      - Severity assessment with rationale
      - Probability assessment before/after controls
      - Benefit-risk analysis where needed
      - **Requires:** Clinical expert, risk manager

3. **Re-run gap analysis** to verify completeness before Phase 6

4. **Proceed only when** `readiness_assessment.can_run_system_aggregation` is `true`

### Phase 6: System Aggregation

After operator input is complete:

1. **Verify prerequisites**:
   - All module aggregations exist
   - All required `system/defined/` files exist
   - Gap analysis shows readiness = true

2. **Collect inputs**:
   - Module aggregations from all modules
   - Operator-defined files from `system/defined/`

3. **Run aggregation prompts**:

   a) `system-aggregation-soup.md`
      - Inputs: module-soup.json from each module
      - Output: `system/aggregated/system-soup.json`

   b) `system-aggregation-risk-file.md`
      - Inputs: module-risk.json + system/defined/*.json
      - Output: `system/aggregated/system-risk-file.json`
      - **Contains:** Complete risk data for visualizer

   c) `system-aggregation-verification.md`
      - Inputs: module-verification.json from each module
      - Output: `system/aggregated/system-verification.json`

   d) `system-aggregation-architecture.md`
      - Aggregate module architectures
      - Output: `system/aggregated/system-architecture.json`

### Phase 7: System Generation (Optional)

Generate system-level regulatory documents:

1. Risk Management Plan (RMP.md)
2. Risk Management Report (RMR.md)
3. System SOUP List
4. System Verification Report
5. Other documents as needed

## Execution Modes

### Full Automated Extraction (Phases 1-4)

Run all automated phases up to gap analysis:

```
"Run automated extraction for <system_id>.
Manifest: <manifest_path>
Execute Phases 1-4 (Item Extraction → Module Aggregation → Gap Analysis).
Output to: Output/<system_id>/
Stop at gap analysis - operator input needed."
```

### Gap Analysis Only (Phase 4)

Run gap analysis after manual review of extractions:

```
"Run gap analysis for <system_id>.
Analyze: items/*/extracted/ and modules/*/aggregated/
Output: system/gaps/risk-data-gaps.json"
```

### System Aggregation (Phase 6)

Run after operator input is complete:

```
"Run system aggregation for <system_id>.
Prerequisites: system/defined/*.json files exist
Verify gap analysis shows readiness = true
Execute Phase 6."
```

### Complete Workflow (Phases 1-7)

Full execution with pause for operator input:

```
"Run complete workflow for <system_id>.

AUTOMATED:
- Phase 1: Item Extraction
- Phase 2: Item Generation
- Phase 3: Module Aggregation
- Phase 4: Gap Analysis

>>> PAUSE FOR OPERATOR INPUT <<<
- Operator fills system/defined/*.json

RESUME AUTOMATED:
- Phase 6: System Aggregation
- Phase 7: System Generation"
```

### Incremental Execution

Re-run specific items after code changes:

```
"Re-extract <item_id>.
Repository: <repo_path>
Then re-aggregate affected module.
Re-run gap analysis to check if operator input needs update."
```

## Progress Tracking

Use TodoWrite to track progress:

```
Phase 1: Item Extraction (AUTOMATED)
- [ ] health-manager: requirements 🆔 M4Cknc
- [ ] health-manager: architecture 🆔 9hCvDK
- [ ] health-manager: soup 🆔 L7Utbg
- [ ] health-manager: verification 🆔 TsJrJd
- [ ] health-manager: risk-contribution 🆔 jDqT7W

Phase 2: Item Generation (AUTOMATED)
- [ ] health-manager: all documents 🆔 IofjTR

Phase 3: Module Aggregation (AUTOMATED)
- [ ] smart-care-plans: all aggregations 🆔 ycTtuU

Phase 4: Gap Analysis (AUTOMATED)
- [ ] risk-data-gaps.json generated 🆔 FpfdvJ
- [ ] risk-data-gaps.md reviewed 🆔 eDgfOb

Phase 5: Operator Input (MANUAL)
- [ ] intended-use.json filled 🆔 ymr8wB
- [ ] safety-characteristics.json filled 🆔 0EGiyN
- [ ] risk-criteria.json filled 🆔 gLLamO
- [ ] hazard-assessments.json filled 🆔 mH2k2J
- [ ] Gap analysis re-run: readiness = true 🆔 vwGpMo

Phase 6: System Aggregation (AUTOMATED)
- [ ] system-soup.json 🆔 tJ3JOa
- [ ] system-risk-file.json 🆔 JCbSnk
- [ ] system-verification.json 🆔 AXlSxq
- [ ] system-architecture.json 🆔 GMkLPG

Phase 7: System Generation (AUTOMATED)
- [ ] RMP.md 🆔 TgvfNe
- [ ] RMR.md 🆔 Sjua7I
```

## Output Verification

After each phase, verify:

### Phase 1 Complete
```
Output/<system>/items/<item>/extracted/
├── item-requirements.json     ✓
├── item-architecture.json     ✓
├── item-soup.json             ✓
├── item-verification.json     ✓
└── item-risk-contribution.json ✓
    ├── software_item{}        ✓ (id, name, safety_class)
    └── failure_modes[]        ✓ (with category field)
```

### Phase 2 Complete
```
Output/<system>/items/<item>/generated/
├── Item-Requirements.md       ✓
├── Item-Architecture.md       ✓
├── Item-SOUP-List.md          ✓
├── Item-Verification.md       ✓
└── Item-Risk-Contribution.md  ✓
```

### Phase 3 Complete
```
Output/<system>/modules/<module>/aggregated/
├── module-architecture.json   ✓
├── module-soup.json           ✓
├── module-risk.json           ✓
│   ├── software_items[]       ✓
│   ├── core_functionalities[] ✓
│   └── failure_modes[]        ✓ (with core_functionality_id)
└── module-verification.json   ✓
```

### Phase 4 Complete
```
Output/<system>/system/gaps/
├── risk-data-gaps.json        ✓
│   ├── extraction_summary     ✓
│   ├── gaps_requiring_input   ✓
│   ├── recommended_actions    ✓
│   └── readiness_assessment   ✓
└── risk-data-gaps.md          ✓ (human-readable)
```

### Phase 5 Complete (Operator Input)
```
Output/<system>/system/defined/
├── intended-use.json          ✓ (USE perspective)
├── safety-characteristics.json ✓ (Annex C)
├── risk-criteria.json         ✓ (risk matrix)
└── hazard-assessments.json    ✓ (clinical assessments)

Gap analysis re-run shows:
└── readiness_assessment.can_run_system_aggregation: true
```

### Phase 6 Complete
```
Output/<system>/system/aggregated/
├── system-architecture.json   ✓
├── system-soup.json           ✓
├── system-risk-file.json      ✓
│   ├── safety_characteristics[] ✓
│   ├── intended_uses[]        ✓
│   ├── user_intents[]         ✓
│   ├── software_items[]       ✓
│   ├── core_functionalities[] ✓
│   ├── failure_modes[]        ✓
│   ├── system_hazards[]       ✓ (with failure_mode_ids)
│   ├── risk_controls{}        ✓
│   └── traceability{}         ✓ (complete chains)
└── system-verification.json   ✓
```

## Error Handling

### Missing Repository
- Log error: "Cannot access repository for <item_id>"
- Skip item, continue with others
- Add to gaps list

### Extraction Failure
- Save partial output if available
- Log specific failure reason
- Continue with other extractions
- Document in gaps

### Missing Dependencies
If aggregation inputs are missing:
- Log which inputs are missing
- Skip aggregation for that module/system
- Document as gap

## Quality Checks

After completion, verify:

1. **No execution metrics in verification files**
   - No `status: passed/failed`
   - No `coverage: X%`
   - No `last_executed` timestamps

2. **USE perspective traceability complete**
   - IntendedUse → UserIntent (every intent has a use)
   - UserIntent → CoreFunctionality (every intent has functionality)
   - CoreFunctionality → SoftwareItem (every functionality has code)

3. **RISK perspective traceability complete**
   - SafetyCharacteristic → FailureMode (characteristic identifies failures)
   - FailureMode → Hazard (failures cause hazards)
   - Hazard → RiskControl (hazards are controlled)
   - RiskControl → Verification (controls are verified)

4. **Failure mode categorization**
   - All failure modes have valid `category`
   - Categories map to appropriate control types

5. **IDs are consistent**
   - `ITEM-<item>-*` at item level
   - `MOD-<module>-*` at module level
   - `SYS-<system>-*` at system level
   - `CF-*` for core functionalities
   - `SC-*` for safety characteristics
   - `IU-*` / `UI-*` for intended uses / user intents

6. **Source references exist**
   - All extracted data has `source.files` references
   - Line numbers where applicable

7. **Operator input validation**
   - All applicable safety characteristics assessed
   - All hazards have severity/probability assessment
   - Risk acceptability criteria defined
   - Benefit-risk analysis where required

## Example Invocation

### Initial Automated Run (Phases 1-4)

```
Run the master orchestration for Platform24 - automated phases only.

System ID: platform24
Manifest: Output/platform24/system-manifest.json

Items to process:
- health-manager at /home/jakob/Noncomplicity/Repos/health-manager

Execute:
- Phase 1: Extract all 5 JSON files for health-manager
- Phase 2: Generate all 5 markdown documents
- Phase 3: Aggregate to smart-care-plans module
- Phase 4: Run gap analysis

Track progress with TodoWrite.
Stop after gap analysis - operator input needed.
```

### After Operator Input (Phase 6)

```
Resume master orchestration for Platform24.

Prerequisites verified:
- system/defined/intended-use.json exists
- system/defined/safety-characteristics.json exists
- system/defined/risk-criteria.json exists
- system/defined/hazard-assessments.json exists

Execute:
- Verify gap analysis readiness = true
- Phase 6: System aggregation with operator inputs
- Phase 7: Generate system documents

Output system-risk-file.json for visualizer.
```

### Complete Single-Session (with operator pause)

```
Run complete workflow for Platform24.

System ID: platform24
Manifest: Output/platform24/system-manifest.json
Repository: /home/jakob/Noncomplicity/Repos/health-manager

AUTOMATED (Phases 1-4):
1. Extract health-manager (requirements, architecture, soup, verification, risk)
2. Generate item documents
3. Aggregate to smart-care-plans module
4. Run gap analysis

>>> Output gap report and STOP <<<
>>> Operator must fill: system/defined/*.json <<<

RESUME (Phases 6-7):
5. Verify readiness
6. Aggregate to platform24 system (with operator inputs)
7. Generate system documents

Final output: system/aggregated/system-risk-file.json
```
