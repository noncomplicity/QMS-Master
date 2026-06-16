---
id: 725af57
title: "extraction 62304 architecture"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "5.3"
inputs: ["source code structure", "import/dependency graphs", "configuration files", "deployment manifests"]
outputs: ["architecture.json", "component diagrams", "interface definitions"]
software_class: "all"
process: "[[Software Development Process.canvas]]"
requirements: "[[EN 62304 5.3 Software architectural design]]"
owner: "[[Head of Quality Management]]"
---

# Extract Software Architecture from Code

## Context

IEC 62304 clause 5.3 requires documentation of software architecture showing decomposition into software items and their relationships. In code-as-truth, architecture is extracted from actual code organization, imports, and runtime dependencies.

This prompt instructs AI workers to analyze codebase structure and produce architectural documentation that reflects the implemented system.

## Inputs

### Directory Structure
- Module/package organization
- Naming conventions indicating layering
- Separation of concerns patterns

### Dependency Information
- Package manifests (`package.json`, `requirements.txt`, `go.mod`, `Cargo.toml`)
- Import statements and module references
- Dependency injection configurations
- Service registrations

### Configuration & Deployment
- Environment configurations
- Docker/container definitions
- Infrastructure-as-code (Terraform, CloudFormation)
- CI/CD pipeline definitions

### Runtime Behavior
- Entry points and bootstrapping
- Message queue subscriptions
- Scheduled job definitions
- Event handler registrations

## Instructions

1. **Identify Software Items**
   Per IEC 62304 terminology:
   - **SOFTWARE SYSTEM**: The complete medical device software
   - **SOFTWARE ITEM**: Any identifiable part (subsystem, module, package)
   - **SOFTWARE UNIT**: Lowest level item (function, class, component)

   Map codebase to this hierarchy:
   ```
   SOFTWARE SYSTEM
   └── SOFTWARE ITEM (subsystem/service)
       └── SOFTWARE ITEM (module/package)
           └── SOFTWARE UNIT (class/function)
   ```

2. **Extract Component Boundaries**
   Identify architectural boundaries:
   - Package/module boundaries
   - Service boundaries (microservices, APIs)
   - Layer boundaries (presentation, business, data)
   - Domain boundaries (bounded contexts)

3. **Map Dependencies**
   For each software item, document:
   - Internal dependencies (other software items)
   - External dependencies (SOUP/OTS)
   - Interface contracts (APIs, events, shared data)
   - Direction of dependencies (who depends on whom)

4. **Identify SOUP/OTS Components** (5.3.3, 5.3.4)
   Catalog all third-party dependencies:
   - Name and version
   - Purpose/functionality used
   - Software items that depend on it
   - Known anomalies or limitations
   - **Functional requirements** for SOUP (what it must do)
   - **Performance requirements** for SOUP (timing, throughput, capacity)
   - **Hardware requirements** (CPU, memory, storage needed by SOUP)
   - **Software environment** (OS, runtime, other dependencies)

5. **Document Interfaces**
   For each interface between software items:
   - Interface type (API, event, shared database, file)
   - Data exchanged (formats, schemas)
   - Protocol (HTTP, gRPC, message queue)
   - Error handling behavior

6. **Assess Segregation for Safety** (5.3.5 - Class C)
   Identify segregation mechanisms:
   - How safety-critical items are isolated
   - Boundaries preventing failure propagation
   - Defensive programming measures
   - **Segregation effectiveness verification**:
     - Memory isolation (separate address spaces)
     - Process/thread isolation
     - Resource partitioning (CPU, I/O)
     - Failure containment mechanisms
     - Watchdog/monitoring for safety items

