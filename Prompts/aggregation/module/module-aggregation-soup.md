---
id:
title: "Aggregate Module SOUP"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "module"
standard: "IEC 62304"
clause: "8.1"
inputs: ["item-soup.json (multiple)", "system-manifest.json"]
outputs: ["module-soup.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Aggregate Module SOUP

## Context

IEC 62304 clause 8.1 requires identification and documentation of Software of Unknown Provenance (SOUP). For modules composed of multiple software items, this aggregation prompt combines item-level SOUP lists into a consolidated module SOUP inventory.

This aggregation prompt:
1. Collects `item-soup.json` from all software items in the module
2. Deduplicates SOUP used by multiple items
3. Consolidates version information
4. Aggregates risk classifications
5. Produces `module-soup.json`

The aggregated SOUP list:
- Identifies all third-party dependencies across the module
- Highlights version inconsistencies between items
- Provides input to system-level SOUP aggregation
- Supports vulnerability monitoring at module scope

## Inputs

### Required
- **`items/<item>/extracted/item-soup.json`** — SOUP list from each software item
- **`system-manifest.json`** — System structure defining which items belong to this module

### Optional (enriches output)
- **`items/<item>/extracted/item-risk-contribution.json`** — Risk information for SOUP impact assessment
- **SBOM files** — Software Bill of Materials from build systems
- **Vulnerability databases** — For known vulnerability cross-reference

### Configuration Required
- **module_id** — Identifier for this module
- **module_name** — Human-readable module name
- **parent_system** — System this module belongs to

## Instructions

### 1. Collect Item SOUP Lists

For each software item belonging to this module (from `system-manifest.json`):
1. Load `item-soup.json`
2. Validate hierarchy metadata matches expected module
3. Collect all SOUP components with their metadata

### 2. Identify Shared SOUP

Group SOUP components used by multiple items:
```
lodash:
  ├── health-manager: 4.17.21
  ├── appointment-api: 4.17.21
  └── notification-service: 4.17.15  ← Version mismatch
```

For each shared SOUP:
- Identify version consistency/inconsistency
- Document which items use which versions
- Flag security implications of version drift

### 3. Deduplicate SOUP Entries

Create a single entry per SOUP component:
- Use **newest version** as the "module version" (with notes on older versions)
- Merge usage contexts from all items
- Combine functional classifications
- Aggregate risk assessments (use highest risk)

### 4. Consolidate Risk Classifications

For SOUP used by multiple items:
- **Risk level**: Use the **highest** risk level from any item
- **Failure modes**: Union of failure modes from all items
- **Anomaly sequences**: Combine published anomaly information

### 5. Identify Version Conflicts

Flag SOUP with inconsistent versions:
- Different major versions = High concern
- Different minor versions = Medium concern
- Different patch versions = Low concern

Document resolution approach:
- Planned upgrade path
- Accepted risk for version drift
- Technical constraints preventing alignment

### 6. Assess Module-Level Impact

For each SOUP, assess impact at module scope:
- How many items depend on it?
- Does failure cascade across items?
- Are there multiple items providing redundancy?

### 7. Cross-Reference Vulnerabilities

If vulnerability data is available:
- Check all SOUP versions against known CVEs
- Flag items using vulnerable versions
- Document remediation status

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "module",
      "module_id": "<module-id>",
      "module_name": "<module name>",
      "parent_system": "<system-id>",
      "child_items": ["<item-ids>"]
    },
    "source_extractions": [
      {
        "item_id": "<item-id>",
        "commit": "<commit hash>",
        "extracted_at": "<timestamp>"
      }
    ]
  },

  "module_summary": {
    "module_id": "<module-id>",
    "total_soup_components": "<count>",
    "unique_soup_components": "<count after deduplication>",
    "shared_soup_components": "<count used by multiple items>",
    "version_conflicts": "<count of SOUP with version mismatches>",
    "high_risk_soup": "<count>",
    "known_vulnerabilities": "<count if available>"
  },

  "soup_by_item": {
    "<item-id>": {
      "item_name": "<human-readable name>",
      "soup_count": "<count>",
      "soup_ids": ["MOD-<mod>-SOUP-<seq>"]
    }
  },

  "soup_components": [
    {
      "id": "MOD-<mod>-SOUP-<seq>",
      "name": "<component name>",
      "module_version": "<consolidated version>",
      "type": "library | framework | runtime | tool | database | os_component",
      "category": "functional | infrastructure | development | security",
      "license": "<license type>",

      "usage_by_item": [
        {
          "item_id": "<item-id>",
          "item_soup_id": "ITEM-<item>-SOUP-<seq>",
          "version": "<version used by this item>",
          "purpose": "<why this item uses it>"
        }
      ],

      "version_status": {
        "consistent": true | false,
        "versions_in_use": ["<versions>"],
        "recommended_version": "<target version>",
        "upgrade_notes": "<migration considerations>"
      },

      "risk_assessment": {
        "risk_level": "low | medium | high",
        "risk_level_rationale": "<why this risk level>",
        "contributing_assessments": [
          {
            "item_id": "<item-id>",
            "risk_level": "low | medium | high",
            "rationale": "<item-specific rationale>"
          }
        ],
        "failure_impact": "<consolidated impact across module>",
        "failure_modes": ["<combined failure modes>"]
      },

      "published_anomalies": {
        "known_issues": ["<issues from all item extractions>"],
        "workarounds_applied": ["<workarounds in use>"],
        "anomaly_documentation": "<reference to anomaly list>"
      },

      "verification": {
        "verification_approach": "<how SOUP is verified>",
        "items_with_verification": ["<item-ids with verification>"],
        "items_without_verification": ["<item-ids lacking verification>"],
        "verification_gaps": ["<what's missing>"]
      },

      "vulnerability_status": {
        "last_checked": "<timestamp or null>",
        "known_cves": [
          {
            "cve_id": "<CVE-YYYY-NNNNN>",
            "severity": "low | medium | high | critical",
            "affected_versions": ["<versions>"],
            "items_affected": ["<item-ids using affected versions>"],
            "remediation_status": "fixed | in_progress | accepted | not_applicable"
          }
        ],
        "vulnerability_monitoring": "automated | manual | none"
      },

      "maintenance": {
        "vendor_support": "active | limited | end_of_life | community",
        "last_release": "<date of last release>",
        "update_frequency": "<how often updated>",
        "source_repository": "<URL if available>"
      }
    }
  ],

  "version_conflicts": [
    {
      "soup_id": "MOD-<mod>-SOUP-<seq>",
      "soup_name": "<component name>",
      "conflict_severity": "high | medium | low",
      "versions": [
        {
          "version": "<version>",
          "items": ["<item-ids>"]
        }
      ],
      "resolution": {
        "strategy": "upgrade_all | accept_drift | deprecate_older",
        "target_version": "<target version>",
        "timeline": "<when to resolve>",
        "blockers": ["<what prevents resolution>"]
      }
    }
  ],

  "cross_item_impact": [
    {
      "soup_id": "MOD-<mod>-SOUP-<seq>",
      "soup_name": "<component name>",
      "items_dependent": "<count>",
      "cascade_risk": "high | medium | low",
      "cascade_description": "<how failure would propagate>",
      "redundancy": "none | partial | full",
      "single_point_of_failure": true | false
    }
  ],

  "traceability": {
    "to_item_soup": [
      {
        "module_soup_id": "MOD-<mod>-SOUP-<seq>",
        "item_soup_ids": ["ITEM-<item>-SOUP-<seq>"]
      }
    ],
    "to_item_requirements": [
      {
        "soup_id": "MOD-<mod>-SOUP-<seq>",
        "requirement_ids": ["ITEM-<item>-REQ-<seq>"]
      }
    ]
  },

  "gaps": [
    {
      "id": "MOD-<mod>-SOUP-GAP-<seq>",
      "type": "version_conflict | missing_verification | vulnerability | maintenance_risk | undocumented_usage",
      "description": "<what's missing or problematic>",
      "affected_soup": ["MOD-<mod>-SOUP-<seq>"],
      "affected_items": ["<item-ids>"],
      "recommendation": "<suggested action>",
      "priority": "high | medium | low"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `soup_components[]` | 8.1.1 | SOUP identification |
| `soup_components[].module_version` | 8.1.2 a) | Version documentation |
| `soup_components[].license` | 8.1.2 b) | License documentation |
| `soup_components[].risk_assessment` | 7.1.3 | Risk contribution |
| `soup_components[].published_anomalies` | 8.1.2 c) | Known anomalies |
| `soup_components[].verification` | 8.1.3 | Verification approach |
| `soup_components[].maintenance` | 6.1 | Maintenance planning |
| `version_conflicts[]` | 8.1.2 | Configuration management |
| `cross_item_impact[]` | 7.1.3 | Risk analysis |
| `gaps[]` | 8.1.3 | Verification completeness |

