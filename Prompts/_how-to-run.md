---
id:
title: "How to Run Code-as-Truth Documentation"
version:
author:
effective_date:
type: "WorkInstruction"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# How to Run Code-as-Truth Documentation

This guide explains how to run the code-as-truth documentation framework to extract regulatory documentation from source code.

## Prerequisites

1. **Claude Code CLI** installed and configured
2. **QMS Master** repository cloned (contains prompts)
3. **Target repository** accessible (the codebase to document)
4. **System manifest** created (defines hierarchy)

## Quick Start

```bash
# From QMS Master directory
claude "Run the documentation workflow for platform24 following Prompts/_master-orchestration.md"
```

## Directory Structure

Before running, ensure this structure exists:

```
Output/<system-id>/
├── system-manifest.json          # REQUIRED: Define this first
├── items/
│   └── <item-id>/
│       ├── extracted/            # JSON extractions go here
│       └── generated/            # Markdown documents go here
├── modules/
│   └── <module-id>/
│       └── aggregated/           # Module aggregations go here
└── system/
    ├── defined/                  # Top-down requirements (manual)
    ├── aggregated/               # System aggregations go here
    └── generated/                # System documents go here
```

## Step 1: Create System Manifest

Create `Output/<system-id>/system-manifest.json`:

```json
{
  "system": {
    "id": "platform24",
    "name": "Platform24 Health Software",
    "version": "1.0.0",
    "safety_class": "B",
    "description": "Digital healthcare platform"
  },
  "modules": [
    {
      "id": "smart-care-plans",
      "name": "Smart Care Plans",
      "description": "Appointment and clinical workflow management",
      "items": ["health-manager"],
      "safety_class": "B"
    }
  ],
  "items": [
    {
      "id": "health-manager",
      "name": "Health Manager Service",
      "repository": "/path/to/health-manager",
      "module": "smart-care-plans",
      "safety_class": "B"
    }
  ],
  "extraction_config": {
    "output_base": "Output/platform24",
    "items_path": "items/{item_id}/extracted",
    "modules_path": "modules/{module_id}/aggregated",
    "system_path": "system/aggregated"
  }
}
```

## Step 2: Run Item Extraction

For each software item, run all 5 extraction prompts:

```bash
# Navigate to QMS Master
cd /path/to/QMS-Master

# Set variables
ITEM_REPO="/path/to/health-manager"
OUTPUT_DIR="Output/platform24/items/health-manager/extracted"

# Create output directory
mkdir -p "$OUTPUT_DIR"

# Run extractions (can be done in parallel)
claude "
Read the repository at $ITEM_REPO.
Follow Prompts/extraction/item/item-extraction-62304-requirements.md.
Output to $OUTPUT_DIR/item-requirements.json.
Use item_id='health-manager', parent_module='smart-care-plans', parent_system='platform24'.
"

claude "
Read the repository at $ITEM_REPO.
Follow Prompts/extraction/item/item-extraction-62304-architecture.md.
Output to $OUTPUT_DIR/item-architecture.json.
"

claude "
Read the repository at $ITEM_REPO.
Follow Prompts/extraction/item/item-extraction-62304-soup.md.
Output to $OUTPUT_DIR/item-soup.json.
"

claude "
Read the repository at $ITEM_REPO.
Follow Prompts/extraction/item/item-extraction-62304-verification.md.
Output to $OUTPUT_DIR/item-verification.json.
"

claude "
Read the repository at $ITEM_REPO.
Follow Prompts/extraction/item/item-extraction-62304-risk-contribution.md.
Output to $OUTPUT_DIR/item-risk-contribution.json.
"
```

### Expected Outputs

After item extraction:
```
Output/platform24/items/health-manager/extracted/
├── item-requirements.json      # Functional, interface, security requirements
├── item-architecture.json      # Components, interfaces, data flows
├── item-soup.json              # Third-party dependencies
├── item-verification.json      # Test cases and traceability
└── item-risk-contribution.json # Hazards, controls, failure modes
```

## Step 3: Generate Item Documents

