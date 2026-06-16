---
id:
title: "Extract SOUP List from Dependencies"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "8.1"
inputs: ["package manifests", "lock files", "build configurations", "container images"]
outputs: ["soup-list.json", "soup risk assessment inputs"]
software_class: "all"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract SOUP List from Dependencies

## Context

IEC 62304 clause 8 requires identification and management of Software of Unknown Provenance (SOUP). SOUP includes any third-party software, libraries, frameworks, or components not developed under the manufacturer's QMS.

This prompt instructs AI workers to comprehensively catalog all SOUP from dependency manifests and assess their qualification requirements.

## Inputs

### Package Manifests
- `package.json` / `package-lock.json` (Node.js)
- `requirements.txt` / `Pipfile.lock` / `poetry.lock` (Python)
- `go.mod` / `go.sum` (Go)
- `Cargo.toml` / `Cargo.lock` (Rust)
- `pom.xml` / `build.gradle` (Java)
- `*.csproj` / `packages.config` (C#/.NET)
- `Gemfile.lock` (Ruby)

### Container/Infrastructure
- `Dockerfile` base images
- Kubernetes manifests
- System package installations (`apt`, `yum`, `apk`)

### Build Tools
- CI/CD pipeline configurations
- Build scripts
- Compiler/runtime versions

### Existing Documentation
- Previous SOUP lists
- Vendor documentation
- Known anomaly registers

## Instructions

1. **Catalog Direct Dependencies**
   For each package manifest:
   - Extract name, version, source registry
   - Identify license type
   - Note version constraints (pinned, range, latest)

2. **Analyze Transitive Dependencies**
   From lock files:
   - Build complete dependency tree
   - Identify total transitive dependency count
   - Flag deeply nested critical dependencies

3. **Identify Runtime SOUP**
   Catalog non-code dependencies:
   - Operating system / base container image
   - Runtime environments (Node.js, Python, JVM)
   - Databases, message queues, caches
   - Cloud services with embedded logic

4. **Assess SOUP Usage**
   For each SOUP item, determine:
   - What functionality is used
   - Is it in the critical path for safety features?
   - What happens if it fails or behaves unexpectedly?

5. **Evaluate Maintenance Status**
   Check for each dependency:
   - Last release date
   - Open security vulnerabilities (CVEs)
   - Maintenance activity (commits, releases)
   - End-of-life or deprecation notices

6. **Classify SOUP Risk**
   Apply risk classification:
   - **Critical**: In safety-critical code path, failure could cause harm
   - **Major**: Core functionality, failure degrades system significantly
   - **Minor**: Utility functions, failure has limited impact
   - **Negligible**: Development-only, not in production code

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "manifest_files_analyzed": ["<path>"]
  },
  "summary": {
    "total_direct_dependencies": 0,
    "total_transitive_dependencies": 0,
    "critical_soup_count": 0,
    "outdated_count": 0,
    "vulnerable_count": 0
  },
  "soup_items": [
    {
      "soup_id": "SOUP-<sequence>",
      "name": "<package-name>",
      "version": "<version>",
      "version_constraint": "<constraint-from-manifest>",
      "source": {
        "registry": "<npm | pypi | crates | maven | etc>",
        "repository_url": "<source-repo-url>",
        "package_url": "<registry-url>"
      },
      "license": {
        "type": "<SPDX-identifier>",
        "compatible": true,
        "notes": "<any license concerns>"
      },
      "dependency_type": "direct | transitive",
      "dependency_chain": ["<parent>", "...", "<this-package>"],
      "purpose": "<why this package is used>",
      "functionality_used": ["<specific APIs/features used>"],
      "integration_points": {
        "software_items": ["<SI-id>"],
        "code_locations": ["<file:line>"]
      },
      "risk_classification": "critical | major | minor | negligible",
      "risk_rationale": "<justification for classification>",
      "safety_relevant": false,
      "safety_analysis": "<if safety_relevant: how it affects safety>",
      "maintenance_status": {
        "last_release": "<ISO-date>",
        "days_since_release": 0,
        "status": "active | maintained | minimal | abandoned | deprecated",
        "known_vulnerabilities": [
          {
            "cve_id": "<CVE-XXXX-XXXXX>",
            "severity": "critical | high | medium | low",
            "description": "<brief description>",
            "fixed_in": "<version or 'unfixed'>"
          }
        ]
      },
      "qualification_requirements": {
        "documentation_required": ["<required-artifacts>"],
        "testing_required": ["<testing-approach>"],
        "verification_status": "not_started | in_progress | complete"
      },
      "alternatives_considered": "<if any, why this was chosen>"
    }
  ],
  "runtime_soup": [
    {
      "soup_id": "RT-SOUP-<sequence>",
      "type": "os | runtime | database | service",
      "name": "<component-name>",
      "version": "<version>",
      "source": "<vendor or distribution>",
      "purpose": "<why required>",
      "safety_relevant": false,
      "configuration": "<relevant configuration settings>"
    }
  ],
  "action_items": [
    {
      "priority": "high | medium | low",
      "soup_id": "<soup-id>",
      "action": "<required action>",
      "rationale": "<why this action is needed>"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `soup_items[].name/version` | 8.1.1 | SOUP identification |
| `soup_items[].purpose` | 8.1.2 a) | SOUP title and manufacturer |
| `soup_items[].functionality_used` | 8.1.2 b) | SOUP identified unique designation |
| `soup_items[].version` | 8.1.2 c) | SOUP version |
| `soup_items[].maintenance_status.known_vulnerabilities` | 8.1.2 d) | Known anomalies |
| `soup_items[].risk_classification` | 8.1.2 e) | Criticality assessment |
| `soup_items[].qualification_requirements` | 8.1.2 | Verification requirements |
| `action_items` | 8.1.3 | SOUP change evaluation |

