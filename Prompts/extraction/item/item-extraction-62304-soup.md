---
id:
title: "Extract Software Item SOUP List"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304"
clause: "8.1"
inputs: ["package manifests", "lock files", "build configurations", "container images"]
outputs: ["item-soup.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Software Item SOUP List

## Context

IEC 62304 clause 8 requires identification and management of Software of Unknown Provenance (SOUP). SOUP includes any third-party software, libraries, frameworks, or components not developed under the manufacturer's QMS.

**Item-level focus:** This prompt extracts SOUP dependencies for a **single software item** (repository). It catalogs:
- Direct dependencies declared in package manifests
- Transitive dependencies from lock files
- Runtime dependencies (OS, runtime, database clients)
- Build-time dependencies that affect production

This output feeds into:
- Module-level SOUP aggregation (deduplication across items)
- System-level SOUP list (consolidated view)
- Risk analysis (SOUP failure contributions)

## Inputs

### Required

**Package Manifests** (one or more):
- `package.json` / `package-lock.json` (Node.js)
- `requirements.txt` / `Pipfile.lock` / `poetry.lock` (Python)
- `go.mod` / `go.sum` (Go)
- `Cargo.toml` / `Cargo.lock` (Rust)
- `pom.xml` / `build.gradle` (Java)
- `*.csproj` / `packages.config` (C#/.NET)
- `Gemfile.lock` (Ruby)

### Optional (enriches output)

**Container/Infrastructure:**
- `Dockerfile` base images
- Container image manifests
- System package installations (`apt`, `yum`, `apk`)

**Build Tools:**
- CI/CD pipeline configurations
- Build scripts
- Compiler/runtime versions

**Existing Documentation:**
- Previous SOUP lists
- Vendor documentation
- Known anomaly registers

### Hierarchy Context

- **item_id** — Identifier for this software item
- **item_name** — Human-readable name
- **parent_module** — Module this item belongs to (if known)
- **parent_system** — System this item belongs to (if known)

## Instructions

### 1. Identify Package Manifests

Locate all dependency declaration files in the repository:
- Root-level manifests
- Sub-project/workspace manifests
- Build configuration files

Note the package manager and ecosystem for each.

### 2. Catalog Direct Dependencies

For each direct dependency:
- Extract name, declared version/constraint
- Identify source registry
- Determine license type
- Note if production or development-only

### 3. Analyze Transitive Dependencies

From lock files:
- Build complete dependency tree
- Identify which direct dependency brought each transitive
- Flag deeply nested dependencies (>3 levels)
- Note total transitive count

### 4. Identify Runtime SOUP

Non-code dependencies used by this item:
- Base container image / OS
- Language runtime (JVM, Node.js, Python interpreter)
- Database drivers/clients
- Message queue clients

### 5. Assess SOUP Usage

For each SOUP item, determine:
- What functionality is used
- Which components in this item depend on it
- What happens if it fails

### 6. Evaluate Maintenance Status

For each significant dependency:
- Last release date
- Known security vulnerabilities (CVEs)
- Maintenance activity indicators
- End-of-life or deprecation status

### 7. Classify SOUP Risk

Apply risk classification based on this item's usage:
- **Critical**: In safety-relevant code path, failure could contribute to harm
- **Major**: Core functionality, failure significantly degrades item
- **Minor**: Utility function, failure has limited impact
- **Negligible**: Development-only, not in production

### 8. Create Traceability

Link SOUP to:
- **Components**: Which item components use each SOUP
- **Upward**: Note for module/system aggregation
- **Risk**: Which hazards SOUP failure could contribute to

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<git remote URL>",
    "commit": "<commit hash>",
    "extracted_at": "<ISO-8601 timestamp>",
    "extractor_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "manifest_files_analyzed": ["<path>"],
    "hierarchy": {
      "level": "item",
      "item_id": "<software-item-id>",
      "item_name": "<human-readable name>",
      "parent_module": "<module-id or null>",
      "parent_system": "<system-id or null>"
    }
  },

  "summary": {
    "total_direct_dependencies": 0,
    "total_transitive_dependencies": 0,
    "production_dependencies": 0,
    "development_dependencies": 0,
    "critical_soup_count": 0,
    "major_soup_count": 0,
    "safety_relevant_count": 0,
    "outdated_count": 0,
    "vulnerable_count": 0
  },

  "soup_items": [
    {
      "id": "ITEM-<item>-SOUP-<seq>",
      "name": "<package-name>",
      "version": "<resolved version from lock file>",
      "version_constraint": "<constraint from manifest>",
      "source": {
        "registry": "npm | pypi | crates | maven | nuget | etc",
        "repository_url": "<source repo URL>",
        "package_url": "<registry URL>"
      },
      "license": {
        "type": "<SPDX identifier>",
        "compatible": true | false,
        "notes": "<license concerns if any>"
      },
      "dependency_type": "direct | transitive",
      "environment": "production | development | both",
      "dependency_chain": ["<root>", "...", "<this-package>"],
      "purpose": "<why this package is used in this item>",
      "functionality_used": ["<specific APIs/features>"],
      "used_by_components": ["ITEM-<item>-COMP-<seq>"],
      "code_locations": ["<path>:<line>"],
      "risk_classification": "critical | major | minor | negligible",
      "risk_rationale": "<justification for classification>",
      "safety_relevant": true | false,
      "safety_analysis": "<if safety_relevant: how it affects safety>",
      "failure_modes": ["<what could go wrong>"],
      "maintenance_status": {
        "last_release": "<ISO date>",
        "days_since_release": 0,
        "status": "active | maintained | minimal | abandoned | deprecated",
        "known_vulnerabilities": [
          {
            "cve_id": "<CVE-XXXX-XXXXX>",
            "severity": "critical | high | medium | low",
            "description": "<brief description>",
            "fixed_in": "<version or 'unfixed'>",
            "affects_this_item": true | false,
            "mitigation": "<if not fixed: how mitigated>"
          }
        ]
      },
      "qualification_requirements": {
        "documentation_required": ["<required artifacts>"],
        "testing_required": ["<testing approach>"],
        "verification_status": "not_started | in_progress | complete"
      },
      "traces_to": {
        "parent_module_soup": "<module-level SOUP ID if known>",
        "hazards": ["ITEM-<item>-HAZ-<seq>"],
        "requirements": ["ITEM-<item>-REQ-<seq>"]
      }
    }
  ],

  "runtime_soup": [
    {
      "id": "ITEM-<item>-RT-<seq>",
      "type": "os | runtime | database | message_queue | cache",
      "name": "<component name>",
      "version": "<version>",
      "source": "<vendor or distribution>",
      "purpose": "<why this item requires it>",
      "safety_relevant": true | false,
      "configuration_notes": "<relevant settings>",
      "used_by_components": ["ITEM-<item>-COMP-<seq>"]
    }
  ],

  "dependency_tree": {
    "depth_statistics": {
      "max_depth": 0,
      "average_depth": 0.0,
      "deep_dependencies": ["<packages at depth > 3>"]
    },
    "duplicated_packages": [
      {
        "name": "<package>",
        "versions": ["<version1>", "<version2>"],
        "reason": "<why multiple versions>"
      }
    ]
  },

  "license_summary": {
    "licenses_found": {
      "<SPDX>": 0
    },
    "copyleft_dependencies": ["<package names>"],
    "unknown_licenses": ["<package names>"],
    "license_conflicts": ["<description>"]
  },

  "gaps": [
    {
      "id": "ITEM-<item>-SOUP-GAP-<seq>",
      "type": "missing_version_lock | vulnerable_dependency | abandoned_package | missing_license | excessive_depth",
      "description": "<what's missing or problematic>",
      "affected_soup": ["ITEM-<item>-SOUP-<seq>"],
      "recommendation": "<suggested action>",
      "priority": "high | medium | low"
    }
  ],

  "action_items": [
    {
      "priority": "high | medium | low",
      "soup_id": "ITEM-<item>-SOUP-<seq>",
      "action": "<required action>",
      "rationale": "<why needed>"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `soup_items[].name/version` | 8.1.1 | SOUP identification |
| `soup_items[].purpose` | 8.1.2 a) | SOUP title and manufacturer |
| `soup_items[].functionality_used` | 8.1.2 b) | SOUP unique designation |
| `soup_items[].version` | 8.1.2 c) | SOUP version |
| `soup_items[].maintenance_status.known_vulnerabilities` | 8.1.2 d) | Known anomalies |
| `soup_items[].risk_classification` | 8.1.2 e) | Criticality assessment |
| `soup_items[].qualification_requirements` | 8.1.2 | Verification requirements |
| `action_items` | 8.1.3 | SOUP change evaluation |

## Validation Criteria

- [ ] All direct dependencies from manifests are catalogued 🆔 CXATu4
- [ ] Transitive dependencies are identified with dependency chains 🆔 wpaVCe
- [ ] Versions match lock files exactly (not manifest ranges) 🆔 OaKgou
- [ ] Each SOUP has a risk classification with rationale 🆔 ljBc0o
- [ ] Safety-relevant SOUP items have safety analysis 🆔 CFXTnk
- [ ] Known vulnerabilities are documented with CVE IDs 🆔 ZtSnAT
- [ ] License types are identified for all direct dependencies 🆔 lIESN2
- [ ] Abandoned/deprecated packages are flagged with action items 🆔 lJqSIM
- [ ] Runtime dependencies (OS, runtime, databases) are catalogued 🆔 tmDeET
- [ ] Hierarchy metadata is correctly populated 🆔 J3n2jG
- [ ] IDs follow `ITEM-<item>-SOUP-*` convention 🆔 GiCL73
- [ ] Component traceability is established 🆔 9v4bS2
