# Health Manager - IEC 62304 Extracted Verification Evidence

This directory contains extracted verification evidence from the health-manager repository for IEC 62304 compliance.

## Files

### verification.json (1.2 MB)
Complete verification evidence in structured JSON format conforming to the IEC 62304 extraction schema.

**Contents**:
- Extraction metadata (commit, date, tools)
- Summary statistics (2,098 tests across 305 files)
- Unit verification data (1,691 unit tests, 253 software units)
- Integration verification data (383 integration tests)
- System verification data (24 system/E2E tests)
- Verification gaps analysis (253 gaps identified)

**Schema**: Follows the schema defined in `/home/jakob/Noncomplicity/Projects/QMS Master/Prompts/extraction/extraction-62304-verification.md`

### VERIFICATION_EXTRACTION_SUMMARY.md
Human-readable comprehensive summary and analysis report.

**Contents**:
- Repository information
- Test coverage statistics
- IEC 62304 compliance mapping (5.5, 5.6, 5.7)
- Acceptance criteria coverage analysis
- Verification gaps and recommendations
- CI/CD integration details
- Next steps

### risks.json (70 KB)
Previously extracted risk analysis data.

### soup-list.json (71 KB)
Previously extracted Software of Unknown Provenance (SOUP) list.

### use-requirements.json (36 KB)
Previously extracted use requirements.

## Quick Statistics

- **Total Tests**: 2,098 test methods
- **Unit Tests**: 1,691 (80.6%)
- **Integration Tests**: 383 (18.3%)
- **System Tests**: 24 (1.1%)
- **Software Units**: 253 units tested
- **Test Files**: 305 files analyzed

## IEC 62304 Compliance

| Clause | Requirement | Evidence |
|--------|-------------|----------|
| 5.5 | Software Unit Verification | 1,691 unit tests |
| 5.6 | Integration Testing | 383 integration tests |
| 5.7 | System Testing | 24 E2E system tests |

## Usage

### Viewing the JSON Data

```bash
# View summary
jq '.summary' verification.json

# List all unit tests
jq '.unit_verification.software_units_tested[].tests[]' verification.json

# View integration tests
jq '.integration_verification.integration_tests[]' verification.json

# View system tests
jq '.system_verification.system_tests[]' verification.json

# View verification gaps
jq '.verification_gaps[]' verification.json
```

### Python Analysis

```python
import json

with open('verification.json', 'r') as f:
    data = json.load(f)

# Get summary
print(data['summary'])

# Analyze unit tests
for unit in data['unit_verification']['software_units_tested']:
    print(f"{unit['unit_id']}: {len(unit['tests'])} tests")

# Check acceptance criteria coverage
for unit in data['unit_verification']['software_units_tested']:
    coverage = unit['acceptance_criteria_coverage']
    covered = sum(1 for v in coverage.values() if v)
    print(f"{unit['unit_id']}: {covered}/7 criteria covered")
```

## Extraction Details

- **Source**: /home/jakob/Noncomplicity/Repos/health-manager
- **Commit**: 16e0273739eda65f73af5410323e1caf53362f16
- **Date**: 2026-02-18 10:46:03 +0000
- **Extracted**: 2026-03-06T10:11:22.386776
- **Method**: Automated extraction using IEC 62304 verification extraction script
- **Standard**: IEC 62304:2006+AMD1:2015

## Acceptance Criteria Coverage

Coverage of IEC 62304 5.5.3/5.5.4 acceptance criteria across tested units:

| Criteria | Units Covered | Percentage |
|----------|---------------|------------|
| Fault Handling | 180/248 | 72.6% |
| Boundary Conditions | 98/248 | 39.5% |
| Functional | 95/248 | 38.3% |
| Data Flow | 49/248 | 19.8% |
| Initialization | 36/248 | 14.5% |
| Event Sequence | 25/248 | 10.1% |
| Memory Management | 0/248 | 0.0% |

## Key Findings

### Strengths
- Comprehensive test suite (2,098 tests)
- Strong unit test coverage (80.6% of tests)
- Good fault handling coverage (72.6% of units)
- Automated CI/CD integration
- Modern test frameworks (JUnit 5, Mockito)

### Gaps
- Limited memory management testing
- No explicit requirements traceability
- System test coverage could be expanded
- Code coverage metrics not available (need JaCoCo reports)
- Regression test suite not explicitly identified

## Recommendations

1. **Add Memory Management Tests**: No units currently test memory management criteria
2. **Establish Requirements Traceability**: Link tests to SRS requirements
3. **Expand System Tests**: Current 24 E2E tests should be increased
4. **Extract Test Results**: Include actual execution results from CI/CD
5. **Generate Coverage Reports**: Run JaCoCo to get line/branch coverage
6. **Document Test Strategy**: Create formal Software Verification Plan

## Related Documents

- Extraction prompt: `/home/jakob/Noncomplicity/Projects/QMS Master/Prompts/extraction/extraction-62304-verification.md`
- Architecture extraction: `handover_architecture.md` (in repo root)
- Risk analysis: `risks.json`
- SOUP list: `soup-list.json`

## Notes

This extraction is based on static code analysis. Actual test execution results, code coverage metrics, and requirements traceability would enhance the verification evidence for full IEC 62304 compliance.

For Class C software, additional documentation and traceability will be required per IEC 62304 clauses 5.5.5, 5.6.7, and 5.7.5.
