**

# Validation object and scope

Triage24 is defined in detail in [Triage24 Product Description.](https://doktor24.centuri.se/RegNo/D095) The software system is detailed in the [software architecture documentation](http://catalog.dev.platform24.se/docs). 

## Intended use

Triage24 is a configurable IT-system that is used by Healthcare providers to implement medical practice in the triage of patients upon first point of contact. 

  

The device is intended to be configured and adopted to the conditions and needs of a specific healthcare provider. This is achieved either through a service provided by Platform24 or its affiliates or by the healthcare provider. The configuration of the system is made through a web user interface.

  

The configured implementation of the device allows patients to initiate contact, through an associated application (Patient app), with healthcare providers by answering questions about their medical complaint, medical history, and health status. 

  

Based on their answers, the patient is directed to an appropriate level of care, as determined and configured by the health care provider. Such levels are for example: emergency care, physical appointment, digital appointment or self-care. That care can be provided to the patients through a physical meeting or digital meeting with a healthcare provider. Digital appointments can be facilitated through an associated clinician application (Clinic24) and a patient app (white label). 

. 

  
  

# Verification and validation strategy and plan

Verification and validation is implemented to provide objective evidence of the suitability to use T24 for its intended use. 

  

## Validation

The benefit of T24 is validated through clinical evaluations, risk management, summative usability evaluations and SW testing. Together, these activities form a complete picture of the safety, performance and usability of the device. These activities are planned as part of the lifecycle management of the device and are implemented at defined stages according to this plan.

  

## Verification

Verifications are performed for different purposes in different stages of the design and development of T24. In general the verifications are performed to ensure compliance to requirements. Compliance is verified for the following requirements:

1. Regulatory requirements as defined in MDR 2017/745 Annex I GSPR
    
2. Use requirements as defined in 82304-1:2017
    
3. System requirements as defined in 82304-1:2017 including SW requirements as defined in 62304:2006.
    

# Clinical Evaluation

Clinical evaluation is a product life-cycle process that uses intended use and main functions as basis to draw up objective evidence of the benefits of using T24. The clinical evaluation shall provide evidence of the fulfillment of the General Safety and Performance requirements as assessed in the [GSPR Checklist](https://doktor24.centuri.se:443/RegNo/D144). It shall also consider the need of clinical evidence connected to the hazardous situations as defined in the [Triage24 Risk management report](https://doktor24.centuri.se:443/RegNo/D152).

  

Clinical evaluation has been performed according to the Clinical Evaluation plan and is reported in the [Clinical Evaluation Report](https://doktor24.centuri.se:443/RegNo/D125).

# Safety and risk acceptance

The safety of T24 is validated through the risk assessment, risk control, residual risk and overall risk acceptability as defined in the Risk [management process](https://doktor24.centuri.se:443/RegNo/D073). A risk management report that concludes the overall residual risk and risk acceptability is mandatory for the validation of the device.

  

The [Triage24 Risk management report](https://doktor24.centuri.se:443/RegNo/D152) is maintained in line with the lifecycle of the device.

# Software testing

Software testing is performed on changes to configuration items to verify implementation of requirements, safety and proper operation. The gitflow process is applied to put software items under configuration control and defines the stages of the different testing activities. A CI/CD pipeline is used to implement the automated testing. Further details on the CI/CD pipeline can be found in the Ref X CI/CD pipeline documentation.

  

Testing is divided into the following activities:

1. Manual Peer reviews
    
2. Manual feature testing
    
3. Manual Pre release system testing
    
4. Automated Static code analysis
    
5. Automated Unit testing
    
6. Automated Integration testing
    
7. Automated System testing
    

  

Each category of testing has its own detailed plan and reporting defined below.

## Manual Peer reviews

The following peer-reviews has been conducted to verify the correct implementation of the following software components:

- Interviewer4 peer reviews: [https://gitlab.com/doktor24/services/interviewer4/-/merge_requests?scope=all&state=merged](https://gitlab.com/doktor24/services/interviewer4/-/merge_requests?scope=all&state=merged)
    
- Patient-UI peer reviews:  
    [https://gitlab.com/doktor24/frontends/patient-ui/-/merge_requests?scope=all&state=merged&author_username=ola.rybacka](https://gitlab.com/doktor24/frontends/patient-ui/-/merge_requests?scope=all&state=merged&author_username=ola.rybacka)  
    [https://gitlab.com/doktor24/frontends/patient-ui/-/merge_requests?scope=all&state=merged&author_username=marcin.modestowicz](https://gitlab.com/doktor24/frontends/patient-ui/-/merge_requests?scope=all&state=merged&author_username=marcin.modestowicz) 
    
- Code24-commons peer reviews:  
    [https://gitlab.com/doktor24/code24/code24-common/-/merge_requests?scope=all&state=merged](https://gitlab.com/doktor24/code24/code24-common/-/merge_requests?scope=all&state=merged) 
    
- Actions peer reviews:  
    [https://gitlab.com/doktor24/services/actions/-/merge_requests?scope=all&state=merged](https://gitlab.com/doktor24/services/actions/-/merge_requests?scope=all&state=merged) 
    

  

All peer-reviews has successfully verified that the sw item conform to the following criteria prior to push to production::

- The software item is identifiable and included in the Configuration management documentation.
    
- SW requirements are documented and traceable to system requirements.
    
- Risk analysis has been performed and documented in the Risk analysis record.
    
- Architectural dependencies have been defined as appropriate in the Software architecture documentation..
    
- Traceability to requirements and tests has been established.
    
- The appropriate automated tests have been implemented according to this plan.
    

- All software requirements have been tested or otherwise verified;
    
- The traceability between software requirements and tests are established;
    
- Test results meet the required pass/fail criteria.
    

Conclusion:

Manual peer reviews has been conducted according to the T24 V&V plan.

  

## Manual feature testing

Manual feature testing is usually part of the software testing process and is performed prior to a release. The aim of the manual feature testing is to make sure that software works properly to meet all the intended specifications. The QA’s use and reviews the developed feature the way an end user would use it so testing the developed feature is extremely important and has to be tested thoroughly in every stage of the development. The testing process is a systematic process so it requires the testers to design test cases exclusively to test the newly developed feature.

  

## Manual system testing

Prior to the release of a changed software component, it undergoes manual system testing. All  of the pre release system testing is carried out in the staging environment and the focus of this type of tests is to first test the changes introduced in the software component against their actual requirements in a production like environment. The second focus is to carry out manual regression testing of parts of the system that may have been affected by the introduced changes. 

  

### Manual system testing execution and output

The following system tests have been conducted to verify correct functionality of the Triage24 software system:

- All the  system test records are available in testrail. For each release a “Release XX Triage test summary.pdf” and a “Release XX detailed test cases info.xlsx” are generated and uploaded to centuri as a bundle (zip file) to the folder “Verification & Validation Testrail Reports”  
    The latest release report bundle is [Release 33](https://doktor24.centuri.se:443/RegNo/D158)
    

The system test verifies the following functionality:

- System and software requirements and the mapping to the test cases associated with them can be found in the [Product requirements for Triage24](https://doktor24.centuri.se:443/RegNo/D149) 
    

  

### Testrail

[Testrail] is for manual tests.

If a [Test Case] has failed an assessment should be included in the report. The assessment can be for example that the [Test Case] is not relevant, as that  part of the application will not be deployed anyways.

  

To create the report

1. Log into Testrail ([https://d24.testrail.io](https://d24.testrail.io))
    
2. Click “Test Runs & Results”
    
3. Select [Test Plan]
    
4. Select [Test Run]
    
5. Click “Print Run”
    
6. Select “Details”
    
7. Click Print
    
8. Select Save as PDF
    
9. Upload to this folder:
    

[https://drive.google.com/drive/folders/1mlrwN5zMyLrcYf2V1D993CBEUpW_ygLe?usp=sharing](https://drive.google.com/drive/folders/1mlrwN5zMyLrcYf2V1D993CBEUpW_ygLe?usp=sharing) 

  

Conclusion:

Manual system testing has been conducted according to the T24 V&V plan.

  

## Automated static code analysis

The following static analysis has been conducted to verify the technical soundness of the following software components:

- Interviewer4 static analysis: [https://gitlab.com/doktor24/services/interviewer4/-/jobs/1490625140](https://gitlab.com/doktor24/services/interviewer4/-/jobs/1490625140) 
    
- Patient-UI static analysis:  
    [https://gitlab.com/doktor24/frontends/patient-ui/-/jobs/1506758057](https://gitlab.com/doktor24/frontends/patient-ui/-/jobs/1506758057) 
    
- Code24-commons static analysis:  
    [https://gitlab.com/doktor24/code24/code24-common/-/jobs/1452606841](https://gitlab.com/doktor24/code24/code24-common/-/jobs/1452606841) 
    
- Actions static analysis:  
    [https://gitlab.com/doktor24/services/actions/-/jobs/1505865759](https://gitlab.com/doktor24/services/actions/-/jobs/1505865759)  
    [https://gitlab.com/doktor24/services/actions/-/jobs/1505808757](https://gitlab.com/doktor24/services/actions/-/jobs/1505808757) 
    

  

Conclusion:  
All static analysis were successful and thus no obvious technical issues could be identified in any of the product's software components.

  

## Automated unit testing

The following automated unit tests have been conducted to verify correct operation of the software units in Triage24:

- Interviewer4 unit and integration test report: [https://gitlab.com/doktor24/services/interviewer4/-/pipelines/350637075/test_report](https://gitlab.com/doktor24/services/interviewer4/-/pipelines/350637075/test_report) 
    
- Code24-commons unit test results:  
    [https://gitlab.com/doktor24/code24/code24-common/-/jobs/1452606841](https://gitlab.com/doktor24/code24/code24-common/-/jobs/1452606841) 
    
- Actions unit and integration test report:  
    [https://gitlab.com/doktor24/services/actions/-/pipelines/353715317/test_report](https://gitlab.com/doktor24/services/actions/-/pipelines/353715317/test_report) 
    

  
  

Conclusion:

Automated unit tests have been conducted according to the T24 V&V plan.

  

## Automated integration testing

The following automated integration tests have been conducted to verify correct interaction between software units in the software components of Triage24:

- Interviewer4 unit and integration test report: [https://gitlab.com/doktor24/services/interviewer4/-/pipelines/350637075/test_report](https://gitlab.com/doktor24/services/interviewer4/-/pipelines/350637075/test_report) 
    
- Interviewer4 unit and integration test report:  
    [https://gitlab.com/doktor24/services/actions/-/pipelines/353715317/test_report](https://gitlab.com/doktor24/services/actions/-/pipelines/353715317/test_report) 
    

  

Conclusion:

Automated integration tests have been conducted according to the T24 V&V plan.

  

## Automated system testing

The following automated system tests have been conducted to verify correct operation of the microservices in Triage24:

- Reference to automated system test records in GITlab. [https://gitlab.com/doktor24/platform/load-test-suite/-/jobs/1491014001](https://gitlab.com/doktor24/platform/load-test-suite/-/jobs/1491014001)
    

The automated system tests verifies proper operation of the following software systems:

- Patient-UI
    
- Interviewer
    
- Actions
    
- Object-Storage
    
- Directory
    
- Healthmanager
    

  

Conclusion:

Automated system tests have been conducted according to the T24 V&V plan.

  

## Software problem resolution

Software problem resolution has been implemented to correct any anomalies identified during testing. The problem resolution is carried out by the person responsible for the software item being developed. 

  

Logs and records from the failed tests are reviewed to identify the root cause of an anomaly. 

The software item is corrected and a new commit is pushed to the CI/CD pipeline.

All problem resolution is traceable to the software items in GITLab.

# Information security

The following tests have been conducted to verify proper information security in Triage24:

- Internal audit of the systems security conducted by senior developers and architects
    
- Two external penetration tests verifying the ability of the system to withstand intrusion.
    

  

Conclusion:

Information security risk control has been verified according to the T24 V&V plan. No severe vulnerabilities were found in the system. Due to the sensitivity of application security reports they are not shared here. Upon request the Platform24 security officer can go through their result and conclusion  with the regulatory body.

  

# Accompanying information

The user manual of Triage24 is updated with the user interface. On each update, the content of the user manual is reviewed by the product manager. 

  

# Usability

The following summative usability evaluations have been conducted to verify the usability of Triage24: 

- [Triage24 Usability testing report  - Patient Application](https://doktor24.centuri.se:443/RegNo/D116)
    
- [Content24 Usability testing report  - Medical Admin Application](https://doktor24.centuri.se:443/RegNo/D145)
    
- The UX development team stores testing prototypes, User Validation and Test records in [Maze](https://app.maze.co/login/).
    

  

Conclusion:

Usability evaluation has been conducted according to the T24 V&V plan.  
  

# Regulatory compliance

Regulatory compliance is verified to ensure that the device complies to applicable legal requirements. Applicable regulatory requirements are defined in the [Regulatory strategy](https://doktor24.centuri.se:443/RegNo/D123). Fulfillment of the MDR 2017/745 requirements is reviewed.

  

The review is documented in the [GSPR checklist for Triage24.](https://doktor24.centuri.se:443/RegNo/D144)

**