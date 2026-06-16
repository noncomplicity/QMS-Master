---
id: 725af57
title: "generation 62304 release notes"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "5.8"
inputs: ["change-control.json", "verification.json", "software-risk.json"]
outputs: ["Software Release Notes document"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Software Release Notes

## Context

The Software Release Notes document the software release per IEC 62304 clause 5.8. This includes:
- **5.8.1** Verification completeness
- **5.8.2** Known residual anomalies
- **5.8.3** Residual anomaly evaluation (Class B, C)
- **5.8.4** Released version documentation
- **5.8.5** Release creation process (Class B, C)
- **5.8.6** Activities and tasks completeness (Class B, C)

## Inputs

### Required
- `change-control.json` — Release configuration and version info
- `verification.json` — Verification status and anomalies

### Optional
- `software-risk.json` — Safety assessment of anomalies
- `requirements.json` — Features implemented
- Previous release notes — For delta comparison

## Instructions

1. **Extract Version Information**
   - Release version number
   - Release date
   - Previous version (for upgrade path)

2. **Summarize Changes**
   - New features
   - Bug fixes
   - Security updates
   - Breaking changes

3. **Document Verification Status**
   - Verification complete confirmation
   - Test summary
   - Coverage metrics

4. **List Known Anomalies**
   - All residual anomalies
   - Safety impact assessment
   - Workarounds if any

5. **Document Release Process**
   - Build environment
   - Release procedure
   - Delivery mechanism

## Output Schema

```markdown
---
id: 725af57
title: "generation 62304 release notes"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Report"
document_id: "RN-[product]-[version]"
software_safety_class: "A | B | C"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Software Release Notes

**Product:** [Product Name]
**Version:** [X.Y.Z]
**Release Date:** [YYYY-MM-DD]
**Safety Class:** [A | B | C]

---

## 1. Release Overview

### 1.1 Release Summary

| Property | Value |
|----------|-------|
| Version | [X.Y.Z] |
| Previous Version | [X.Y.W] |
| Release Type | [Major / Minor / Patch / Hotfix] |
| Release Date | [YYYY-MM-DD] |
| Build Number | [build-id] |
| Commit | [commit-hash] |

### 1.2 Release Description
[Brief description of this release and its primary purpose]

### 1.3 Intended Use
[Statement of intended use for this software version]

### 1.4 Target Audience
- [User type 1]
- [User type 2]

## 2. What's New

### 2.1 New Features

| Feature | Description | Requirements |
|---------|-------------|--------------|
| [Feature Name] | [Description] | SRS-xxx |
| ... | ... | ... |

#### [Feature Name]
[Detailed description of the feature]

**User Impact:** [How users will experience this feature]

---

[Repeat for each new feature]

### 2.2 Enhancements

| Enhancement | Description | Impact |
|-------------|-------------|--------|
| [Enhancement] | [Description] | [User impact] |
| ... | ... | ... |

### 2.3 Bug Fixes

| Issue | Description | Severity | Resolution |
|-------|-------------|----------|------------|
| [PR-xxx] | [Bug description] | [Sev] | [How fixed] |
| ... | ... | ... | ... |

### 2.4 Security Updates

| Update | Description | CVE | Severity |
|--------|-------------|-----|----------|
| [Update] | [Description] | [CVE-ID] | [Severity] |
| ... | ... | ... | ... |

### 2.5 SOUP/Dependency Updates

| Component | Previous | New | Reason |
|-----------|----------|-----|--------|
| [package] | [old-ver] | [new-ver] | [why updated] |
| ... | ... | ... | ... |

## 3. Breaking Changes

### 3.1 API Changes
[List any breaking API changes]

| Change | Migration Path |
|--------|----------------|
| [Change description] | [How to migrate] |

### 3.2 Configuration Changes
[List any configuration changes required]

### 3.3 Data Migration
[Any data migration requirements]

### 3.4 Deprecations
[Features deprecated in this release]

| Deprecated | Replacement | Removal Version |
|------------|-------------|-----------------|
| [feature] | [replacement] | [version] |

## 4. Verification Status (5.8.1)

### 4.1 Verification Summary

| Verification Activity | Status | Reference |
|-----------------------|--------|-----------|
| Unit Testing | ✅ Complete | SVR Section 3 |
| Integration Testing | ✅ Complete | SVR Section 4 |
| System Testing | ✅ Complete | SVR Section 5 |
| Risk Control Verification | ✅ Complete | SRMF Section 5 |
| Requirements Verification | ✅ Complete | SVR Section 6 |

### 4.2 Test Results

| Metric | Value |
|--------|-------|
| Total Tests | [n] |
| Passed | [n] ([%]) |
| Failed | [0] |
| Code Coverage | [%] |
| Requirements Covered | [n]/[n] ([%]) |

### 4.3 Verification Statement
All software verification activities defined in the Software Development Plan have been completed and evaluated. Results are documented in the Software Verification Report [SVR-xxx].

## 5. Known Issues and Residual Anomalies (5.8.2, 5.8.3)

### 5.1 Known Issues Summary

| Severity | Count | Safety-Relevant |
|----------|-------|-----------------|
| Critical | [n] | [n] |
| Major | [n] | [n] |
| Minor | [n] | [n] |

### 5.2 Known Issue Details

#### ANO-xxx: [Issue Title]

| Property | Value |
|----------|-------|
| ID | ANO-xxx |
| Severity | [Severity] |
| Status | Known |
| Affected Functionality | [What's affected] |

**Description:**
[Description of the known issue]

**User Impact:**
[How this affects users]

**Workaround:**
[Workaround if available, or "None"]

**Safety Evaluation (5.8.3):**
[Assessment of whether this anomaly contributes to unacceptable risk]

**Resolution Plan:**
[Planned fix version or "Monitoring"]

---

[Repeat for each known issue]

### 5.3 Safety Assessment of Residual Anomalies

> *Applicable to Class B and C software (5.8.3)*

All known residual anomalies have been evaluated for safety impact:

| Anomaly | Safety Relevant | Assessment | Acceptable |
|---------|-----------------|------------|------------|
| ANO-xxx | [Yes/No] | [Assessment] | ✅ |
| ... | ... | ... | ... |

**Conclusion:** All known residual anomalies have been evaluated and do not contribute to unacceptable risk.

## 6. Release Configuration (5.8.4)

### 6.1 Software Version

| Component | Version | Hash |
|-----------|---------|------|
| [Product] | [X.Y.Z] | [commit] |

### 6.2 System Configuration

| Configuration Item | Version |
|--------------------|---------|
| [component] | [version] |
| [dependency] | [version] |
| ... | ... |

### 6.3 SOUP Configuration

| SOUP | Version | Source |
|------|---------|--------|
| [package] | [version] | [registry] |
| ... | ... | ... |

## 7. Release Creation (5.8.5)

> *Applicable to Class B and C software*

### 7.1 Build Environment

| Component | Version |
|-----------|---------|
| Operating System | [OS version] |
| Build Tool | [tool version] |
| Compiler/Runtime | [version] |
| CI/CD System | [system version] |

### 7.2 Build Procedure

1. [Build step 1]
2. [Build step 2]
3. [Build step 3]
4. [Artifact creation]
5. [Signing/packaging]

### 7.3 Build Artifacts

| Artifact | Type | Checksum (SHA256) |
|----------|------|-------------------|
| [artifact-name] | [type] | [checksum] |
| ... | ... | ... |

### 7.4 CI/CD Reference
- Build Job: [job-id/url]
- Build Date: [timestamp]
- Build Status: ✅ Successful

## 8. Development Completeness (5.8.6)

> *Applicable to Class B and C software*

### 8.1 Development Plan Activities

| Activity | Status | Reference |
|----------|--------|-----------|
| Requirements Definition | ✅ Complete | SRS v[x] |
| Architecture Design | ✅ Complete | SAD v[x] |
| Detailed Design | ✅ Complete | [ref] |
| Implementation | ✅ Complete | [commit] |
| Verification | ✅ Complete | SVR v[x] |
| Risk Management | ✅ Complete | SRMF v[x] |
| Documentation | ✅ Complete | [refs] |

### 8.2 Documentation Package

| Document | Version | Status |
|----------|---------|--------|
| Software Requirements Specification | [v] | ✅ Approved |
| Software Architecture Description | [v] | ✅ Approved |
| Software Verification Report | [v] | ✅ Approved |
| Software Risk Management File | [v] | ✅ Approved |
| SOUP List | [v] | ✅ Approved |
| User Documentation | [v] | ✅ Approved |

### 8.3 Completeness Statement
All activities and tasks defined in the Software Development Plan for this release have been completed, including associated documentation.

## 9. Installation and Upgrade

### 9.1 System Requirements

| Requirement | Specification |
|-------------|---------------|
| Operating System | [supported OS] |
| Memory | [RAM requirement] |
| Storage | [disk requirement] |
| Network | [network requirements] |
| Dependencies | [external dependencies] |

### 9.2 Installation Instructions
[Brief installation instructions or reference to Installation Guide]

### 9.3 Upgrade Path

| From Version | Upgrade Path | Notes |
|--------------|--------------|-------|
| [X.Y.W] | Direct upgrade | [notes] |
| [X.Y.V] | Requires [X.Y.W] first | [notes] |

### 9.4 Rollback Procedure
[How to rollback to previous version if needed]

## 10. Delivery and Distribution (5.8.8)

### 10.1 Distribution Method
[How software is delivered - download, installation media, etc.]

### 10.2 Integrity Verification
[How users can verify integrity - checksums, signatures]

| Artifact | SHA256 Checksum |
|----------|-----------------|
| [artifact] | [checksum] |

### 10.3 Access and Authentication
[How to access the release - credentials, permissions]

## 11. Support Information

### 11.1 Support Contact
[Support contact information]

### 11.2 Documentation
- User Guide: [link/reference]
- API Documentation: [link/reference]
- Known Issues: [link/reference]

### 11.3 Feedback
[How to report issues or provide feedback]

## 12. Regulatory Information

### 12.1 Regulatory Status
[Regulatory clearance/approval status if applicable]

### 12.2 UDI Information
[Unique Device Identifier if applicable]

### 12.3 Standards Compliance
| Standard | Compliance |
|----------|------------|
| IEC 62304:2006+A1:2015 | Compliant |
| ISO 14971:2019 | Compliant |
| [other standards] | [status] |

## 13. Release Approval

### 13.1 Release Checklist

- [ ] Verification complete (5.8.1) 🆔 kscz5C
- [ ] Residual anomalies documented (5.8.2) 🆔 d2XemI
- [ ] Residual anomalies evaluated for safety (5.8.3) [Class B, C] 🆔 BXrnwR
- [ ] Version documented (5.8.4) 🆔 DswkJ7
- [ ] Release process documented (5.8.5) [Class B, C] 🆔 jgC9YA
- [ ] All activities complete (5.8.6) [Class B, C] 🆔 9IEHd3
- [ ] Software archived (5.8.7) 🆔 eUPYdH
- [ ] Delivery mechanism verified (5.8.8) 🆔 FVzXaa

### 13.2 Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Software Lead | | | |
| Quality Assurance | | | |
| Release Manager | | | |
| Regulatory Affairs | | | |

---

## Appendix A: Change Log

[Detailed list of all changes from previous version]

## Appendix B: Extraction Metadata

- Repository: [repo]
- Commit: [hash]
- Generated: [timestamp]
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 4 | 5.8.1 | Verification completeness |
| Section 5 | 5.8.2 | Residual anomalies documentation |
| Section 5.3 | 5.8.3 | Residual anomaly safety evaluation |
| Section 6 | 5.8.4 | Released version documentation |
| Section 7 | 5.8.5 | Release creation documentation |
| Section 8 | 5.8.6 | Activities and tasks completeness |
| Section 8.2 | 5.8.7 | Archive reference (documentation) |
| Section 10 | 5.8.8 | Reliable delivery procedures |

## Validation Criteria

- [ ] Version number and date are documented 🆔 pZR9DL
- [ ] All changes since previous version are listed 🆔 oHIAYb
- [ ] Verification status is confirmed complete 🆔 Orb59u
- [ ] Known anomalies are documented with severity 🆔 U0y8AD
- [ ] Safety evaluation of anomalies is included (Class B/C) 🆔 NbXMhp
- [ ] Build environment and process are documented (Class B/C) 🆔 xG7U4o
- [ ] Development plan completeness is confirmed (Class B/C) 🆔 SZ9ebF
- [ ] Installation/upgrade instructions are provided 🆔 raCKUi
- [ ] Integrity verification method is documented 🆔 C4m89g
- [ ] Approval signatures section is present 🆔 t3xdMg
