---
id:
title: "Extract Software Item Requirements"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304"
clause: "5.2"
inputs: ["source code", "type definitions", "API specs", "config files"]
outputs: ["item-requirements.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Software Item Requirements

## Context

IEC 62304 clause 5.2 requires documentation of software requirements. In a code-as-truth paradigm, requirements are extracted from implemented code, ensuring documentation reflects actual capabilities.

**Item-level focus:** This prompt extracts requirements **implemented in a single repository**. These requirements trace UP to module/system requirements and DOWN to implementation.

This output feeds into:
- Module-level requirement aggregation
- System-level SRS generation
- Traceability matrices

## Inputs

### Required
- **Source code** — Implementation files
- **Type definitions** — Interfaces, DTOs, domain models
- **API specifications** — OpenAPI, GraphQL schemas, or code-defined endpoints

### Optional (enriches output)
- **Configuration files** — Application settings showing configurable behavior
- **Test files** — Test cases revealing expected behavior
- **Existing documentation** — README, architecture docs

### Hierarchy Context
- **item_id** — Identifier for this software item
- **parent_module** — Module this item belongs to (if known)
- **parent_system** — System this item belongs to (if known)

## Instructions

### 1. Extract Functional Requirements (Clause 5.2.2 a)

Identify what the software does:
- API endpoints and their behavior
- Business logic in service classes
- Data transformations
- State machines and workflows
- Event handlers

For each functional requirement:
- Describe the capability
- Reference the implementing code
- Identify inputs and outputs
- Note any preconditions/postconditions

### 2. Extract Interface Requirements (Clause 5.2.2 b, c)

Identify external interfaces:
- REST/GraphQL/SOAP APIs (provided)
- Database schemas
- Message queues (JMS, Kafka, etc.)
- External service clients (consumed)
- File formats

For each interface:
- Document the contract
- Identify protocol and format
- Note error handling
- Reference implementation

### 3. Extract Performance Requirements (Clause 5.2.2)

Identify performance characteristics:
- Timeouts and SLAs in code
- Connection pool sizes
- Cache configurations
- Rate limiting
- Batch sizes

### 4. Extract Security Requirements (Clause 5.2.2 e)

Identify security measures:
- Authentication mechanisms
- Authorization rules
- Input validation
- Encryption usage
- Audit logging

### 5. Extract Data Requirements (Clause 5.2.2 g)

Identify data handling:
- Database schemas
- Data validation rules
- Retention policies
- Privacy considerations (GDPR)

### 6. Identify Traceability Links

For each requirement, identify:
- **Upward trace**: System/module requirement it implements (if known)
- **Downward trace**: Source files implementing it
- **Verification trace**: Tests verifying it

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

  "item_requirements": {
    "item_id": "<software-item-id>",
    "summary": {
      "total_requirements": "<count>",
      "by_type": {
        "functional": "<count>",
        "interface": "<count>",
        "performance": "<count>",
        "security": "<count>",
        "data": "<count>"
      }
    },

    "functional": [
      {
        "id": "ITEM-<item>-REQ-FUNC-<seq>",
        "description": "<what the software does>",
        "rationale": "<why this capability exists>",
        "inputs": ["<input descriptions>"],
        "outputs": ["<output descriptions>"],
        "preconditions": ["<conditions that must be true>"],
        "postconditions": ["<conditions true after execution>"],
        "verification_method": "test | inspection | analysis",
        "traces_to_parent": ["<parent requirement IDs if known>"],
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "interface": [
      {
        "id": "ITEM-<item>-REQ-INT-<seq>",
        "interface_name": "<API/endpoint name>",
        "direction": "provided | consumed",
        "protocol": "REST | JMS | JDBC | etc",
        "description": "<what the interface does>",
        "contract": {
          "endpoint": "<URL pattern or queue name>",
          "method": "<HTTP method if applicable>",
          "request_schema": "<schema reference or inline>",
          "response_schema": "<schema reference or inline>",
          "error_codes": ["<error code>: <meaning>"]
        },
        "verification_method": "test | inspection | analysis",
        "traces_to_parent": ["<parent requirement IDs if known>"],
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "performance": [
      {
        "id": "ITEM-<item>-REQ-PERF-<seq>",
        "description": "<performance characteristic>",
        "metric": "<what is measured>",
        "target": "<target value>",
        "configured_at": "<where this is configured>",
        "verification_method": "test | analysis",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "security": [
      {
        "id": "ITEM-<item>-REQ-SEC-<seq>",
        "description": "<security measure>",
        "security_category": "authentication | authorization | confidentiality | integrity | audit",
        "implementation": "<how implemented>",
        "verification_method": "test | inspection | analysis",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "data": [
      {
        "id": "ITEM-<item>-REQ-DATA-<seq>",
        "description": "<data requirement>",
        "data_entity": "<entity name>",
        "validation_rules": ["<validation rule>"],
        "storage": "<how/where stored>",
        "retention": "<retention policy if known>",
        "privacy_impact": "none | low | medium | high",
        "verification_method": "test | inspection",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ]
  },

  "traceability": {
    "to_parent": [
      {
        "item_requirement_id": "ITEM-<item>-REQ-*",
        "parent_requirement_ids": ["<parent IDs>"],
        "trace_type": "implements | partially_implements | derived_from"
      }
    ],
    "to_verification": [
      {
        "requirement_id": "ITEM-<item>-REQ-*",
        "test_ids": ["ITEM-<item>-TC-*"],
        "verification_status": "verified | partial | unverified"
      }
    ]
  },

  "gaps": [
    {
      "id": "ITEM-<item>-GAP-<seq>",
      "type": "unclear_requirement | untested | undocumented | missing_parent_trace",
      "description": "<what's missing>",
      "affected_requirements": ["<requirement IDs>"],
      "recommendation": "<suggested action>",
      "priority": "high | medium | low"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `functional[]` | 5.2.2 a) | Functional and capability requirements |
| `interface[]` | 5.2.2 b), c) | Input/output and interface requirements |
| `performance[]` | 5.2.2 | Performance requirements |
| `security[]` | 5.2.2 e) | Security requirements |
| `data[]` | 5.2.2 g) | Data definition requirements |
| `traceability.to_parent` | 5.1.1 | Traceability to system requirements |
| `traceability.to_verification` | 5.2.6 | Traceability to verification |
| `gaps[]` | 5.2.6 | Requirements analysis completeness |

## Validation Criteria

- [ ] All significant code areas have corresponding requirements 🆔 DgBbbM
- [ ] Each requirement has source file references 🆔 JAejxm
- [ ] Interface requirements include contract details 🆔 hDh5Ef
- [ ] Performance requirements have measurable targets 🆔 syL8yy
- [ ] Security requirements cover authentication and authorization 🆔 ONsf9H
- [ ] Data requirements address privacy where applicable 🆔 3sBXbi
- [ ] Verification methods are appropriate for each type 🆔 07wkWe
- [ ] Gaps are identified for unclear or untested requirements 🆔 98lnLK
- [ ] Hierarchy metadata is correctly populated 🆔 te5KJ1
