# MRHB Account Management Processes

## Task: T1-A-013-1
**Status**: Pending  
**Priority**: High  
**Dependencies**: None  
**Effort**: 1 point

## Description
This document details the account management processes within the MRHB system that need to be integrated with Creditcoin.

## Account Management Overview

MRHB (Marhaba) Network provides Shariah-compliant financial services and requires robust account management processes to ensure compliance with both Islamic finance principles and regulatory requirements. These processes cover the entire lifecycle of user accounts from registration to closure.

## Account Management Processes

### 1. Account Registration and Verification

#### Process Flow
1. User submits registration information [REFERENCE NEEDED: MRHB registration documentation]
   - Basic personal information (name, email, phone number)
   - Authentication credentials creation
   - Device information capture
   - User agreement acceptance

2. Initial verification
   - Email verification via confirmation link
   - Phone verification via SMS code
   - Basic bot/fraud detection

3. Identity verification (KYC) [REFERENCE NEEDED: MRHB KYC provider and requirements]
   - Document submission (government ID, proof of address)
   - Biometric verification (selfie with ID, liveness check)
   - Document authenticity validation
   - Identity verification scoring

4. Shariah compliance assessment [REFERENCE NEEDED: MRHB Shariah compliance framework]
   - User intention declaration
   - Prohibited activities acknowledgment
   - Ethical investment preferences

5. Account approval
   - Compliance officer review (for high-risk cases)
   - Automated approval (for standard cases)
   - Account status activation
   - Welcome notification

#### Data Flows
- User → Registration API → User Database
- User → KYC Provider → Compliance Engine → User Database
- Compliance Engine → Shariah Advisory System → User Database
- User Database → Notification System → User

#### Business Rules
- All users must complete identity verification within 30 days of registration [REFERENCE NEEDED: MRHB verification timeframe policy]
- Different verification levels determine transaction limits and feature access
- Accounts with incomplete verification have restricted functionality
- Users from sanctioned countries or high-risk jurisdictions require enhanced due diligence
- All verification data must be encrypted in transit and at rest
- Biometric data must not be stored longer than necessary for verification

### 2. Account Maintenance and Updates

#### Process Flow
1. Profile management [REFERENCE NEEDED: MRHB profile management documentation]
   - Personal information updates
   - Contact information changes
   - Preference settings

2. Security management [REFERENCE NEEDED: MRHB security policies]
   - Password changes
   - Two-factor authentication setup/change
   - Session management
   - Security question management

3. Identity re-verification
   - Periodic KYC refresh (risk-based approach)
   - Document expiration management
   - Address change verification

4. Account recovery
   - Forgotten password recovery
   - Account lockout resolution
   - Multi-factor authentication reset

#### Data Flows
- User → Account Management API → User Database
- User → Security Module → User Database
- Monitoring System → User Database → Notification System → User
- User → Recovery System → Security Module → User Database

#### Business Rules
- Critical information changes require re-authentication
- Password updates must enforce strong password requirements [REFERENCE NEEDED: MRHB password policy]
- Contact information changes require verification of new details
- Security-critical changes must be notified to users via secondary channels
- Account recovery requires multi-step verification process
- Security events must be logged for audit purposes
- Periodic risk assessment determines re-verification frequency

### 3. Account Level Management

#### Process Flow
1. Account level assignment [REFERENCE NEEDED: MRHB account tier structure]
   - Initial level based on verification status
   - Level changes based on activity and compliance

2. Level upgrade process
   - Additional verification requirements
   - Activity threshold achievement
   - Compliance history review
   - Approval workflow

3. Level downgrade process
   - Compliance issue detection
   - Suspicious activity identification
   - Inactivity management
   - Notification and grace period

#### Data Flows
- User Database → Level Management System → User Database
- Compliance Engine → Level Management System → User Database
- Level Management System → Notification System → User
- Transaction Monitoring → Level Management System → User Database

