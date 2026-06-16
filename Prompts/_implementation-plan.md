---
id: 725af57
title: "_implementation plan"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Plan"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Code-as-Truth Implementation Plan

## Current State Assessment

### What Exists

**Meta-documentation (complete):**
- `_prompt-schema.md` — Schema with hierarchy levels ✅
- `_documentation-flow-diagram.md` — Three-level hierarchy diagram ✅
- `_orchestration.md` — Workflow (needs update for hierarchy)
- `_unified-coverage-matrix.md` — Updated for hierarchy ✅
- `_62304-coverage-matrix.md` — Updated for hierarchy ✅
- `_14971-coverage-matrix.md` — Updated for hierarchy ✅
- `_82304-coverage-matrix.md` — Updated for hierarchy ✅

**Item-level prompts (complete):**
| Type | Prompt | Status |
|------|--------|--------|
| Extraction | `item-extraction-62304-requirements.md` | ✅ Created |
| Extraction | `item-extraction-62304-risk-contribution.md` | ✅ Created |
| Extraction | `item-extraction-62304-architecture.md` | ✅ Created |
| Extraction | `item-extraction-62304-soup.md` | ✅ Created |
| Extraction | `item-extraction-62304-verification.md` | ✅ Created |
| Generation | `item-generation-62304-risk-contribution.md` | ✅ Created |
| Generation | `item-generation-62304-requirements.md` | ✅ Created |
| Generation | `item-generation-62304-architecture.md` | ✅ Created |
| Generation | `item-generation-62304-soup-list.md` | ✅ Created |
| Generation | `item-generation-62304-verification.md` | ✅ Created |

**Module-level prompts (complete):**
| Type | Prompt | Status |
|------|--------|--------|
| Aggregation | `module-aggregation-architecture.md` | ✅ Created |
| Aggregation | `module-aggregation-soup.md` | ✅ Created |
| Aggregation | `module-aggregation-risks.md` | ✅ Created |
| Aggregation | `module-aggregation-verification.md` | ✅ Created |

**System-level prompts (mostly complete):**
| Type | Prompt | Status |
|------|--------|--------|
| Aggregation | `system-aggregation-risk-file.md` | ✅ Created |
| Aggregation | `system-aggregation-soup.md` | ✅ Created |
| Aggregation | `system-aggregation-verification.md` | ✅ Created |
| Aggregation | `system-aggregation-architecture.md` | ❌ Missing |
| Generation | All 14971/82304 generation prompts | ✅ Exist (flat) |

**Legacy (flat) prompts:**
All original extraction/generation prompts exist but operate on single-repo assumption.

### Gap Analysis

```
ITEM LEVEL
├── Extraction: 5/5 complete (100%) ✅
├── Generation: 5/5 complete (100%) ✅
└── Total: 10/10 (100%)

MODULE LEVEL
├── Aggregation: 4/4 complete (100%) ✅
└── Total: 4/4 (100%)

SYSTEM LEVEL
├── Aggregation: 3/4 complete (75%) - missing architecture
├── Generation: 12/12 exist as flat (100% - but need review)
└── Total: 15/16 (94%)

OVERALL: ~98% complete for hierarchical framework
```

---

## Implementation Plan

### Phase 1: Complete Item-Level Prompts

**Priority: HIGH** — These are the foundation for all aggregation.

#### 1.1 Item Extraction Prompts

| Task | Based On | Effort |
|------|----------|--------|
| Create `item-extraction-62304-architecture.md` | `extraction-62304-architecture.md` | Medium |
| Create `item-extraction-62304-soup.md` | `extraction-62304-soup.md` | Small |
| Create `item-extraction-62304-verification.md` | `extraction-62304-verification.md` | Medium |

**Changes from flat versions:**
- Add `hierarchy` metadata to output schema
- Change IDs from `ARCH-*` to `ITEM-<item>-ARCH-*`
- Add `traces_to_parent` fields for upward traceability
- Remove system-level conclusions (move to aggregation)

#### 1.2 Item Generation Prompts

| Task | Based On | Effort |
|------|----------|--------|
| Create `item-generation-62304-requirements.md` | `generation-62304-srs.md` | Small |
| Create `item-generation-62304-architecture.md` | `generation-62304-sad.md` | Small |
| Create `item-generation-62304-soup-list.md` | `generation-62304-soup-list.md` | Small |
| Create `item-generation-62304-verification.md` | `generation-62304-verification-report.md` | Small |

