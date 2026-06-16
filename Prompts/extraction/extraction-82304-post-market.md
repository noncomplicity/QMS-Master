---
id: 725af57
title: "extraction 82304 post market"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "IEC 82304-1"
clause: "8"
inputs: ["CHANGELOG", "release notes", "security advisories", "issue tracker", "communication records"]
outputs: ["post-market.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Post-Market Activity Evidence

## Context

IEC 82304-1 clause 8 requires manufacturers to conduct post-market activities throughout the health software product's life cycle. This includes software maintenance (linking to IEC 62304), re-validation after changes, and communication with users and responsible organizations.

In code-as-truth, post-market activities are evidenced through:
- Release history and changelogs
- Security advisories and patches
- Issue tracking and resolution
- User communication records
- Version compatibility documentation

## Inputs

### Release History
- **CHANGELOG.md**: Version history, changes per release
- **Release notes**: Detailed release documentation
- **Git tags**: Version tagging
- **Package registry history**: npm, PyPI, etc. release records

### Security Information
- **SECURITY.md**: Security policy, vulnerability reporting
- **Security advisories**: CVE disclosures, security bulletins
- **Dependency updates**: Security-related dependency changes
- **Audit logs**: Security scanning results

### Issue Tracking
- **GitHub Issues**: Bug reports, feature requests
- **Problem reports**: User-reported issues
- **Support tickets**: Customer support records

### Communication Records
- **Email notifications**: Release announcements
- **Blog posts**: Product updates
- **Documentation updates**: User guide changes
- **In-app notifications**: User alerts

### Maintenance Evidence
- **Pull requests**: Changes made
- **CI/CD logs**: Build and deployment records
- **Test results**: Regression testing

## Instructions

### 1. Extract Software Maintenance History (8.2)

For each release, document:
- Version identifier
- Release date
- Change type (corrective, adaptive, perfective, preventive)
- Changes made
- Compliance evidence (did change follow 82304-1?)

### 2. Extract Re-Validation Evidence (8.3)

For changes requiring re-validation:
- Affected use requirements
- Re-validation scope
- Re-validation results
- Platform compatibility confirmation

### 3. Extract Post-Market Communication (8.4)

Document communications about:
- Security vulnerabilities (8.4)
- Regulatory changes affecting use
- New features (8.4 a)
- Corrected errors (8.4 b)
- Safety/security impact (8.4 c)
- Identification updates (8.4 d)
- Documentation updates (8.4 e)

### 4. Extract Decommissioning Information (8.5)

Document:
- End-of-life announcements
- Migration paths
- Data export procedures
- Support termination timeline

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0",
    "standard": "IEC 82304-1:2016"
  },

  "product_identification": {
    "name": "<product name>",
    "current_version": "<current version>",
    "initial_release": "<initial release date>",
    "latest_release": "<latest release date>"
  },

  "release_history": [
    {
      "version": "<version>",
      "release_date": "<date>",
      "release_type": "major | minor | patch | security",
      "maintenance_type": "corrective | adaptive | perfective | preventive",
      "summary": "<release summary>",
      "changes": {
        "features": [
          {
            "description": "<new feature>",
            "use_requirements_affected": ["<USE-* ids>"]
          }
        ],
        "fixes": [
          {
            "description": "<bug fix>",
            "issue_reference": "<issue ID>",
            "safety_impact": "none | low | medium | high"
          }
        ],
        "security": [
          {
            "description": "<security fix>",
            "cve": "<CVE if applicable>",
            "severity": "low | medium | high | critical"
          }
        ],
        "breaking_changes": [
          {
            "description": "<breaking change>",
            "migration_path": "<how to migrate>"
          }
        ]
      },
      "compliance": {
        "followed_82304": true,
        "revalidation_performed": true,
        "revalidation_reference": "<validation reference>"
      },
      "source": {
        "changelog_entry": "<changelog reference>",
        "commit_range": "<commit range>",
        "release_notes": "<release notes URL>"
      }
    }
  ],

  "software_maintenance": {
    "maintenance_process": {
      "documented": true,
      "follows_62304_clause_6": true,
      "process_reference": "<maintenance process document>"
    },
    "maintenance_activities": [
      {
        "activity_id": "MAINT-<seq>",
        "type": "corrective | adaptive | perfective | preventive",
        "description": "<maintenance activity>",
        "trigger": "<what triggered this>",
        "version_introduced": "<version>",
        "clause_5_activities_repeated": ["<62304 clause 5 activities>"],
        "source": {
          "pr": "<PR reference>",
          "issue": "<issue reference>"
        }
      }
    ],
    "soup_maintenance": {
      "upgrade_policy": "<policy for SOUP upgrades>",
      "security_patch_policy": "<policy for security patches>",
      "obsolescence_handling": "<how obsolete SOUP is handled>",
      "recent_updates": [
        {
          "soup_component": "<component name>",
          "from_version": "<old version>",
          "to_version": "<new version>",
          "reason": "<reason for update>",
          "date": "<date>"
        }
      ]
    }
  },

  "revalidation_records": [
    {
      "revalidation_id": "REVAL-<seq>",
      "version": "<version>",
      "trigger": {
        "type": "maintenance | regulatory_change | platform_change | user_feedback",
        "description": "<what triggered revalidation>"
      },
      "scope": {
        "affected_requirements": ["<USE-* ids>"],
        "validation_activities": ["<validation activities performed>"]
      },
      "results": {
        "status": "pass | fail | partial",
        "summary": "<results summary>",
        "issues_found": [
          {
            "issue": "<issue description>",
            "resolution": "<how resolved>"
          }
        ]
      },
      "platform_compatibility": {
        "platforms_tested": ["<platforms>"],
        "all_claimed_platforms_verified": true
      },
      "date": "<revalidation date>",
      "validation_plan_updated": true,
      "source": {
        "files": ["<evidence files>"]
      }
    }
  ],

  "security_communications": [
    {
      "communication_id": "SEC-COMM-<seq>",
      "date": "<date>",
      "type": "vulnerability_disclosure | security_advisory | patch_notification",
      "vulnerability": {
        "cve": "<CVE if applicable>",
        "description": "<vulnerability description>",
        "severity": "low | medium | high | critical",
        "cvss_score": "<score if available>",
        "affected_versions": ["<versions>"],
        "fixed_version": "<version with fix>"
      },
      "communication": {
        "channels": ["email | website | in_app | registry"],
        "recipients": "users | responsible_organizations | both",
        "content_summary": "<summary of communication>",
        "recommended_action": "<action recommended>"
      },
      "source": {
        "files": ["<advisory files>"],
        "urls": ["<advisory URLs>"]
      }
    }
  ],

  "regulatory_communications": [
    {
      "communication_id": "REG-COMM-<seq>",
      "date": "<date>",
      "regulatory_change": "<what changed>",
      "impact_on_use": "<how it affects product use>",
      "communication": {
        "channels": ["<channels used>"],
        "recipients": "<who was notified>",
        "content_summary": "<summary>"
      },
      "source": {
        "files": ["<evidence files>"]
      }
    }
  ],

  "release_communications": [
    {
      "communication_id": "REL-COMM-<seq>",
      "version": "<version>",
      "date": "<date>",
      "content": {
        "new_features": ["<features announced>"],
        "corrected_errors": ["<fixes announced>"],
        "safety_security_impact": {
          "has_impact": true,
          "description": "<impact description>",
          "user_action_required": "<action if any>"
        },
        "identification_updates": {
          "version_changed": true,
          "name_changed": false
        },
        "documentation_updates": ["<doc updates>"]
      },
      "communication": {
        "channels": ["<channels used>"],
        "recipients": "<users | responsible_organizations | both>"
      },
      "recommendation": {
        "urgency": "immediate | soon | convenience",
        "rationale": "<why this urgency>"
      },
      "source": {
        "files": ["<evidence files>"],
        "urls": ["<release announcement URLs>"]
      }
    }
  ],

  "decommissioning_information": {
    "end_of_life_announced": false,
    "announcement_date": "<date if announced>",
    "end_of_support_date": "<date>",
    "end_of_life_date": "<date>",
    "migration_path": {
      "successor_product": "<successor if any>",
      "migration_instructions": "<how to migrate>",
      "data_migration": "<data migration approach>"
    },
    "data_handling": {
      "export_available_until": "<date>",
      "export_formats": ["<formats>"],
      "retention_policy": "<how long data is kept>",
      "deletion_process": "<deletion procedure>"
    },
    "communication_plan": {
      "notification_timeline": [
        {
          "milestone": "<milestone>",
          "date": "<date>",
          "communication": "<what will be communicated>"
        }
      ]
    },
    "source": {
      "files": ["<evidence files>"]
    }
  },

  "feedback_collection": {
    "channels": [
      {
        "channel": "<channel name>",
        "type": "issue_tracker | email | support_portal | in_app",
        "url": "<URL if applicable>"
      }
    ],
    "feedback_summary": {
      "period": "<reporting period>",
      "total_reports": 0,
      "by_category": {
        "bug_reports": 0,
        "feature_requests": 0,
        "security_reports": 0,
        "usability_issues": 0
      },
      "safety_relevant_reports": 0
    },
    "safety_relevant_feedback": [
      {
        "report_id": "<report ID>",
        "date": "<date>",
        "description": "<feedback description>",
        "safety_relevance": "<why safety relevant>",
        "action_taken": "<action taken>",
        "status": "open | investigating | resolved | closed"
      }
    ]
  },

  "summary": {
    "total_releases": 0,
    "releases_this_year": 0,
    "security_releases": 0,
    "revalidations_performed": 0,
    "security_communications_sent": 0,
    "open_safety_issues": 0,
    "compliance_status": {
      "maintenance_process_compliant": true,
      "communication_process_compliant": true,
      "revalidation_process_compliant": true
    }
  },

  "gaps": [
    {
      "clause": "<82304-1 clause>",
      "gap_description": "<what is missing>",
      "recommendation": "<how to address>",
      "priority": "high | medium | low"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 82304-1 Clause | Evidence Provided |
|----------------|-------------------|-------------------|
| `software_maintenance` | 8.2 | Software maintenance per 82304-1 |
| `software_maintenance.clause_5_activities_repeated` | 8.2 | Clause 5 activities repeated |
| `software_maintenance.soup_maintenance` | 8.2 | SOUP upgrade/patch handling |
| `revalidation_records` | 8.3 | Re-validation evidence |
| `revalidation_records.scope` | 8.3 | Affected parts re-validated |
| `revalidation_records.platform_compatibility` | 8.3 | Platform compatibility confirmed |
| `security_communications` | 8.4 | Security vulnerability communication |
| `regulatory_communications` | 8.4 | Regulatory change communication |
| `release_communications.content.new_features` | 8.4 a) | New features |
| `release_communications.content.corrected_errors` | 8.4 b) | Corrected errors |
| `release_communications.content.safety_security_impact` | 8.4 c) | Safety/security impact |
| `release_communications.content.identification_updates` | 8.4 d) | Identification updates |
| `release_communications.content.documentation_updates` | 8.4 e) | Documentation updates |
| `release_communications.recommendation` | 8.4 | Replacement recommendation |
| `decommissioning_information` | 8.5 | Decommissioning/disposal |

