---
id: 725af57
title: "github actions integration"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Specification"
category: "automation"
process: "[Document and Record Control](../../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# GitHub Actions Integration for Code-as-Truth Documentation

## Overview

This document specifies how code-as-truth documentation generation integrates with GitHub Actions for automated regulatory document maintenance.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Customer Repository                          │
├─────────────────────────────────────────────────────────────────┤
│  src/                    │  .github/workflows/                  │
│  ├── components/         │  └── docs-generation.yml             │
│  ├── services/           │                                      │
│  └── ...                 │  docs/regulatory/                    │
│                          │  ├── srs.md          ← Generated     │
│  package.json            │  ├── sad.md          ← Generated     │
│  package-lock.json       │  ├── soup-list.md    ← Generated     │
│                          │  └── extracted/                      │
│                          │      ├── requirements.json           │
│                          │      ├── architecture.json           │
│                          │      └── soup-list.json              │
└─────────────────────────────────────────────────────────────────┘
                                    ↑
                    References prompts from QMS Master
```

## Workflow Triggers

### 1. On Pull Request (Preview)
Generate documentation preview for review before merge.

```yaml
on:
  pull_request:
    branches: [main]
    paths:
      - 'src/**'
      - 'package*.json'
      - 'requirements*.txt'
```

**Outputs:**
- PR comment with documentation diff summary
- Artifacts: generated docs for review
- Checks: validation criteria pass/fail

### 2. On Push to Main (Update)
Regenerate and commit documentation after merge.

```yaml
on:
  push:
    branches: [main]
    paths:
      - 'src/**'
      - 'package*.json'
```

**Outputs:**
- Committed documentation updates
- Release notes including doc changes

### 3. Manual Dispatch (Full Regeneration)
Complete regeneration for initial setup or major changes.

```yaml
on:
  workflow_dispatch:
    inputs:
      force_regenerate:
        description: 'Force full regeneration'
        type: boolean
        default: false
```

## Workflow Implementation

### Option A: Claude API Direct Integration

```yaml
# .github/workflows/docs-generation.yml
name: Generate Regulatory Documentation

on:
  push:
    branches: [main]
    paths:
      - 'src/**'
      - 'package*.json'
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  extract:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Extract Requirements
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          # Use Claude API to run extraction prompt
          # Prompt content from QMS Master repository
          curl -X POST https://api.anthropic.com/v1/messages \
            -H "x-api-key: $ANTHROPIC_API_KEY" \
            -H "anthropic-version: 2024-01-01" \
            -H "content-type: application/json" \
            -d @- << 'EOF'
          {
            "model": "claude-sonnet-4-20250514",
            "max_tokens": 8192,
            "system": "You are a regulatory documentation extraction assistant...",
            "messages": [
              {
                "role": "user",
                "content": "Analyze the codebase and extract requirements per IEC 62304..."
              }
            ]
          }
          EOF

      - name: Upload Extraction Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: extracted-data
          path: docs/regulatory/extracted/

  generate:
    needs: extract
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/download-artifact@v4
        with:
          name: extracted-data
          path: docs/regulatory/extracted/

      - name: Generate SRS
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          # Generate SRS from extracted requirements
          # ...

      - name: Commit Documentation
        if: github.event_name == 'push'
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add docs/regulatory/
          git diff --staged --quiet || git commit -m "docs: Update regulatory documentation"
          git push
```

### Option B: Claude Code CLI Integration

```yaml
# Using Claude Code in CI
jobs:
  generate-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: |
          npm install -g @anthropic-ai/claude-code

      - name: Run Extraction
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --print "Follow the extraction prompt at Prompts/extraction/extraction-62304-requirements.md for this codebase. Output to docs/regulatory/extracted/requirements.json"
```

### Option C: Hybrid with MCP Tools

```yaml
# Using MCP filesystem tools for better code access
jobs:
  generate-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Configure MCP
        run: |
          # Setup MCP with filesystem access to repo

      - name: Run Documentation Pipeline
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          # Execute extraction and generation with MCP tools
```

## Prompt Injection Pattern

The workflow needs to inject prompt content dynamically:

```yaml
- name: Load Prompt
  id: prompt
  run: |
    # Fetch prompt from QMS Master repo or local copy
    PROMPT=$(cat prompts/extraction-62304-requirements.md)
    echo "prompt<<EOF" >> $GITHUB_OUTPUT
    echo "$PROMPT" >> $GITHUB_OUTPUT
    echo "EOF" >> $GITHUB_OUTPUT

- name: Execute Extraction
  env:
    PROMPT: ${{ steps.prompt.outputs.prompt }}
  run: |
    # Use PROMPT variable in API call
```

## Validation Checks

### Pre-Commit Validation
```yaml
validate:
  runs-on: ubuntu-latest
  steps:
    - name: Validate Extraction Output
      run: |
        # Check JSON schema compliance
        npx ajv validate -s schemas/requirements.schema.json -d docs/regulatory/extracted/requirements.json

    - name: Check Traceability Coverage
      run: |
        # Verify all code modules have requirements
        # Verify all requirements have tests
        python scripts/check-traceability.py

    - name: Validate Safety Classifications
      run: |
        # Ensure safety-relevant code has B/C classification
        python scripts/check-safety-class.py
```

### Post-Generation Validation
```yaml
- name: Validate Generated Documents
  run: |
    # Check required sections exist
    # Verify frontmatter is complete
    # Confirm traceability matrix is populated
```

## Configuration

### Repository Setup

Create `.github/qms-config.yml`:

```yaml
# QMS Documentation Generation Configuration

extraction:
  # Source paths to analyze
  source_paths:
    - src/
    - lib/

  # Exclude patterns
  exclude:
    - "**/*.test.ts"
    - "**/__mocks__/**"

  # Package manifests for SOUP
  manifests:
    - package.json
    - package-lock.json

generation:
  # Output directory
  output_dir: docs/regulatory

  # Document formats
  formats:
    - markdown

  # Safety classification
  default_safety_class: B

prompts:
  # QMS Master repository reference
  source: "github:noncomplicity/qms-master"
  ref: main

  # Or local path
  # source: ".qms/prompts"

traceability:
  # Link requirements to tests
  test_patterns:
    - "**/*.test.ts"
    - "**/*.spec.ts"

  # Requirement ID pattern in test names
  req_pattern: "SRS-\\w+-\\d+"
```

## Security Considerations

1. **API Key Management**
   - Store `ANTHROPIC_API_KEY` as repository secret
   - Use GitHub Environments for production vs. preview

2. **Prompt Integrity**
   - Pin QMS Master prompts to specific commit/tag
   - Validate prompt checksums before use

3. **Output Sanitization**
   - Review generated content before auto-commit
   - Require approval for safety-critical doc changes

## Implementation Phases

### Phase 1: Manual Invocation
- Workflow dispatch only
- Human review of all outputs
- Validation without auto-commit

### Phase 2: PR Preview
- Generate on PR
- Comment with diff summary
- No auto-merge of docs

### Phase 3: Automated Updates
- Auto-commit on main push
- Full validation pipeline
- Change notifications

## Cost Considerations

| Workflow | API Calls | Estimated Tokens | Cost (Claude Sonnet) |
|----------|-----------|------------------|----------------------|
| Full extraction | 3 | ~50,000 | ~$0.50 |
| Full generation | 3 | ~100,000 | ~$1.00 |
| Incremental update | 1-2 | ~20,000 | ~$0.20 |

**Optimization:**
- Cache extraction results
- Incremental updates on file changes
- Use smaller models for validation
