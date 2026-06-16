# Risk Management Report

## Document Information

| Field | Value |
|-------|-------|
| Product Name | Medical Device Software |
| Version | 1.0 |
| Manufacturer | P24 |
| Report Date | 2026-03-06 |
| Document Status | Released |
| Classification | Confidential |

---

## 1. Scope

This Risk Management Report documents the risk management activities performed for **Medical Device Software**
in accordance with ISO 14971:2019 "Medical devices — Application of risk management to medical devices".

This report provides evidence that:
- The risk management plan has been implemented
- The overall residual risk is acceptable
- Appropriate methods are in place for collecting post-production information

---

## 2. References

### 2.1 Standards and Regulations

| Standard | Title |
|----------|-------|
| ISO 14971:2019 | Medical devices — Application of risk management to medical devices |
| IEC 62304:2006+A1:2015 | Medical device software — Software life cycle processes |
| IEC 62366-1:2015 | Medical devices — Application of usability engineering to medical devices |
| ISO 13485:2016 | Medical devices — Quality management systems |

### 2.2 Related Documents

- Risk Management Plan
- Software Development Plan
- Usability Engineering File
- Clinical Evaluation Report

---

## 3. Risk Management Process Summary

### 3.1 Risk Analysis Sessions

| Date       | Summary                                                    | Participants                                                               |
| ---------- | ---------------------------------------------------------- | -------------------------------------------------------------------------- |
| 2026-02-15 | Initial risk analysis session for dosing calculator module | Dr. J. Smith (Clinical Lead), A. Johnson (SW Architect), M. Chen (QA Lead) |

### 3.2 Risk Management Activities

The following risk management activities were performed:

1. **Risk Analysis**: Systematic identification of hazards and hazardous situations
2. **Risk Evaluation**: Assessment of risks against acceptance criteria
3. **Risk Control**: Implementation of risk control measures
4. **Residual Risk Evaluation**: Assessment of remaining risks after controls
5. **Risk/Benefit Analysis**: Evaluation where residual risks require justification
6. **Review**: Verification that risk management activities are complete

---

## 4. System Overview

### 4.1 Software Modules

| Module ID | Name                     | Safety Class | Description                                           |
| --------- | ------------------------ | ------------ | ----------------------------------------------------- |
| MOD-006   | Vital Signs Monitor      | C            | Software module that collects, processes, and disp... |
| MOD-002   | Drug Interaction Checker | C            | Software module that identifies potential drug-dru... |
| MOD-004   | Alerting Service         | C            | Shared software module responsible for generating,... |
| MOD-003   | Clinical Protocol Engine | B            | Software module that manages evidence-based clinic... |
| MOD-001   | Dosing Calculator        | C            | Software module responsible for calculating medica... |
| MOD-005   | Patient Data Service     | B            | Shared software module that manages patient demogr... |
| MOD-007   | Trend Analysis Engine    | C            | Software module that analyzes historical vital sig... |

---

## 5. Hazard Identification

### 5.1 Identified Hazards

| Hazard ID | Description                                 | Category  | Associated Harm |
| --------- | ------------------------------------------- | --------- | --------------- |
| HAZ-002   | Patient identification mismatch             | software  | HARM-001        |
| HAZ-009   | Early warning score calculated incorrect... | software  | HARM-002        |
| HAZ-001   | Software calculates incorrect medication... | software  | HARM-001        |
| HAZ-008   | False alarm generated from signal artifa... | usability | HARM-007        |
| HAZ-005   | Critical clinical alert not delivered to... | software  | HARM-001        |
| HAZ-006   | Patient data displayed for wrong patient    | software  | HARM-001        |
| HAZ-010   | Deterioration trend not detected or aler... | software  | HARM-001        |
| HAZ-004   | Drug interaction not detected or alerted    | software  | HARM-001        |
| HAZ-007   | Vital signs display shows incorrect or s... | software  | HARM-001        |
| HAZ-003   | System unavailable during critical opera... | software  | HARM-003        |

### 5.2 Identified Harms

| Harm ID  | Description                                       | Severity       |
| -------- | ------------------------------------------------- | -------------- |
| HARM-005 | Cardiac arrest from delayed intervention          | 4 (Critical)   |
| HARM-003 | Treatment delay causing patient discomfort        | 2 (Marginal)   |
| HARM-006 | Unnecessary clinical intervention from false data | 2 (Marginal)   |
| HARM-007 | Death or serious injury from alarm fatigue        | 4 (Critical)   |
| HARM-002 | Serious injury requiring hospitalization          | 3 (Medium)     |
| HARM-004 | Prolonged hospitalization due to medication error | 2 (Marginal)   |
| HARM-008 | Workflow disruption and clinician distraction     | 1 (Negligible) |
| HARM-001 | Death due to medication overdose                  | 4 (Critical)   |