## Examples

### Input: CHANGELOG.md

```markdown
# Changelog

## [2.1.0] - 2024-03-15
### Added
- Blood glucose tracking feature
- Export to PDF functionality

### Fixed
- Fixed incorrect unit conversion for weight (kg/lbs)
- Resolved data sync issue on slow connections

### Security
- Updated authentication library (CVE-2024-1234)

## [2.0.1] - 2024-02-01
### Security
- Critical security patch for authentication bypass
```

### Input: SECURITY.md

```markdown
# Security Policy

## Reporting a Vulnerability
Email security@healthtrack.example.com

## Security Advisories
- 2024-02-01: Authentication bypass vulnerability (fixed in 2.0.1)
```

### Output: Extracted Post-Market Data (partial)

```json
{
  "release_history": [
    {
      "version": "2.1.0",
      "release_date": "2024-03-15",
      "release_type": "minor",
      "maintenance_type": "perfective",
      "summary": "New features and bug fixes",
      "changes": {
        "features": [
          {"description": "Blood glucose tracking feature"},
          {"description": "Export to PDF functionality"}
        ],
        "fixes": [
          {"description": "Fixed incorrect unit conversion for weight (kg/lbs)", "safety_impact": "medium"},
          {"description": "Resolved data sync issue on slow connections", "safety_impact": "low"}
        ],
        "security": [
          {"description": "Updated authentication library", "cve": "CVE-2024-1234", "severity": "medium"}
        ]
      }
    }
  ],

  "security_communications": [
    {
      "communication_id": "SEC-COMM-001",
      "date": "2024-02-01",
      "type": "vulnerability_disclosure",
      "vulnerability": {
        "description": "Authentication bypass vulnerability",
        "severity": "critical",
        "affected_versions": ["2.0.0"],
        "fixed_version": "2.0.1"
      },
      "communication": {
        "recipients": "users",
        "recommended_action": "Update to version 2.0.1 immediately"
      }
    }
  ]
}
```

## Validation Criteria

- [ ] All releases are documented with change type and summary 🆔 iVjLZU
- [ ] Maintenance activities reference 62304 clause 5 activities repeated 🆔 5AVBGC
- [ ] Security updates include CVE references where applicable 🆔 I42CLA
- [ ] Re-validation is documented for changes affecting use requirements 🆔 E37D84
- [ ] Platform compatibility is confirmed after changes 🆔 Eu5NuG
- [ ] Security vulnerabilities are communicated with severity and action 🆔 HdvV5K
- [ ] Release communications cover all 8.4 elements (a-e) 🆔 06GKRZ
- [ ] Urgency recommendations are provided for safety/security releases 🆔 dyrSDU
- [ ] SOUP maintenance policy is documented 🆔 faO70Z
- [ ] Feedback collection channels are documented 🆔 VeKZ8V
- [ ] Safety-relevant feedback is tracked separately 🆔 BTWuP5
- [ ] Decommissioning plan exists (even if not yet activated) 🆔 zYcU0a