7. **Architecture Verification Evidence** (5.3.6)
   Collect evidence that architecture:
   - Implements system and software requirements
   - Implements risk control measures
   - Supports interfaces between software items and hardware
   - Supports proper operation of SOUP items

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>"
  },
  "software_system": {
    "name": "<system-name>",
    "description": "<high-level description>",
    "safety_class": "A | B | C",
    "entry_points": ["<entry-point-path>"]
  },
  "software_items": [
    {
      "item_id": "SI-<hierarchy>",
      "name": "<item-name>",
      "path": "<directory-or-file-path>",
      "level": "system | subsystem | module | unit",
      "description": "<purpose and responsibility>",
      "safety_class": "A | B | C",
      "parent": "<parent-item_id>",
      "children": ["<child-item_id>"],
      "dependencies": {
        "internal": [
          {
            "item_id": "<item_id>",
            "interface": "<interface-name>",
            "coupling": "tight | loose"
          }
        ],
        "external": [
          {
            "soup_id": "<soup-reference>",
            "usage": "<what functionality is used>"
          }
        ]
      },
      "interfaces_provided": ["<interface-id>"],
      "interfaces_consumed": ["<interface-id>"]
    }
  ],
  "interfaces": [
    {
      "interface_id": "IF-<sequence>",
      "name": "<interface-name>",
      "type": "api | event | database | file | ipc",
      "provider": "<item_id>",
      "consumers": ["<item_id>"],
      "protocol": "<protocol-name>",
      "data_format": "<json | protobuf | xml | binary>",
      "schema_location": "<path-to-schema>",
      "error_handling": "<description of error behavior>"
    }
  ],
  "soup_components": [
    {
      "soup_id": "SOUP-<sequence>",
      "name": "<package-name>",
      "version": "<version>",
      "source": "<npm | pypi | crates.io | maven | etc>",
      "purpose": "<why it's used>",
      "functionality_used": ["<specific functions/modules used>"],
      "dependent_items": ["<item_id>"],
      "safety_class": "A | B | C",
      "anomalies": ["<known issues or limitations>"],
      "verification": "<how correctness is ensured>",
      "functional_requirements": ["<what SOUP must do>"],
      "performance_requirements": {
        "timing": "<response time constraints>",
        "throughput": "<transactions/second>",
        "capacity": "<data volume limits>"
      },
      "hardware_requirements": {
        "cpu": "<processing requirements>",
        "memory": "<RAM requirements>",
        "storage": "<disk requirements>",
        "other": "<other hardware needs>"
      },
      "software_environment": {
        "os": ["<supported operating systems>"],
        "runtime": "<required runtime/interpreter>",
        "dependencies": ["<other required software>"]
      }
    }
  ],
  "safety_segregation": {
    "mechanisms": [
      {
        "type": "process_isolation | container | namespace | module_boundary",
        "description": "<how segregation is achieved>",
        "protected_items": ["<item_id>"]
      }
    ],
    "failure_propagation_controls": ["<description>"]
  },
  "architectural_patterns": ["<pattern-name: e.g., MVC, Microservices, Event-Driven>"],
  "deployment_topology": {
    "environments": ["<dev | staging | production>"],
    "components": [
      {
        "name": "<deployment-component>",
        "software_items": ["<item_id>"],
        "infrastructure": "<kubernetes | vm | serverless | etc>"
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `software_items` | 5.3.1 | Transform requirements into architecture |
| `software_items[].level` | 5.3.2 | Architecture for interfaces of software items |
| `soup_components[].functional_requirements` | 5.3.3 | Functional and performance requirements for SOUP |
| `soup_components[].hardware_requirements` | 5.3.4 | System hardware/software required by SOUP |
| `safety_segregation` | 5.3.5 | Segregation necessary for risk control (Class C) |
| `interfaces` | 5.3.6 a) | Architecture implements requirements |
| `interfaces` | 5.3.6 b) | Architecture supports interfaces |
| `soup_components[].software_environment` | 5.3.6 c) | Architecture supports SOUP operation |

## Examples

### Input: Node.js Project Structure

```
src/
├── api/
│   ├── routes/
│   │   ├── patients.ts
│   │   └── vitals.ts
│   └── middleware/
│       └── auth.ts
├── services/
│   ├── patient-service.ts
│   └── vital-service.ts
├── domain/
│   ├── patient.ts
│   └── vital.ts
├── infrastructure/
│   ├── database.ts
│   └── message-queue.ts
└── index.ts
```

```json
// package.json (partial)
{
  "dependencies": {
    "express": "^4.18.2",
    "pg": "^8.11.0",
    "amqplib": "^0.10.3"
  }
}
```

### Output: Extracted Architecture

```json
{
  "software_items": [
    {
      "item_id": "SI-01",
      "name": "API Layer",
      "path": "src/api/",
      "level": "subsystem",
      "description": "HTTP API endpoints exposing patient and vital sign functionality",
      "safety_class": "B",
      "children": ["SI-01-01", "SI-01-02"]
    },
    {
      "item_id": "SI-01-01",
      "name": "Patient Routes",
      "path": "src/api/routes/patients.ts",
      "level": "module",
      "description": "REST endpoints for patient CRUD operations",
      "safety_class": "B",
      "parent": "SI-01",
      "dependencies": {
        "internal": [
          {"item_id": "SI-02-01", "interface": "PatientService", "coupling": "loose"}
        ]
      }
    }
  ],
  "soup_components": [
    {
      "soup_id": "SOUP-01",
      "name": "express",
      "version": "4.18.2",
      "source": "npm",
      "purpose": "HTTP server framework",
      "functionality_used": ["routing", "middleware", "request parsing"],
      "dependent_items": ["SI-01"],
      "safety_class": "B",
      "anomalies": [],
      "verification": "Integration tests cover all API endpoints"
    },
    {
      "soup_id": "SOUP-02",
      "name": "pg",
      "version": "8.11.0",
      "source": "npm",
      "purpose": "PostgreSQL database driver",
      "functionality_used": ["query execution", "connection pooling"],
      "dependent_items": ["SI-04-01"],
      "safety_class": "B",
      "anomalies": ["Connection timeout handling requires explicit configuration"],
      "verification": "Database integration tests validate query behavior"
    }
  ]
}
```

## Validation Criteria

- [ ] All directories with code have corresponding software items 🆔 CRm1In
- [ ] Software items form a complete hierarchy from system to units 🆔 G8rDSB
- [ ] All external dependencies are catalogued as SOUP 🆔 uOjL0J
- [ ] Interfaces between items are documented with data formats 🆔 KvHcQv
- [ ] Safety classifications are assigned and justified 🆔 9zcxuu
- [ ] Dependency directions are captured (no cycles at item level) 🆔 GFMEBV
- [ ] SOUP versions match package manifest exactly 🆔 XKsXr8
- [ ] Entry points are identified and documented 🆔 bAaHWQ
