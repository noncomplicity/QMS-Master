---
id: 725af57
title: "system aggregation soup"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "aggregation"
level: "system"
standard: "IEC 62304"
clause: "8.1"
inputs: ["item-soup.json (multiple)", "module-soup.json (multiple)"]
outputs: ["system-soup.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Aggregate System SOUP List

## Context

IEC 62304 clause 8.1 requires a complete list of SOUP for the medical device software. For multi-item systems, this must aggregate SOUP from all software items while deduplicating shared dependencies.

This aggregation prompt:
1. Collects `item-soup.json` from all software items
2. Deduplicates SOUP that appear in multiple items
3. Identifies version conflicts across items
4. Consolidates vulnerability information
5. Creates system-level SOUP qualification requirements
6. Produces the complete `system-soup.json`

The aggregated SOUP list is then used to generate:
- `System-SOUP-List.md` — Complete SOUP documentation
- SOUP qualification records

## Inputs

### Required
- **`items/<item>/extracted/item-soup.json`** — SOUP from each software item
- **`system-manifest.json`** — System structure defining modules and items

### Optional (enriches output)
- **`modules/<module>/aggregated/module-soup.json`** — Module-level SOUP aggregations
- **Vulnerability database** — Current CVE information
- **License policy** — Approved/prohibited licenses

## Instructions

### 1. Collect Item SOUP

For each software item defined in `system-manifest.json`:
1. Load `item-soup.json`
2. Validate hierarchy metadata matches manifest
3. Collect all SOUP items including runtime dependencies

### 2. Deduplicate SOUP

SOUP appearing in multiple items should be consolidated:
- Match by package name AND registry
- Document which items use each SOUP
- Note version differences (conflict vs. compatible ranges)

```
System SOUP: lodash
  ├── ITEM-HM-SOUP-005: lodash@4.17.21
  └── ITEM-API-SOUP-003: lodash@4.17.21
  Status: Consistent version
```

### 3. Identify Version Conflicts

Flag cases where items use different versions of the same SOUP:
- Different major versions (likely incompatible)
- Different minor/patch versions (review needed)
- Provide recommendation (align to latest, pin specific version)

```
CONFLICT: axios
  ├── ITEM-HM-SOUP-010: axios@0.21.4
  └── ITEM-API-SOUP-015: axios@1.4.0
  Recommendation: Upgrade ITEM-HM to axios@1.x
```

### 4. Aggregate Vulnerability Information

Consolidate known vulnerabilities across all items:
- Combine CVE lists (deduplicate)
- Note which items are affected by each CVE
- Calculate system-wide vulnerability exposure

### 5. Consolidate License Information

Create system-level license analysis:
- All unique licenses used
- Copyleft dependencies (compliance obligations)
- License conflicts or concerns
- License compatibility assessment

### 6. Determine System SOUP Risk

For each unique SOUP at system level:
- Use highest risk classification from any item using it
- Consider broader system context
- Identify safety-relevant SOUP (affects any safety-critical path)

### 7. Create Qualification Requirements

Consolidate qualification needs:
- Documentation requirements
- Testing requirements
- Verification status across items
- Gaps in qualification

### 8. Generate Traceability

Create bidirectional traceability:
- System SOUP → Item SOUP instances → Components → Source files
- System SOUP → System requirements
- System SOUP → System hazards

## Output Schema

```json
{
  "aggregation_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "system",
      "system_id": "<system-id>",
      "system_name": "<system name>",
      "child_modules": ["<module-ids>"],
      "child_items": ["<item-ids>"]
    },
    "source_extractions": [
      {
        "item_id": "<item-id>",
        "commit": "<commit hash>",
        "extracted_at": "<timestamp>",
        "soup_count": 0
      }
    ]
  },

  "summary": {
    "total_unique_soup": 0,
    "total_instances": 0,
    "production_soup": 0,
    "development_soup": 0,
    "items_with_soup": ["<item-ids>"],
    "version_conflicts": 0,
    "safety_relevant_soup": 0,
    "critical_risk_soup": 0,
    "known_vulnerabilities": 0,
    "unresolved_vulnerabilities": 0
  },

  "system_soup": [
    {
      "id": "SYS-<sys>-SOUP-<seq>",
      "name": "<package name>",
      "registry": "npm | pypi | maven | etc",
      "instances": [
        {
          "item_id": "<item-id>",
          "item_soup_id": "ITEM-<item>-SOUP-<seq>",
          "version": "<version used>",
          "environment": "production | development",
          "components_using": ["ITEM-<item>-COMP-<seq>"]
        }
      ],
      "version_status": {
        "consistent": true | false,
        "versions_in_use": ["<version1>", "<version2>"],
        "recommended_version": "<version>",
        "conflict_severity": "none | minor | major",
        "resolution": "<recommended action>"
      },
      "license": {
        "type": "<SPDX identifier>",
        "copyleft": true | false,
        "compatible": true | false,
        "notes": "<license concerns>"
      },
      "risk_classification": "critical | major | minor | negligible",
      "risk_rationale": "<system-level justification>",
      "safety_relevant": true | false,
      "safety_analysis": "<system-level safety impact>",
      "items_affected": ["<item-ids>"],
      "known_vulnerabilities": [
        {
          "cve_id": "<CVE-XXXX-XXXXX>",
          "severity": "critical | high | medium | low",
          "description": "<description>",
          "fixed_in": "<version or 'unfixed'>",
          "items_affected": ["<item-ids>"],
          "system_exposure": "critical | limited | none",
          "mitigation": "<system-level mitigation>"
        }
      ],
      "qualification": {
        "documentation_required": ["<artifacts>"],
        "testing_required": ["<approach>"],
        "verification_status": "not_started | in_progress | complete",
        "verification_by_item": {
          "<item-id>": "verified | partial | unverified"
        }
      },
      "source": {
        "repository_url": "<URL>",
        "package_url": "<URL>"
      }
    }
  ],

  "runtime_soup": [
    {
      "id": "SYS-<sys>-RT-<seq>",
      "type": "os | runtime | database | message_queue | cache",
      "name": "<component name>",
      "version": "<version>",
      "items_using": ["<item-ids>"],
      "instances": [
        {
          "item_id": "<item-id>",
          "item_rt_id": "ITEM-<item>-RT-<seq>",
          "version": "<version>"
        }
      ],
      "version_status": {
        "consistent": true | false,
        "versions_in_use": ["<versions>"],
        "resolution": "<if inconsistent>"
      },
      "safety_relevant": true | false
    }
  ],

  "version_conflicts": [
    {
      "package": "<package name>",
      "registry": "<registry>",
      "conflict_type": "major | minor | patch",
      "versions": [
        {
          "item_id": "<item-id>",
          "version": "<version>"
        }
      ],
      "risk": "high | medium | low",
      "recommendation": "<how to resolve>",
      "priority": "high | medium | low"
    }
  ],

  "license_summary": {
    "all_licenses": {
      "<SPDX>": {
        "count": 0,
        "copyleft": true | false,
        "compatible": true | false
      }
    },
    "copyleft_dependencies": [
      {
        "soup_id": "SYS-<sys>-SOUP-<seq>",
        "name": "<package>",
        "license": "<license>",
        "obligation": "<compliance requirement>"
      }
    ],
    "license_conflicts": [
      {
        "description": "<conflict description>",
        "affected_soup": ["<SOUP IDs>"],
        "recommendation": "<resolution>"
      }
    ],
    "unknown_licenses": [
      {
        "soup_id": "SYS-<sys>-SOUP-<seq>",
        "name": "<package>",
        "license_text": "<raw text>"
      }
    ]
  },

  "security_summary": {
    "total_vulnerabilities": 0,
    "by_severity": {
      "critical": 0,
      "high": 0,
      "medium": 0,
      "low": 0
    },
    "unresolved": [
      {
        "cve_id": "<CVE>",
        "soup": "<SOUP name>",
        "severity": "<severity>",
        "items_affected": ["<items>"],
        "action_required": "<action>"
      }
    ],
    "mitigated": [
      {
        "cve_id": "<CVE>",
        "soup": "<SOUP name>",
        "mitigation": "<how mitigated>"
      }
    ]
  },

  "qualification_summary": {
    "total_requiring_qualification": 0,
    "complete": 0,
    "in_progress": 0,
    "not_started": 0,
    "gaps": [
      {
        "soup_id": "SYS-<sys>-SOUP-<seq>",
        "name": "<package>",
        "gap_type": "no_documentation | no_testing | incomplete",
        "priority": "high | medium | low",
        "recommendation": "<action>"
      }
    ]
  },

  "gaps": [
    {
      "id": "SYS-<sys>-SOUP-GAP-<seq>",
      "type": "version_conflict | vulnerability | license | qualification",
      "description": "<description>",
      "affected_soup": ["SYS-<sys>-SOUP-<seq>"],
      "affected_items": ["<item-ids>"],
      "priority": "high | medium | low",
      "recommendation": "<action>"
    }
  ],

  "traceability": {
    "soup_to_items": [
      {
        "system_soup_id": "SYS-<sys>-SOUP-<seq>",
        "item_soup_ids": ["ITEM-<item>-SOUP-<seq>"]
      }
    ],
    "soup_to_hazards": [
      {
        "system_soup_id": "SYS-<sys>-SOUP-<seq>",
        "system_hazard_ids": ["SYS-<sys>-HAZ-<seq>"]
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `system_soup[]` | 8.1.1 | SOUP identification |
| `system_soup[].name/version` | 8.1.2 a) | SOUP title and manufacturer |
| `system_soup[].instances[].components_using` | 8.1.2 b) | SOUP unique designation |
| `system_soup[].version_status` | 8.1.2 c) | SOUP version |
| `system_soup[].known_vulnerabilities` | 8.1.2 d) | Known anomalies |
| `system_soup[].risk_classification` | 8.1.2 e) | Criticality assessment |
| `system_soup[].qualification` | 8.1.2 | Verification requirements |
| `gaps` | 8.1.3 | SOUP change evaluation |

## Aggregation Rules

### Version Conflict Severity

```
Major version difference (e.g., 1.x vs 2.x): HIGH
  - Likely breaking changes
  - Recommend alignment

Minor version difference (e.g., 1.1 vs 1.2): MEDIUM
  - Review changelog for changes
  - Usually safe to update to latest

Patch version difference (e.g., 1.1.1 vs 1.1.2): LOW
  - Usually bug fixes only
  - Recommend latest patch
```

### Risk Classification Aggregation

Use **highest** risk classification from any item using the SOUP:
```
ITEM-A classifies lodash as: minor
ITEM-B classifies lodash as: major (uses in critical path)
System classification: major
```

### Vulnerability Aggregation

- Deduplicate CVEs across items
- Track which items are affected by each CVE
- System exposure = MAX(item exposures)

### Qualification Status Aggregation

Use **weakest** qualification status:
```
ITEM-A qualification: complete
ITEM-B qualification: in_progress
System status: in_progress
```

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 ic9ibW
- [ ] SOUP from all items is aggregated 🆔 NcQfQi
- [ ] Deduplication is complete (no duplicate SOUP entries) 🆔 nwutPf
- [ ] Version conflicts are identified and assessed 🆔 XCSK3u
- [ ] All licenses are documented 🆔 Jr12Na
- [ ] License conflicts are identified 🆔 XBfcMW
- [ ] Vulnerabilities are consolidated 🆔 3ilOMZ
- [ ] Qualification status is aggregated 🆔 A8ryUo
- [ ] Safety-relevant SOUP is identified 🆔 kgkFN7
- [ ] Gaps are documented with priority 🆔 qe1fLS
- [ ] Full traceability to item SOUP and components 🆔 LAq3UT