## Examples

### Input: package.json

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "pg": "^8.11.0",
    "winston": "^3.10.0",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "jest": "^29.6.0",
    "typescript": "^5.1.6"
  }
}
```

### Output: Extracted SOUP Entry

```json
{
  "soup_id": "SOUP-001",
  "name": "pg",
  "version": "8.11.0",
  "version_constraint": "^8.11.0",
  "source": {
    "registry": "npm",
    "repository_url": "https://github.com/brianc/node-postgres",
    "package_url": "https://www.npmjs.com/package/pg"
  },
  "license": {
    "type": "MIT",
    "compatible": true,
    "notes": ""
  },
  "dependency_type": "direct",
  "purpose": "PostgreSQL database connectivity for patient data persistence",
  "functionality_used": ["query execution", "connection pooling", "parameterized queries"],
  "integration_points": {
    "software_items": ["SI-INFRA-01"],
    "code_locations": ["src/infrastructure/database.ts:15"]
  },
  "risk_classification": "critical",
  "risk_rationale": "Handles all database operations including patient health records; SQL injection or data corruption could cause patient harm",
  "safety_relevant": true,
  "safety_analysis": "Database integrity directly affects clinical data accuracy; failures could result in lost or corrupted patient information",
  "maintenance_status": {
    "last_release": "2024-01-15",
    "days_since_release": 45,
    "status": "active",
    "known_vulnerabilities": []
  },
  "qualification_requirements": {
    "documentation_required": ["API documentation review", "Security advisory monitoring"],
    "testing_required": ["Integration tests for all query patterns", "Connection failure recovery tests"],
    "verification_status": "in_progress"
  }
}
```

## Validation Criteria

- [ ] All direct dependencies from manifests are catalogued 🆔 hBvEAg
- [ ] Transitive dependencies are identified with dependency chains 🆔 DLy3cQ
- [ ] Versions match lock files exactly (not manifest ranges) 🆔 KajyeQ
- [ ] Each SOUP has a risk classification with rationale 🆔 HTvzAE
- [ ] Safety-relevant SOUP items have safety analysis 🆔 AzG1LK
- [ ] Known vulnerabilities are documented with CVE IDs 🆔 cFMupk
- [ ] License types are identified for all SOUP 🆔 2XVOd7
- [ ] Abandoned/deprecated packages are flagged with action items 🆔 2z4ezs
- [ ] Runtime dependencies (OS, runtime, databases) are catalogued 🆔 VJk1Zx
- [ ] Qualification requirements are specified for Class B/C systems 🆔 WtS2ci