---

## 6. Risk Estimation and Evaluation

### 6.1 Risk Estimation Criteria

#### Probability Levels

| Level | Name | Description |
|-------|------|-------------|
| 5 | Frequent | Will occur frequently, expected during normal use |
| 4 | Probable | Will occur several times during product lifetime |
| 3 | Occasional | Likely to occur sometimes during product lifetime |
| 2 | Remote | Unlikely but possible over the product lifetime |
| 1 | Improbable | Negligible probability |

#### Severity Levels

| Level | Name | Description |
|-------|------|-------------|
| 4 | Critical | Death or permanent impairment |
| 3 | Medium | Serious injury requiring medical intervention |
| 2 | Marginal | Minor injury, temporary impairment |
| 1 | Negligible | Inconvenience or temporary discomfort |

#### Risk Acceptance Matrix

```
         Probability
         P1    P2    P3    P4    P5
    S4   [A]   [I]   [I]   [U]   [U]   Critical
S   S3   [A]   [A]   [I]   [I]   [U]   Medium
e   S2   [A]   [A]   [A]   [I]   [I]   Marginal
v   S1   [A]   [A]   [A]   [A]   [A]   Negligible

[A] = Acceptable    [I] = Investigate (ALARP)    [U] = Unacceptable
```

### 6.2 Risk Analysis Results

| Risk ID  | Source  | Sequence of Events                | S   | P   | RPN | Controls | RS  | RP  | RRPN | Level       | Acceptable |
| -------- | ------- | --------------------------------- | --- | --- | --- | -------- | --- | --- | ---- | ----------- | ---------- |
| RISK-014 | MOD-004 | Shared alerting service failur... | 4   | 1   | 4   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-008 | MOD-004 | Critical alert fails to delive... | 4   | 3   | 12  | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-003 | MOD-002 | Patient context switches durin... | 4   | 2   | 8   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-013 | MOD-007 | Deterioration trend missed lea... | 4   | 2   | 8   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-006 | MOD-001 | Renal function not assessed le... | 4   | 3   | 12  | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-010 | MOD-006 | Delayed vital signs display ca... | 4   | 2   | 8   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-011 | MOD-006 | Signal artifact causes false a... | 4   | 3   | 12  | None     | 4   | 2   | 8    | investigate | Yes        |
| RISK-004 | MOD-001 | Dosing calculator service cras... | 2   | 3   | 6   | None     | 2   | 1   | 2    | acceptable  | Yes        |
| RISK-007 | MOD-002 | Contraindicated drug pair not ... | 4   | 2   | 8   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-001 | MOD-001 | Calculation algorithm error pr... | 4   | 3   | 12  | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-015 | MOD-005 | Shared patient data service re... | 3   | 2   | 6   | None     | 3   | 1   | 3    | acceptable  | Yes        |
| RISK-012 | MOD-007 | Incorrect NEWS2 score causes i... | 3   | 2   | 6   | None     | 3   | 1   | 3    | acceptable  | Yes        |
| RISK-002 | MOD-001 | Database contains incorrect me... | 3   | 3   | 9   | None     | 3   | 2   | 6    | investigate | Yes        |
| RISK-009 | MOD-005 | Wrong patient data retrieved l... | 4   | 2   | 8   | None     | 4   | 1   | 4    | acceptable  | Yes        |
| RISK-005 | MOD-001 | Floating point precision error... | 3   | 2   | 6   | None     | 3   | 1   | 3    | acceptable  | Yes        |

### 6.3 Risk Summary Statistics

| Metric | Value |
|--------|-------|
| Total Risks Identified | 15 |
| Acceptable Risks | 15 |
| Unacceptable Risks | 0 |
| Average Initial RPN | 8.3 |
| Average Residual RPN | 4.1 |
| RPN Reduction | 51.2% |

#### Risk Level Distribution

| Level | Count | Percentage |
|-------|-------|------------|
| Acceptable | 13 | 86.7% |
| Investigate (ALARP) | 2 | 13.3% |
| Unacceptable | 0 | 0.0% |

---

## 7. Risk Control Measures

### 7.1 Implemented Controls

| Control ID | Type | Description | Verification | Mitigates |
|------------|------|-------------|--------------|----------|
| REQ-003 | design | Patient identity verification before dos... | TC-005 | N/A |
| REQ-002 | design | Input validation for all patient paramet... | TC-003, TC-004 | N/A |
| REQ-001 | design | Implement maximum dose limits with hard ... | TC-001, TC-002 | N/A |
| REQ-004 | design | High availability architecture with fail... | TC-006, TC-007 | N/A |

