# Risk Data Input Templates

These templates help operators provide the clinical and regulatory context that cannot be extracted from code.

## Workflow

```
1. Run automated extractions (item, module level)
   ↓
2. Run gap analysis prompt
   → Produces risk-data-gaps.json showing what's missing
   ↓
3. Copy relevant templates to Output/<system>/system/defined/
   ↓
4. Fill in templates (requires clinical/regulatory expertise)
   ↓
5. Re-run gap analysis to verify completeness
   ↓
6. Run system aggregation
```

## Templates

| Template | Purpose | When to Use | Who Should Fill |
|----------|---------|-------------|-----------------|
| `intended-use-template.json` | Define USE perspective | Always required | Product owner, clinical expert |
| `safety-characteristics-template.json` | ISO 14971 Annex C assessment | Always required | Risk manager, clinical expert |
| `risk-criteria-template.json` | Severity/probability definitions | Once per organization | Risk manager, QA |
| `hazard-assessments-template.json` | Clinical assessment of hazards | Per hazard needing assessment | Clinical expert, risk manager |

## Usage

### 1. Copy template to system output

```bash
cp Templates/risk-data-inputs/intended-use-template.json \
   Output/platform24/system/defined/intended-use.json
```

### 2. Edit the file

- Replace all `<FILL: ...>` placeholders with actual values
- Remove `_template_instructions` section
- Remove `_notes` and `_examples` fields (or keep as comments)

### 3. Validate with gap analysis

```bash
claude "Run Prompts/analysis/gap-analysis-risk-data.md for platform24"
```

## Template Details

### intended-use-template.json

Defines the **USE perspective**:
- Product context (what the device is)
- Intended use and clinical context (what it's for)
- Intended uses (high-level clinical purposes)
- User intents (what users want to accomplish)
- Core functionality mapping (links code to user needs)

**Key relationships:**
```
IntendedUse → UserIntent → CoreFunctionality
```

### safety-characteristics-template.json

Documents **ISO 14971 Annex C** assessment:
- 17 safety characteristics to evaluate
- Applicability decision for each
- Rationale for applicability
- Hazards identified through each characteristic

**Important:** This is a systematic hazard identification method. Every applicable characteristic should lead to identified hazards.

### risk-criteria-template.json

Defines **organizational risk policy**:
- Severity levels (S1-S5) with definitions and examples
- Probability levels (P1-P5) with definitions and examples
- Risk matrix (which combinations are acceptable/ALARP/unacceptable)
- Benefit-risk policy

**Note:** This is typically defined once per organization, not per product.

### hazard-assessments-template.json

Provides **clinical risk assessment** for each hazard:
- Clinical context and scenario
- Harm analysis
- Severity assessment with rationale
- Probability assessment before/after controls
- Residual risk evaluation
- Benefit-risk analysis (if needed)

**Includes:** A complete worked example showing how to fill in the template.

## Output Location

Templates should be filled and placed in:

```
Output/<system-id>/
└── system/
    └── defined/
        ├── intended-use.json
        ├── safety-characteristics.json
        ├── risk-criteria.json
        └── hazard-assessments.json
```

## Tips

1. **Start with intended-use.json** - This defines the foundation for everything else

2. **Use gap analysis iteratively** - Run it after each template to track progress

3. **Risk criteria can be reused** - If your organization has defined criteria, copy from a previous project

4. **Safety characteristics require clinical input** - Don't guess; involve someone who understands the clinical context

5. **Hazard assessments need traceability** - Reference specific hazard IDs from the extractions
