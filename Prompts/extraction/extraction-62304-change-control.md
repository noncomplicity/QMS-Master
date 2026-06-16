---
id: 725af57
title: "extraction 62304 change control"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "8.2, 8.3, 9"
inputs: ["git history", "pull requests", "issues", "CI/CD logs"]
outputs: ["change-control.json", "problem-reports.json"]
software_class: "all"
process: "[Document and Record Control](../../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Change Control and Problem Resolution Evidence

## Context

IEC 62304 clauses 8 and 9 require:
- **8.2** Change Control — approve, implement, and verify changes
- **8.3** Configuration Status Accounting — maintain history of controlled items
- **9** Software Problem Resolution — prepare, investigate, and resolve problem reports

In a code-as-truth model, Git history, PRs, and issues provide the evidence for these processes.

## Inputs

### Git History
- Commit messages and metadata
- Branch structure
- Tags and releases
- File change history

### Pull Requests / Merge Requests
- PR descriptions and titles
- Review comments and approvals
- CI/CD check results
- Linked issues

### Issues / Problem Reports
- Issue descriptions
- Labels and priorities
- Resolution status
- Linked PRs

### CI/CD Pipelines
- Build and test results
- Deployment logs
- Verification outputs

## Instructions

### 1. Extract Change Requests (8.2.1)

For each merged PR, document:

a) **Change Request Identification**
   - PR number and title
   - Author and reviewers
   - Creation and merge dates
   - Target branch

b) **Change Request Approval**
   - Required approvals received
   - Review comments and resolutions
   - CI checks passed

c) **Change Description**
   - What was changed (files, components)
   - Why it was changed (linked issues, rationale)
   - Type of change (feature, fix, refactor, etc.)

### 2. Extract Change Implementation (8.2.2)

For each change:

a) **Files Modified**
   - List of changed files
   - Configuration items affected
   - Software items impacted

b) **Activities Repeated**
   - Tests re-run
   - Verification activities
   - Documentation updates

c) **Safety Classification Impact**
   - Did safety classification change?
   - Were safety-critical items modified?

### 3. Extract Change Verification (8.2.3)

a) **Verification Activities**
   - CI/CD pipeline results
   - Test execution reports
   - Code review outcomes

b) **Verification Status**
   - All checks passed
   - Identified issues and resolutions

### 4. Extract Change Traceability (8.2.4)

Link together:
- Change Request (PR)
- Related Problem Reports (Issues)
- Approval records (Reviews)
- Verification records (CI results)

### 5. Extract Configuration Status (8.3)

a) **Version History**
   - All versions of configuration items
   - System configurations over time
   - Release history

b) **Current Configuration**
   - Active configuration items
   - Current versions
   - Deployment status

### 6. Extract Problem Reports (9.1-9.8)

For each issue/bug:

a) **Problem Report Content** (9.1)
   - Problem description
   - Criticality assessment
   - Affected versions
   - Reproduction steps

b) **Investigation** (9.2)
   - Root cause analysis
   - Safety relevance evaluation
   - Change requests created

c) **Resolution** (9.4, 9.5)
   - Resolution approach
   - Verification of resolution
   - Status (open, resolved, deferred)