#### Business Rules
- Account levels determine transaction limits and feature access [REFERENCE NEEDED: MRHB transaction limits per account level]
- Level upgrades require approval based on risk assessment
- Compliance violations result in automatic level downgrade
- Users must be notified of level changes with explanation
- Dormant accounts are automatically downgraded after defined period [REFERENCE NEEDED: MRHB account dormancy policy]
- Level history must be maintained for audit purposes

### 4. Account Suspension and Termination

#### Process Flow
1. Temporary suspension [REFERENCE NEEDED: MRHB account suspension criteria]
   - Suspicious activity detection
   - Compliance violation identification
   - User-requested suspension
   - Investigation process

2. Permanent termination
   - Severe compliance violations
   - Fraudulent activity confirmation
   - User-requested closure
   - Regulatory requirements

3. Fund handling [REFERENCE NEEDED: MRHB fund handling procedures]
   - Balance verification
   - Fund withdrawal process
   - Asset transfer procedures
   - Remaining balance management

4. Data retention
   - Data archiving process
   - Retention period enforcement [REFERENCE NEEDED: MRHB data retention policy]
   - Data anonymization
   - Deletion procedures

#### Data Flows
- Monitoring System → Compliance Engine → User Database
- User → Account Closure API → User Database
- User Database → Fund Management System → External Banking System
- User Database → Data Retention System → Secure Archive

#### Business Rules
- Temporary suspensions require documented justification
- Users must be notified of suspensions via multiple channels
- Terminations involving funds require compliance officer approval
- User-requested closures must verify identity before processing
- Regulatory-required data must be retained for the legally mandated period
- Non-essential PII must be anonymized after closure
- Account recovery option available for specific suspension cases

## Account Data Model

### User Profile [REFERENCE NEEDED: MRHB data schema documentation]
```json
{
  "accountId": "ACC-12345678",
  "status": "ACTIVE", // PENDING, ACTIVE, SUSPENDED, CLOSED
  "creationDate": "2025-01-15T14:30:00Z",
  "lastUpdated": "2025-04-10T09:45:22Z",
  "personalInfo": {
    "firstName": "string",
    "lastName": "string",
    "dateOfBirth": "date",
    "email": "string",
    "phone": "string",
    "address": {
      "street": "string",
      "city": "string",
      "state": "string",
      "postalCode": "string",
      "country": "string"
    }
  },
  "verificationStatus": {
    "emailVerified": true,
    "phoneVerified": true,
    "identityVerified": true,
    "addressVerified": true,
    "enhancedDueDiligence": false,
    "lastVerificationDate": "2025-01-20T11:15:32Z"
  },
  "accountLevel": {
    "currentLevel": 2, // 0-3 [REFERENCE NEEDED: MRHB account level definitions]
    "levelHistory": [
      {
        "level": 1,
        "effectiveDate": "2025-01-16T10:20:15Z",
        "reason": "Initial verification complete"
      },
      {
        "level": 2,
        "effectiveDate": "2025-02-28T14:22:10Z",
        "reason": "Enhanced verification complete"
      }
    ]
  },
  "complianceInfo": {
    "riskScore": 15, // 0-100
    "lastScreeningDate": "2025-04-01T08:30:45Z",
    "pep": false,
    "sanctionsHit": false,
    "highRiskCountry": false
  },
  "securitySettings": {
    "twoFactorEnabled": true,
    "twoFactorMethod": "APP", // SMS, EMAIL, APP
    "lastPasswordChange": "2025-03-10T16:45:22Z",
    "sessionTimeout": 1800 // seconds
  },
  "balances": [
    {
      "currency": "USD",
      "amount": 1500.50
    },
    {
      "currency": "ETH",
      "amount": 2.5
    }
  ],
  "shariahCompliance": {
    "acceptedTerms": true,
    "acceptanceDate": "2025-01-15T14:35:22Z",
    "investmentPreferences": {
      "ethical": true,
      "sustainable": true,
      "socialImpact": true
    }
  }
}
```