**Output format:**
- `Item-Requirements.md` (not full SRS)
- `Item-Architecture.md` (not full SAD)
- `Item-SOUP-List.md`
- `Item-Verification.md`

### Phase 2: Complete System-Level Aggregation

**Priority: HIGH** — Required for complete product documentation.

| Task | Aggregates | Effort |
|------|------------|--------|
| Create `system-aggregation-soup.md` | Module SOUP → System SOUP | Medium |
| Create `system-aggregation-verification.md` | Module verification → Validation evidence | Medium |
| Create `system-aggregation-architecture.md` | Module architecture → System architecture | Medium |

**Key features:**
- Deduplicate SOUP across modules/items
- Aggregate test coverage and results
- Create cross-item/module interface map
- Identify system-level gaps

### Phase 3: Update Legacy Generation Prompts

**Priority: MEDIUM** — Existing prompts work but should accept aggregated input.

| Task | Change |
|------|--------|
| Update `generation-62304-srs.md` | Accept `system-requirements.json` from aggregation |
| Update `generation-62304-sad.md` | Accept `system-architecture.json` from aggregation |
| Update `generation-14971-rmr.md` | Accept `product-risk-file.json` from aggregation |
| Update `generation-82304-validation-report.md` | Accept `system-verification.json` from aggregation |

### Phase 4: Update Orchestration

**Priority: MEDIUM** — Required for end-to-end workflow.

| Task | Effort |
|------|--------|
| Update `_orchestration.md` for three-level hierarchy | Medium |
| Add `system-manifest.json` template | Small |
| Add example execution scripts | Small |
| Document incremental update workflow | Medium |

### Phase 5: Testing & Validation

**Priority: HIGH** — Validate against real codebase.

| Task | Target |
|------|--------|
| Run item extraction on `health-manager` | `item-requirements.json` ✅ Done |
| Run item extraction on `health-manager` | `item-architecture.json` |
| Run item extraction on `health-manager` | `item-risk-contribution.json` |
| Run module aggregation | `module-*.json` |
| Run system aggregation | `product-risk-file.json` |
| Generate full document set | All markdown outputs |
| Review against IEC 62304 checklist | Compliance verification |

---

## Revised Workflow: Bidirectional Iteration

See `_workflow-iterations.md` for detailed workflow.

**Key insight:** The workflow is NOT purely bottom-up extraction. It's:

```
PHASE 0: Define system requirements (top-down)
    ↓
PHASE 1: Extract from code (bottom-up)
    ↓
PHASE 2: Gap analysis (compare)
    ↓
PHASE 3: Iterate (close gaps) ←──┐
    ↓                            │
    └── Return to Phase 1 if gaps remain
    ↓
PHASE 4: Aggregate (after aligned)
    ↓
PHASE 5: Generate documents
```

## Revised Prompt Requirements

| Phase | Prompts Needed | Status |
|-------|----------------|--------|
| **0: Definition** | `system-definition-use-requirements.md` | ❌ To create |
| | `system-definition-system-requirements.md` | ❌ To create |
| | `system-definition-risk-criteria.md` | ❌ To create |
| **1: Extraction** | `item-extraction-62304-requirements.md` | ✅ Created |
| | `item-extraction-62304-architecture.md` | ✅ Created |
| | `item-extraction-62304-soup.md` | ✅ Created |
| | `item-extraction-62304-verification.md` | ✅ Created |
| | `item-extraction-62304-risk-contribution.md` | ✅ Created |
| **2: Gap Analysis** | `gap-analysis-requirements.md` | ❌ To create |
| | `gap-analysis-risks.md` | ❌ To create |
| | `gap-analysis-verification.md` | ❌ To create |
| **3: Iteration** | Manual decisions + code changes | N/A |
| **4: Aggregation** | `module-aggregation-*.md` | ✅ All 4 created |
| | `system-aggregation-risk-file.md` | ✅ Created |
| | `system-aggregation-soup.md` | ✅ Created |
| | `system-aggregation-verification.md` | ✅ Created |
| **5: Generation** | `item-generation-*.md` | ✅ All 5 created |
| | System generation prompts | ✅ Exist (flat) |

## Execution Sequence (Revised)

