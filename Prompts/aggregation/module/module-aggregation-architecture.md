---
id:
title: "Aggregate Module Architecture"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "module"
standard: "IEC 62304"
clause: "5.3"
inputs: ["item-architecture.json (multiple)", "system-manifest.json"]
outputs: ["module-architecture.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Aggregate Module Architecture

## Context

IEC 62304 clause 5.3 requires documentation of software architecture. For modules composed of multiple software items (repositories), this aggregation prompt combines item-level architecture extractions into a coherent module architecture view.

This aggregation prompt:
1. Collects `item-architecture.json` from all software items in the module
2. Identifies inter-item interfaces and dependencies
3. Creates module-level component diagram
4. Documents data flows between items
5. Produces `module-architecture.json`

The aggregated architecture is then used to generate:
- `Module-Architecture.md` — Human-readable module architecture document
- Input to system-level architecture aggregation

## Inputs

### Required
- **`items/<item>/extracted/item-architecture.json`** — Architecture from each software item
- **`system-manifest.json`** — System structure defining which items belong to this module

### Optional (enriches output)
- **`items/<item>/extracted/item-requirements.json`** — Requirements for traceability
- **`module/integration-tests/`** — Integration test specifications
- **`module/deployment/`** — Deployment configurations showing runtime relationships

### Configuration Required
- **module_id** — Identifier for this module
- **module_name** — Human-readable module name
- **parent_system** — System this module belongs to

## Instructions

### 1. Collect Item Architectures

For each software item belonging to this module (from `system-manifest.json`):
1. Load `item-architecture.json`
2. Validate hierarchy metadata matches expected module
3. Collect all components, interfaces, dependencies

### 2. Identify Inter-Item Interfaces

Map item interfaces to module-level connections:
- **Provided APIs** — Services one item exposes that others consume
- **Consumed APIs** — Services an item requires from other items
- **Shared data stores** — Databases accessed by multiple items
- **Message queues** — Asynchronous communication channels
- **Event buses** — Event-driven integration points

For each inter-item interface:
```
MOD-<mod>-INT-001: health-manager → appointment-api
  Protocol: REST
  Endpoint: /api/appointments
  Data exchanged: AppointmentRequest, AppointmentResponse
  Source item interface: ITEM-HM-INT-005
  Target item interface: ITEM-API-INT-001
```

### 3. Create Module Component Diagram

Produce a logical view showing:
- Software items as major components
- Inter-item interfaces with protocols
- External interfaces (to other modules or external systems)
- Shared infrastructure (databases, queues, caches)

### 4. Document Data Flows

For significant data flows across items:
1. Identify the flow path (item → item → item)
2. Document transformations at each step
3. Note validation points
4. Identify data integrity controls

### 5. Aggregate Dependencies

Combine item-level dependencies:
- **Internal dependencies** — Between items within this module
- **External dependencies** — On other modules or external systems
- **Infrastructure dependencies** — Shared infrastructure components

### 6. Identify Integration Risks

Flag architecture patterns that require integration verification:
- Circular dependencies between items
- Single points of failure
- Complex data transformation chains
- Timing-sensitive interactions
- Security boundary crossings

### 7. Create Traceability

Link module architecture to:
- **Upward**: System requirements this module addresses
- **Downward**: Item architectures this aggregates
- **Lateral**: Integration tests that verify interfaces

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
    "module_name": "<human-readable name>",
    "description": "<module purpose>",
    "item_count": "<count>",
    "interface_count": "<count>",
    "external_dependency_count": "<count>"
  },

  "items": [
    {
      "item_id": "<item-id>",
      "item_name": "<human-readable name>",
      "description": "<item purpose>",
      "component_count": "<count from item-architecture>",
      "primary_technology": "<main tech stack>"
    }
  ],

  "inter_item_interfaces": [
    {
      "id": "MOD-<mod>-INT-<seq>",
      "name": "<interface name>",
      "source_item": "<item-id>",
      "source_interface": "ITEM-<item>-INT-<seq>",
      "target_item": "<item-id>",
      "target_interface": "ITEM-<item>-INT-<seq>",
      "protocol": "REST | gRPC | JMS | JDBC | etc",
      "direction": "sync | async",
      "data_exchanged": [
        {
          "name": "<data type>",
          "direction": "request | response | event",
          "schema_reference": "<path to schema>"
        }
      ],
      "error_handling": "<how errors propagate>",
      "security": "<authentication/authorization>"
    }
  ],

  "external_interfaces": [
    {
      "id": "MOD-<mod>-EXT-<seq>",
      "name": "<interface name>",
      "item_id": "<item providing interface>",
      "item_interface": "ITEM-<item>-INT-<seq>",
      "target": "<other module or external system>",
      "protocol": "REST | gRPC | etc",
      "direction": "provided | consumed",
      "description": "<interface purpose>"
    }
  ],

  "shared_infrastructure": [
    {
      "id": "MOD-<mod>-INFRA-<seq>",
      "type": "database | cache | queue | event_bus | file_storage",
      "name": "<infrastructure component name>",
      "technology": "<specific technology>",
      "items_using": ["<item-ids>"],
      "purpose": "<why shared>",
      "data_isolation": "schema | namespace | none"
    }
  ],

  "data_flows": [
    {
      "id": "MOD-<mod>-FLOW-<seq>",
      "name": "<flow name>",
      "description": "<what data flows and why>",
      "trigger": "<what initiates the flow>",
      "path": [
        {
          "step": 1,
          "item": "<item-id>",
          "component": "<component within item>",
          "action": "<what happens>",
          "data_out": "<data produced>"
        }
      ],
      "validation_points": ["<where data is validated>"],
      "safety_impact": "none | low | medium | high"
    }
  ],

  "integration_concerns": [
    {
      "id": "MOD-<mod>-CONCERN-<seq>",
      "type": "circular_dependency | single_point_of_failure | timing_sensitive | security_boundary | data_consistency",
      "description": "<what the concern is>",
      "items_affected": ["<item-ids>"],
      "interfaces_affected": ["MOD-<mod>-INT-<seq>"],
      "mitigation": "<how it's addressed or null if gap>",
      "verification_required": "<what testing is needed>"
    }
  ],

  "traceability": {
    "to_system_requirements": [
      {
        "module_component": "MOD-<mod>-INT-<seq> | <item-id>",
        "system_requirement_ids": ["SYS-<sys>-SYS-<seq>"]
      }
    ],
    "to_item_architectures": [
      {
        "module_element": "MOD-<mod>-INT-<seq>",
        "item_elements": ["ITEM-<item>-ARCH-<seq>"]
      }
    ],
    "to_integration_tests": [
      {
        "interface_id": "MOD-<mod>-INT-<seq>",
        "test_ids": ["MOD-<mod>-IT-<seq>"]
      }
    ]
  },

  "gaps": [
    {
      "id": "MOD-<mod>-GAP-<seq>",
      "type": "undocumented_interface | missing_integration_test | unclear_data_flow | security_concern",
      "description": "<what's missing>",
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
| `items[]` | 5.3.1 | Software items identified |
| `inter_item_interfaces[]` | 5.3.2 | Interfaces between items |
| `external_interfaces[]` | 5.3.2 | External interfaces |
| `shared_infrastructure[]` | 5.3.3 | Shared resources |
| `data_flows[]` | 5.3.4 | Data flow documentation |
| `integration_concerns[]` | 5.3.5 | Architectural concerns |
| `traceability.to_system_requirements` | 5.1.1 | Traceability to requirements |
| `traceability.to_integration_tests` | 5.6.3 | Integration verification |
| `gaps[]` | 5.3.6 | Architecture completeness |

## Aggregation Rules

### Interface Deduplication
When multiple items reference the same interface from different perspectives:
- Use the **provider's** definition as authoritative
- Verify consumer's expectation matches provider's contract
- Flag mismatches as gaps

### Dependency Direction
- Track direction: which item **depends on** which
- Identify circular dependencies for review
- External dependencies flow **outward** from module

### Technology Consolidation
Group similar technologies:
```
Item A: PostgreSQL 14
Item B: PostgreSQL 15
→ Module uses: PostgreSQL (14-15)
```

### Concern Prioritization
| Concern Type | Default Priority |
|--------------|------------------|
| Security boundary crossing | High |
| Single point of failure | High |
| Circular dependency | Medium |
| Timing-sensitive | Medium |
| Data consistency | Medium |

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 Wfz0iC
- [ ] Each item-to-item communication has interface documentation 🆔 JSXHnQ
- [ ] External interfaces to other modules are identified 🆔 TlhJfc
- [ ] Shared infrastructure components are documented 🆔 b2vu4U
- [ ] Data flows cover major business scenarios 🆔 0f54Qd
- [ ] Integration concerns are identified with mitigation status 🆔 iRBXS0
- [ ] Traceability links to system requirements exist 🆔 HUM8Py
- [ ] Integration test coverage is documented 🆔 6poMPM
- [ ] Gaps are prioritized for remediation 🆔 bklw8u