### Verification Data [REFERENCE NEEDED: MRHB verification data schema]
```json
{
  "accountId": "ACC-12345678",
  "documents": [
    {
      "type": "PASSPORT",
      "status": "VERIFIED",
      "submissionDate": "2025-01-16T09:20:15Z",
      "verificationDate": "2025-01-17T11:30:22Z",
      "expiryDate": "2030-05-20",
      "documentId": "DOC-87654321"
    },
    {
      "type": "PROOF_OF_ADDRESS",
      "status": "VERIFIED",
      "submissionDate": "2025-01-16T09:25:30Z",
      "verificationDate": "2025-01-17T11:35:10Z",
      "expiryDate": "2025-07-16",
      "documentId": "DOC-87654322"
    }
  ],
  "verificationResults": {
    "identityScore": 92, // 0-100
    "addressScore": 95, // 0-100
    "fraudRiskScore": 5, // 0-100
    "manualReviewRequired": false,
    "verificationProvider": "VerifyPlus",
    "verificationId": "VER-12345678"
  },
  "biometricData": {
    "faceMatchScore": 98, // 0-100
    "livenessScore": 99, // 0-100
    "lastBiometricVerification": "2025-01-17T11:28:35Z"
  },
  "auditTrail": [
    {
      "action": "EMAIL_VERIFICATION",
      "timestamp": "2025-01-15T14:40:22Z",
      "result": "SUCCESS",
      "details": "Email verified on first attempt"
    },
    {
      "action": "PHONE_VERIFICATION",
      "timestamp": "2025-01-15T14:45:10Z",
      "result": "SUCCESS",
      "details": "Phone verified on first attempt"
    },
    {
      "action": "DOCUMENT_SUBMISSION",
      "timestamp": "2025-01-16T09:20:15Z",
      "result": "SUCCESS",
      "details": "Passport and proof of address submitted"
    },
    {
      "action": "DOCUMENT_VERIFICATION",
      "timestamp": "2025-01-17T11:30:22Z",
      "result": "SUCCESS",
      "details": "All documents verified"
    }
  ]
}
```

## Integration Points

### Input Points [REFERENCE NEEDED: MRHB API documentation]
- User registration API
- KYC provider integration
- Document verification service
- Biometric verification service
- Shariah compliance assessment service
- AML/CFT screening service

### Output Points
- Account status notifications
- Account level change notifications
- Security event notifications
- Verification status updates
- Compliance alert notifications

### Validation Rules
- User information must be validated against required formats
- Identity documents must be checked for authenticity
- Biometric data must meet minimum quality standards
- Address verification must confirm physical existence
- Compliance checks must be performed against current regulations
- Shariah compliance assessment must follow approved principles [REFERENCE NEEDED: MRHB Shariah compliance principles]
- All validation results must be recorded for audit purposes

## Compliance Management Integration

### Shariah Compliance [REFERENCE NEEDED: MRHB Shariah compliance documentation]
- All account processes must adhere to Islamic principles
- Verification of user intention to participate in halal activities
- Account terms must be clear and transparent (avoiding Gharar)
- Investment screening to exclude prohibited industries [REFERENCE NEEDED: MRHB prohibited industry list]
- Profit distribution instead of interest calculations
- Regular audits by Shariah Advisory Board

### Regulatory Compliance [REFERENCE NEEDED: MRHB regulatory compliance framework]
- KYC/AML processes following local and international regulations
- Risk-based approach to customer due diligence
- Enhanced due diligence for high-risk customers
- Screening against global sanctions lists
- Politically Exposed Person (PEP) identification
- Suspicious activity monitoring and reporting
- Regular compliance audits and updates

## Notes
- All account management processes must be approved by both Shariah and regulatory compliance teams
- Integration with Creditcoin must maintain Shariah compliance throughout
- Data privacy regulations must be followed for all personal information
- Account processes must be regularly audited for compliance
- Changes to account management processes require approval workflow 