```bash
OUTPUT_DIR="Output/platform24/items/health-manager"

claude "
Read $OUTPUT_DIR/extracted/item-requirements.json.
Follow Prompts/generation/item/item-generation-62304-requirements.md.
Output to $OUTPUT_DIR/generated/Item-Requirements.md.
"

# Repeat for other generation prompts...
```

### Expected Outputs

```
Output/platform24/items/health-manager/generated/
├── Item-Requirements.md
├── Item-Architecture.md
├── Item-SOUP-List.md
├── Item-Verification.md
└── Item-Risk-Contribution.md
```

## Step 4: Run Module Aggregation

After all items in a module are extracted:

```bash
MODULE_DIR="Output/platform24/modules/smart-care-plans/aggregated"
mkdir -p "$MODULE_DIR"

claude "
Read all item extractions from Output/platform24/items/health-manager/extracted/.
Read Output/platform24/system-manifest.json.
Follow Prompts/aggregation/module/module-aggregation-architecture.md.
Output to $MODULE_DIR/module-architecture.json.
"

# Repeat for soup, risk, verification...
```

### Expected Outputs

```
Output/platform24/modules/smart-care-plans/aggregated/
├── module-architecture.json
├── module-soup.json
├── module-risk.json
└── module-verification.json
```

## Step 5: Run System Aggregation

After all modules are aggregated:

```bash
SYSTEM_DIR="Output/platform24/system/aggregated"
mkdir -p "$SYSTEM_DIR"

claude "
Read all module aggregations from Output/platform24/modules/*/aggregated/.
Read Output/platform24/system-manifest.json.
Follow Prompts/aggregation/system/system-aggregation-soup.md.
Output to $SYSTEM_DIR/system-soup.json.
"

# Repeat for risk-file, verification...
```

### Expected Outputs

```
Output/platform24/system/aggregated/
├── system-architecture.json
├── system-soup.json
├── system-risk-file.json
└── system-verification.json
```

## Step 6: Generate System Documents

```bash
claude "
Read Output/platform24/system/aggregated/system-risk-file.json.
Follow Prompts/generation/system-generation-14971-rmr.md.
Output to Output/platform24/system/generated/RMR.md.
"
```

## Single Command Execution

For convenience, use the master orchestration prompt:

```bash
claude "
Follow Prompts/_master-orchestration.md to run the complete documentation workflow.

System: platform24
Manifest: Output/platform24/system-manifest.json

Repositories:
- health-manager: /home/jakob/Noncomplicity/Repos/health-manager

Run all phases:
1. Item extraction for all items
2. Item document generation
3. Module aggregation
4. System aggregation

Output structure: Output/platform24/
"
```

## Incremental Updates

When code changes, re-run only affected extractions:

```bash
# If only tests changed
claude "
Re-extract item-verification.json for health-manager.
Repository: /path/to/health-manager
Output: Output/platform24/items/health-manager/extracted/item-verification.json
Then regenerate Item-Verification.md.
Then re-run module and system verification aggregation.
"
```

## Validation

After running, verify outputs:

```bash
# Check all expected files exist
ls -la Output/platform24/items/*/extracted/
ls -la Output/platform24/modules/*/aggregated/
ls -la Output/platform24/system/aggregated/

# Validate JSON syntax
for f in Output/platform24/**/*.json; do
  python -m json.tool "$f" > /dev/null && echo "✓ $f" || echo "✗ $f"
done
```

## Common Issues

### "Repository not found"
- Ensure the repository path in system-manifest.json is absolute
- Ensure Claude Code has access to the directory

### "Missing input file"
- Run phases in order: extraction → generation → aggregation
- Check that previous phase completed successfully

### "Hallucinated data"
- The AI may synthesize data if it can't find real information
- Verify extracted data against actual source code
- Re-run with more specific instructions if needed

### "Wrong hierarchy level"
- Check that item_id, module_id, system_id are consistent
- Verify system-manifest.json hierarchy is correct

## Automation (Future)

For CI/CD integration, see:
- `Prompts/automation/github-actions-integration.md`

Planned automation:
- GitHub Action to run on push
- Scheduled SOUP vulnerability checks
- PR preview generation
