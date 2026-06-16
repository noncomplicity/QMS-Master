---
id:
title: "Extract Software Item Architecture"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304"
clause: "5.3"
inputs: ["source code structure", "import/dependency graphs", "configuration files"]
outputs: ["item-architecture.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Software Item Architecture

## Context

IEC 62304 clause 5.3 requires documentation of software architecture. In a hierarchical documentation model, each **software item** (repository) has its own architecture showing internal components and their relationships.

**Item-level focus:** This prompt extracts architecture **within a single repository**. It identifies:
- Components internal to this item
- Interfaces provided to other items
- Interfaces consumed from other items
- SOUP dependencies used by this item

This output feeds into:
- Module-level architecture aggregation
- System-level architecture documentation
- Risk analysis (component failure modes)

## Inputs

### Required
- **Source code** — Directory structure, packages, modules
- **Import statements** — Internal and external dependencies
- **Configuration files** — Dependency injection, service registration

### Optional (enriches output)
- **API definitions** — OpenAPI, GraphQL schemas
- **Deployment configs** — Docker, Kubernetes
- **CI/CD pipelines** — Build and deployment structure

### Hierarchy Context
- **item_id** — Identifier for this software item
- **item_name** — Human-readable name
- **parent_module** — Module this item belongs to (if known)
- **parent_system** — System this item belongs to (if known)

## Instructions

### 1. Identify Item Boundary

Define what constitutes THIS software item:
- Root directory of the repository
- Main deployable artifact(s)
- Entry point(s)

This item is ONE node in the larger system. Don't model the entire system.

### 2. Extract Internal Components

Identify architectural components WITHIN this item:
- **Subsystems** — Major functional areas (e.g., `api/`, `service/`, `domain/`)
- **Modules** — Logical groupings (e.g., `appointments/`, `patients/`)
- **Layers** — Architectural layers (controller, service, repository)

For each component:
- Assign ID: `ITEM-<item>-COMP-<seq>`
- Document purpose and responsibility
- Identify internal dependencies

### 3. Map Internal Dependencies

For each component, document:
- Which other components it depends on
- Type of dependency (function call, event, data)
- Coupling level (tight/loose)

Identify dependency direction to detect layering violations.

### 4. Extract Provided Interfaces

Interfaces this item EXPOSES to other items:
- REST API endpoints
- JMS/Kafka topics published
- Database schemas owned
- Event types emitted

For each provided interface:
- Assign ID: `ITEM-<item>-INT-<seq>`
- Document contract (endpoint, schema, protocol)
- Note which internal component provides it

### 5. Extract Consumed Interfaces

Interfaces this item CONSUMES from other items:
- External REST APIs called
- JMS/Kafka topics subscribed
- External databases accessed
- Events consumed

For each consumed interface:
- Note the external system/item
- Document expected contract
- Note which internal component consumes it

### 6. Extract SOUP Dependencies

Third-party libraries used by this item:
- Direct dependencies from package manifest
- Transitive dependencies (if safety-relevant)

For each SOUP:
- Assign ID: `ITEM-<item>-SOUP-<seq>`
- Document purpose and functionality used
- Note which components depend on it

### 7. Identify Safety Segregation (Class B, C)

If safety-critical components exist:
- How are they isolated?
- What prevents failure propagation?
- What monitoring exists?

### 8. Create Traceability

Link architecture to:
- **Upward**: System/module requirements (if known)
- **Downward**: Source files implementing each component
- **Lateral**: Item requirements this architecture supports

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<git remote URL>",
    "commit": "<commit hash>",
    "extracted_at": "<ISO-8601 timestamp>",
    "extractor_version": "2.0",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "item",
      "item_id": "<software-item-id>",
      "item_name": "<human-readable name>",
      "parent_module": "<module-id or null>",
      "parent_system": "<system-id or null>"
    }
  },

  "item_architecture": {
    "item_id": "<software-item-id>",
    "item_name": "<human-readable name>",
    "description": "<item purpose>",
    "safety_class": "A | B | C",
    "primary_technology": "<main language/framework>",
    "architectural_style": "layered | microkernel | event-driven | etc",

    "entry_points": [
      {
        "name": "<entry point name>",
        "type": "http_server | cli | scheduled_job | message_listener",
        "path": "<source file path>",
        "description": "<what it starts>"
      }
    ],

    "components": [
      {
        "id": "ITEM-<item>-COMP-<seq>",
        "name": "<component name>",
        "type": "subsystem | module | layer | package",
        "path": "<directory path>",
        "description": "<purpose and responsibility>",
        "safety_class": "A | B | C",
        "internal_dependencies": [
          {
            "component_id": "ITEM-<item>-COMP-<seq>",
            "dependency_type": "function_call | event | data",
            "coupling": "tight | loose",
            "description": "<nature of dependency>"
          }
        ],
        "soup_dependencies": ["ITEM-<item>-SOUP-<seq>"],
        "source_files": ["<path>:<line-range>"]
      }
    ],

    "provided_interfaces": [
      {
        "id": "ITEM-<item>-INT-<seq>",
        "name": "<interface name>",
        "type": "rest_api | jms_topic | database | event | grpc",
        "direction": "provided",
        "protocol": "HTTP | JMS | JDBC | etc",
        "description": "<what this interface does>",
        "contract": {
          "endpoint": "<URL pattern or topic name>",
          "method": "<HTTP method if applicable>",
          "request_schema": "<schema reference or description>",
          "response_schema": "<schema reference or description>",
          "error_codes": ["<code>: <meaning>"]
        },
        "provided_by_component": "ITEM-<item>-COMP-<seq>",
        "consumers": "<external items that use this, if known>",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "consumed_interfaces": [
      {
        "id": "ITEM-<item>-EXT-<seq>",
        "name": "<interface name>",
        "type": "rest_api | jms_topic | database | event | grpc",
        "direction": "consumed",
        "protocol": "HTTP | JMS | JDBC | etc",
        "description": "<what this interface provides>",
        "external_system": "<name of external system/item>",
        "contract": {
          "endpoint": "<URL or topic name>",
          "expected_schema": "<what we expect>"
        },
        "consumed_by_component": "ITEM-<item>-COMP-<seq>",
        "failure_handling": "<what happens if unavailable>",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "soup_dependencies": [
      {
        "id": "ITEM-<item>-SOUP-<seq>",
        "name": "<package name>",
        "version": "<version>",
        "source": "npm | maven | pypi | etc",
        "purpose": "<why used>",
        "functionality_used": ["<specific features>"],
        "used_by_components": ["ITEM-<item>-COMP-<seq>"],
        "safety_relevant": true | false,
        "known_anomalies": ["<issues>"],
        "license": "<license type>"
      }
    ],

    "safety_segregation": {
      "applicable": true | false,
      "mechanisms": [
        {
          "type": "process_isolation | container | thread_boundary | module_boundary",
          "description": "<how segregation is achieved>",
          "protected_components": ["ITEM-<item>-COMP-<seq>"],
          "verification": "<how it's verified>"
        }
      ],
      "failure_propagation_controls": ["<description>"]
    },

    "data_flows": [
      {
        "id": "ITEM-<item>-FLOW-<seq>",
        "name": "<flow name>",
        "description": "<what data flows>",
        "path": ["ITEM-<item>-COMP-<seq>", "..."],
        "data_type": "<type of data>",
        "safety_relevant": true | false
      }
    ]
  },

  "traceability": {
    "to_parent_requirements": [
      {
        "component_id": "ITEM-<item>-COMP-<seq>",
        "parent_requirement_ids": ["<if known>"]
      }
    ],
    "to_item_requirements": [
      {
        "component_id": "ITEM-<item>-COMP-<seq>",
        "item_requirement_ids": ["ITEM-<item>-REQ-*"]
      }
    ]
  },

  "gaps": [
    {
      "id": "ITEM-<item>-ARCH-GAP-<seq>",
      "type": "undocumented_component | missing_interface_contract | circular_dependency | missing_segregation",
      "description": "<what's missing>",
      "affected_components": ["ITEM-<item>-COMP-<seq>"],
      "recommendation": "<suggested action>",
      "priority": "high | medium | low"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `components[]` | 5.3.1 | Architecture showing software items |
| `provided_interfaces[]` | 5.3.2 | Interface definitions |
| `consumed_interfaces[]` | 5.3.2 | External interface dependencies |
| `soup_dependencies[]` | 5.3.3 | SOUP functional requirements |
| `soup_dependencies[].known_anomalies` | 5.3.4 | SOUP limitations |
| `safety_segregation` | 5.3.5 | Segregation for risk control |
| `traceability.to_item_requirements` | 5.3.6 | Architecture implements requirements |
| `data_flows[]` | 5.3.6 | Data flow through architecture |
| `gaps[]` | 5.3.6 | Architecture completeness |

## Validation Criteria

- [ ] All major directories have corresponding components 🆔 4Cqy1M
- [ ] Component hierarchy reflects actual code organization 🆔 LzbFqd
- [ ] All external calls are documented as consumed interfaces 🆔 uorRct
- [ ] All exposed APIs are documented as provided interfaces 🆔 V5rjDs
- [ ] SOUP dependencies match package manifest 🆔 hsUCnk
- [ ] Internal dependencies show no unintended circular references 🆔 f4hI6L
- [ ] Safety-critical components have segregation documented 🆔 AyP5LU
- [ ] Entry points are identified 🆔 vLIGD5
- [ ] Hierarchy metadata is correctly populated 🆔 DiIibM