d) **Trend Analysis** (9.6)
   - Problem categories
   - Frequency trends
   - Systemic issues

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "extracted_at": "<ISO-8601-timestamp>",
    "commit_range": {
      "from": "<commit-hash>",
      "to": "<commit-hash>"
    },
    "date_range": {
      "from": "<ISO-date>",
      "to": "<ISO-date>"
    }
  },
  "change_requests": [
    {
      "cr_id": "CR-<pr-number>",
      "pr_number": 0,
      "pr_url": "<url>",
      "title": "<PR title>",
      "description": "<PR description>",
      "author": "<username>",
      "created_at": "<ISO-timestamp>",
      "merged_at": "<ISO-timestamp>",
      "target_branch": "<branch>",
      "change_type": "feature | bugfix | refactor | docs | security | dependency",
      "approval": {
        "required_reviewers": 0,
        "approvals_received": 0,
        "reviewers": [
          {
            "username": "<username>",
            "status": "approved | changes_requested | commented",
            "reviewed_at": "<ISO-timestamp>"
          }
        ],
        "approval_status": "approved | pending | rejected"
      },
      "verification": {
        "ci_checks": [
          {
            "name": "<check-name>",
            "status": "passed | failed | skipped",
            "url": "<check-url>"
          }
        ],
        "all_checks_passed": true,
        "tests_executed": 0,
        "tests_passed": 0
      },
      "changes": {
        "files_changed": 0,
        "insertions": 0,
        "deletions": 0,
        "files": [
          {
            "path": "<file-path>",
            "change_type": "added | modified | deleted | renamed",
            "software_item": "<SI-id>"
          }
        ]
      },
      "configuration_items_affected": ["<CI-id>"],
      "software_items_affected": ["<SI-id>"],
      "safety_impact": {
        "affects_safety_items": false,
        "safety_items": ["<SI-id>"],
        "safety_class_changed": false,
        "risk_analysis_required": false
      },
      "linked_issues": ["<issue-number>"],
      "activities_repeated": ["<activity-name>"]
    }
  ],
  "problem_reports": [
    {
      "pr_id": "PR-<issue-number>",
      "issue_number": 0,
      "issue_url": "<url>",
      "title": "<issue title>",
      "description": "<issue description>",
      "reported_by": "<username>",
      "reported_at": "<ISO-timestamp>",
      "criticality": {
        "severity": "critical | high | medium | low",
        "safety_relevant": false,
        "security_relevant": false,
        "affects_performance": false
      },
      "affected_versions": ["<version>"],
      "affected_software_items": ["<SI-id>"],
      "labels": ["<label>"],
      "investigation": {
        "root_cause": "<description>",
        "cause_category": "specification | design | implementation | configuration | external",
        "safety_evaluation": "<assessment>",
        "investigated_by": "<username>",
        "investigated_at": "<ISO-timestamp>"
      },
      "resolution": {
        "status": "open | in_progress | resolved | deferred | wont_fix",
        "resolution_type": "fix | workaround | documentation | deferred",
        "resolution_description": "<how resolved>",
        "change_requests": ["CR-<id>"],
        "resolved_by": "<username>",
        "resolved_at": "<ISO-timestamp>",
        "verified": false,
        "verification_evidence": "<test or analysis reference>"
      },
      "rationale_for_no_action": "<if no action taken, why>"
    }
  ],
  "configuration_status": {
    "releases": [
      {
        "version": "<semver>",
        "tag": "<git-tag>",
        "commit": "<commit-hash>",
        "released_at": "<ISO-timestamp>",
        "release_notes": "<notes>",
        "configuration_items": [
          {
            "item_id": "<CI-id>",
            "type": "<file | package | config>",
            "version": "<version or hash>"
          }
        ]
      }
    ],
    "current_configuration": {
      "branch": "<main-branch>",
      "commit": "<head-commit>",
      "items": [
        {
          "path": "<file-path>",
          "version": "<commit-hash>",
          "last_modified": "<ISO-timestamp>",
          "last_modified_by": "<username>"
        }
      ]
    }
  },
  "traceability": {
    "problem_to_change": [
      {
        "problem_id": "PR-<id>",
        "change_ids": ["CR-<id>"]
      }
    ],
    "change_to_verification": [
      {
        "change_id": "CR-<id>",
        "verification_records": ["<ci-run-id>"]
      }
    ]
  },
  "trend_analysis": {
    "period": {
      "from": "<ISO-date>",
      "to": "<ISO-date>"
    },
    "total_problems": 0,
    "problems_by_severity": {
      "critical": 0,
      "high": 0,
      "medium": 0,
      "low": 0
    },
    "problems_by_category": {
      "specification": 0,
      "design": 0,
      "implementation": 0,
      "configuration": 0,
      "external": 0
    },
    "resolution_metrics": {
      "average_time_to_resolve_days": 0,
      "open_problems": 0,
      "resolved_problems": 0,
      "deferred_problems": 0
    },
    "trends": [
      {
        "trend_type": "increasing | decreasing | stable",
        "category": "<problem category>",
        "description": "<trend observation>",
        "recommendation": "<action to address>"
      }
    ]
  },
  "gaps": [
    {
      "gap_type": "unapproved_change | unverified_change | unresolved_problem | missing_investigation",
      "description": "<what is missing>",
      "affected_items": ["<id>"],
      "recommendation": "<action to close gap>"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `change_requests[].approval` | 8.2.1 | Approve change requests |
| `change_requests[].changes` | 8.2.2 | Implement changes |
| `change_requests[].verification` | 8.2.3 | Verify changes |
| `traceability` | 8.2.4 | Traceability of change |
| `configuration_status` | 8.3 | Configuration status accounting |
| `problem_reports` | 9.1 | Prepare problem reports |
| `problem_reports[].investigation` | 9.2 | Investigate the problem |
| `problem_reports[].criticality.safety_relevant` | 9.2 b) | Evaluate safety relevance |
| `problem_reports[].resolution.change_requests` | 9.2 d) | Create change requests |
| `traceability.problem_to_change` | 9.4 | Use change control process |
| `problem_reports[].resolution` | 9.5 | Maintain records |
| `trend_analysis` | 9.6 | Analyse problems for trends |
| `problem_reports[].resolution.verified` | 9.7 | Verify problem resolution |

## Examples

### Input: GitHub PR

```markdown
## PR #142: Fix critical heart rate threshold validation

**Description:**
Fixes issue #89 where heart rate values of exactly 40 bpm were not triggering alerts.

**Changes:**
- Modified `heartRateAnalyzer.ts` to use `<=` instead of `<` for lower bound check
- Added unit tests for boundary conditions

**Testing:**
- All existing tests pass
- Added new boundary tests

**Labels:** bugfix, safety-critical
```

### Output: Extracted Change Record

```json
{
  "cr_id": "CR-142",
  "pr_number": 142,
  "title": "Fix critical heart rate threshold validation",
  "change_type": "bugfix",
  "approval": {
    "required_reviewers": 2,
    "approvals_received": 2,
    "approval_status": "approved"
  },
  "safety_impact": {
    "affects_safety_items": true,
    "safety_items": ["SI-VITALS-ANALYZER"],
    "safety_class_changed": false,
    "risk_analysis_required": true
  },
  "linked_issues": [89],
  "activities_repeated": ["unit_tests", "integration_tests", "risk_assessment"]
}
```

## Validation Criteria

- [ ] All merged PRs have corresponding change request records 🆔 I5GvXr
- [ ] Change requests show required approvals obtained 🆔 7dpsdw
- [ ] CI/CD verification results are captured for each change 🆔 CFm2UR
- [ ] Changes affecting safety items are flagged 🆔 PJOzoE
- [ ] Problem reports include criticality assessment 🆔 FTIwz9
- [ ] Problems are linked to resolution change requests 🆔 9GkHjO
- [ ] Safety-relevant problems have safety evaluation documented 🆔 bkaPGs
- [ ] Configuration status shows current and historical versions 🆔 g5PsPC
- [ ] Trend analysis identifies patterns in problems 🆔 IRvnpi
- [ ] Unresolved critical problems are highlighted 🆔 G08KXg