### 7.2 Control Effectiveness

| Metric | Value |
|--------|-------|
| Total Controls | 4 |
| Risks with Controls | 0 |
| Risks without Controls | 15 |
| Control Coverage | 0.0% |

---

## 8. Overall Residual Risk Evaluation

### 8.1 Residual Risk Assessment

All identified risks have been reduced to acceptable levels or have been justified
through benefit-risk analysis where the medical benefits outweigh the residual risks.


### 8.2 Benefit-Risk Analysis

The following risks are in the ALARP (As Low As Reasonably Practicable) region and require benefit-risk justification:

| Risk ID | Residual RPN | Benefit Outweighs Risk | Justification |
|---------|--------------|------------------------|---------------|
| RISK-011 | 8 | Yes | Ongoing alarm optimization efforts with ... |
| RISK-002 | 6 | Yes | ALARP - medication database undergoes qu... |

### 8.3 Overall Residual Risk Conclusion

**The overall residual risk is ACCEPTABLE.**

Based on the risk analysis performed:
- All identified hazards have been analyzed
- Risk control measures have been implemented and verified
- Residual risks have been evaluated against acceptance criteria
- Risks in the ALARP region have been justified through benefit-risk analysis
- The combination of residual risks has been considered and found acceptable

---

## 9. Production and Post-Production Information

### 9.1 Production Information Requirements

The following information shall be collected during production:
- Manufacturing deviations affecting safety
- Customer complaints related to safety
- Field safety corrective actions

### 9.2 Post-Production Surveillance

The following post-production activities are required:
- Monitoring of customer complaints
- Analysis of field failure data
- Review of similar device incidents
- Periodic review of risk management file

### 9.3 Review Triggers

Risk management activities shall be reviewed when:
- New hazards are identified
- Significant design changes occur
- New regulatory requirements apply
- Post-market data indicates increased risk

---

## 10. Conclusion

This Risk Management Report demonstrates that:

1. ✓ Risk management activities have been performed according to the Risk Management Plan
2. ✓ All foreseeable hazards have been identified and analyzed
3. ✓ Risk control measures have been implemented
4. ✓ All residual risks are acceptable or justified
5. ✓ Methods are in place for post-production monitoring

**Overall Assessment: ACCEPTABLE - Product may be released**

---

## 11. Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Manager | | | |
| Quality Assurance | | | |
| Regulatory Affairs | | | |
| Management Representative | | | |

---

## Appendix A: Risk Management File Contents

- [ ] Risk Management Plan 🆔 SqcDWF
- [ ] Risk Analysis Records 🆔 I9y3qv
- [ ] Risk Evaluation Records 🆔 um8xul
- [ ] Risk Control Records 🆔 8suqhw
- [ ] Residual Risk Evaluation 🆔 IGqv2j
- [ ] Risk Management Report (this document) 🆔 Eutb8M
- [ ] Production and Post-Production Information 🆔 rpRCH5

---

## Appendix B: Traceability Matrix

| Risk ID | Hazard | Harm | Controls | Verification |
|---------|--------|------|----------|-------------|
| RISK-014 | HAZ-005 | HARM-001 | None | N/A |
| RISK-008 | HAZ-005 | HARM-005 | None | N/A |
| RISK-003 | HAZ-002 | HARM-001 | None | N/A |
| RISK-013 | HAZ-010 | HARM-005 | None | N/A |
| RISK-006 | HAZ-001 | HARM-001 | None | N/A |
| RISK-010 | HAZ-007 | HARM-001 | None | N/A |
| RISK-011 | HAZ-008 | HARM-007 | None | N/A |
| RISK-004 | HAZ-003 | HARM-003 | None | N/A |
| RISK-007 | HAZ-004 | HARM-001 | None | N/A |
| RISK-001 | HAZ-001 | HARM-001 | None | N/A |
| RISK-015 | HAZ-006 | HARM-002 | None | N/A |
| RISK-012 | HAZ-009 | HARM-002 | None | N/A |
| RISK-002 | HAZ-001 | HARM-002 | None | N/A |
| RISK-009 | HAZ-006 | HARM-001 | None | N/A |
| RISK-005 | HAZ-001 | HARM-002 | None | N/A |

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-06 | Risk Management System | Initial release |

---

*This report was generated automatically by the Risk Management System.*

*Generated: 2026-03-06 08:38:30 UTC*