```
Phase A: System Definition Prompts
├── system-definition-use-requirements.md
├── system-definition-system-requirements.md
└── system-definition-risk-criteria.md

Phase B: Complete Item Extraction
├── item-extraction-62304-architecture.md
├── item-extraction-62304-soup.md
└── item-extraction-62304-verification.md

Phase C: Gap Analysis Prompts
├── gap-analysis-requirements.md
├── gap-analysis-risks.md
└── gap-analysis-verification.md

Phase D: Complete System Aggregation
├── system-aggregation-soup.md
└── system-aggregation-verification.md

Phase E: Complete Item Generation
├── item-generation-62304-requirements.md
├── item-generation-62304-architecture.md
├── item-generation-62304-soup-list.md
└── item-generation-62304-verification.md

Phase F: Test Full Pipeline on Platform24/health-manager
```

---

## Immediate Next Steps

~~1. **Create `item-extraction-62304-architecture.md`**~~ ✅ Done
~~2. **Create `item-extraction-62304-soup.md`**~~ ✅ Done
~~3. **Create `item-extraction-62304-verification.md`**~~ ✅ Done
~~4. **Create system aggregation prompts**~~ ✅ Done

5. **Test full item extraction on health-manager**
   - Run all 5 item extraction prompts
   - Validate JSON outputs
   - Generate item-level markdown

6. **Create `system-aggregation-architecture.md`** (optional)
   - Complete system aggregation set

7. **Create gap analysis prompts** (optional)
   - `gap-analysis-requirements.md`
   - `gap-analysis-risks.md`
   - `gap-analysis-verification.md`

8. **Create system definition prompts** (optional)
   - `system-definition-use-requirements.md`
   - `system-definition-system-requirements.md`
   - `system-definition-risk-criteria.md`

---

## Success Criteria

| Criterion | Measure |
|-----------|---------|
| Item extraction complete | All 5 JSON files generated for health-manager |
| Module aggregation complete | 4 module-level JSON files generated |
| System aggregation complete | 4 system-level JSON files generated |
| Document generation complete | All regulatory markdown files generated |
| IEC 62304 coverage | All clauses in coverage matrix marked covered |
| ISO 14971 coverage | Risk file aggregates correctly from items |
| IEC 82304-1 coverage | Validation report traces to item verification |
| Tested on real codebase | health-manager full pipeline passes |

---

## File Structure After Completion

```
Prompts/
├── _prompt-schema.md
├── _documentation-flow-diagram.md
├── _orchestration.md                    # Updated
├── _implementation-plan.md              # This file
├── _workflow-iterations.md              # Bidirectional workflow
├── _unified-coverage-matrix.md
├── _62304-coverage-matrix.md
├── _14971-coverage-matrix.md
├── _82304-coverage-matrix.md
│
├── extraction/
│   ├── item/
│   │   ├── item-extraction-62304-requirements.md      ✅
│   │   ├── item-extraction-62304-architecture.md      ✅
│   │   ├── item-extraction-62304-soup.md              ✅
│   │   ├── item-extraction-62304-verification.md      ✅
│   │   └── item-extraction-62304-risk-contribution.md ✅
│   │
│   ├── extraction-82304-use-requirements.md           # System level
│   ├── extraction-82304-system-requirements.md        # System level
│   └── ... (legacy flat prompts, retained for simple products)
│
├── aggregation/
│   ├── module/
│   │   ├── module-aggregation-architecture.md         ✅
│   │   ├── module-aggregation-soup.md                 ✅
│   │   ├── module-aggregation-risks.md                ✅
│   │   └── module-aggregation-verification.md         ✅
│   │
│   └── system/
│       ├── system-aggregation-risk-file.md            ✅
│       ├── system-aggregation-soup.md                 ✅
│       ├── system-aggregation-verification.md         ✅
│       └── system-aggregation-architecture.md         # Optional
│
└── generation/
    ├── item/
    │   ├── item-generation-62304-requirements.md      ✅
    │   ├── item-generation-62304-architecture.md      ✅
    │   ├── item-generation-62304-soup-list.md         ✅
    │   ├── item-generation-62304-verification.md      ✅
    │   └── item-generation-62304-risk-contribution.md ✅
    │
    ├── generation-62304-srs.md                        # System level
    ├── generation-14971-rmr.md                        # System level
    └── ... (existing generation prompts)
```