## Aggregation Rules

### Version Consolidation
When items use different versions:
- **Module version** = newest version in use
- Document all versions in `usage_by_item`
- Flag as version conflict if major versions differ

### Risk Level Aggregation
Use the **highest** risk level from any item:
```
ITEM-A risk: low (standalone usage)
ITEM-B risk: high (security-critical path)
→ Module risk: high
```

### Verification Status Aggregation
- **Verified** = All items have verification
- **Partial** = Some items have verification
- **Unverified** = No items have verification

### Failure Mode Union
Combine all failure modes from item extractions:
```
ITEM-A failure modes: ["memory leak on large input"]
ITEM-B failure modes: ["timeout on slow network"]
→ Module failure modes: ["memory leak on large input", "timeout on slow network"]
```

### Vulnerability Aggregation
- Check all versions in use against CVE databases
- Mark item-specific impact
- Prioritize by severity × affected item count

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 jcfFMg
- [ ] Each SOUP component from items appears in module list 🆔 OxUqMA
- [ ] Version conflicts are identified and documented 🆔 GI3mH2
- [ ] Risk assessments use highest level from contributing items 🆔 lOMqE2
- [ ] Cross-item impact is assessed for shared SOUP 🆔 ihekSH
- [ ] Verification gaps are identified per item 🆔 mYNRSi
- [ ] Known vulnerabilities are documented (if available) 🆔 ODsBqm
- [ ] Maintenance status is assessed for critical SOUP 🆔 6fVgZw
- [ ] Gaps are prioritized for remediation 🆔 RWpZyM
