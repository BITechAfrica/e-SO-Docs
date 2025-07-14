# Email Messaging Examples

This document provides examples of where and how emails are sent in the service layer of the ePMO backend. For each case, you will find:

1. **The file sending the email**
2. **The function that is sending the email**
3. **Email Example** (subject, recipient, and body template)

---

## 1. `EmailService.java`

### Function: `sendStyledEmail(String to, String subject, String name, String body)`
**Example Usage:**
- Used by many services to send HTML emails. The body can be a full HTML document or a message wrapped in a default HTML template.


**End-User Example:**

Subject: Welcome to e-SO Platform

Recipient: victor.mahluza@email.com

Body:
Hi Victor Mahluza,

Welcome to the e-SO platform! Your account has been created successfully. Please use the link below to activate your account:

https://epmo.example.com/activate?token=abc123

If you did not request this account, please ignore this email.

Thank you,
The e-SO Team

---

## 2. `SponsorNotificationService.java`


### Function: `sendBudgetVarianceAlertEmail(Schedule schedule, double budgetVariance, String sponsorEmail)`
**End-User Example:**

Subject: CRITICAL: Budget Variance Alert - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

This is to notify you that your project, Real-time Data-Driven Decision Support System, is currently over budget by 12.5%, which exceeds the allowed threshold of 10%.

Status: BUDGET OVERRUN
Threshold: 10%
Current Variance: 12.5% (Over Budget)

Immediate review and corrective action are required. Please coordinate with your project team and finance department as needed.

Best regards,
e-SO Team


### Function: `sendQualityScoreAlertEmail(Schedule schedule, double qualityScore, String sponsorEmail)`
**End-User Example:**

Subject: Quality Score Alert - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

The quality score for your project, Real-time Data-Driven Decision Support System, has dropped to 78.0%.

Required Threshold: 85.0%
Status: BELOW THRESHOLD

Quality improvement actions are required to meet project standards. Please coordinate with your project team to implement quality assurance measures.

Best regards,
e-SO Team


### Function: `sendProductivityDropAlertEmail(Schedule schedule, double productivityDrop, String sponsorEmail)`
**End-User Example:**

Subject: Productivity Drop Alert - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

Project productivity for Real-time Data-Driven Decision Support System has dropped by 15.0%, exceeding the allowed threshold of 10%.

Immediate action is required. Please review resource allocation and identify bottlenecks affecting project performance.

Best regards,
e-SO Team


### Function: `sendSlipRateAlertEmail(Schedule schedule, double slipRate, String sponsorEmail)`
**End-User Example:**

Subject: Schedule Slip Alert - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

Schedule delay has been detected for your project, Real-time Data-Driven Decision Support System. The current slip rate is 8.0%.

Schedule recovery actions are required to bring the project back on track. Please review the project timeline and implement corrective measures.

Best regards,
e-SO Team


### Function: `sendHighRiskAlertEmail(Schedule schedule, String riskDescription, String sponsorEmail)`
**End-User Example:**

Subject: HIGH RISK ALERT - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

A high risk has been registered for your project, Real-time Data-Driven Decision Support System.

Risk Description: Vendor contract delays may impact the project timeline.

Immediate risk mitigation actions are required. Please coordinate with your project team to address this risk promptly.

Best regards,
e-SO Team


### Function: `sendProjectManagerChangeEmail(Schedule schedule, String oldManager, String newManager, String sponsorEmail)`
**End-User Example:**

Subject: Project Manager Change - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

Please be informed that the Project Manager for Real-time Data-Driven Decision Support System has changed.

Previous Manager: Sarah Ndlovu
New Manager: Victor Mahluza

Please ensure a smooth transition and update all relevant stakeholders about this change.

Best regards,
e-SO Team


### Function: `sendContractorChangeEmail(Schedule schedule, String oldContractor, String newContractor, String sponsorEmail)`
**End-User Example:**

Subject: Contractor Change - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

The main contractor for your project, Real-time Data-Driven Decision Support System, has changed.

Previous Contractor: BuildTech Ltd.
New Contractor: FutureConstruct Inc.

Please review contracts and ensure proper transition procedures are followed.

Best regards,
e-SO Team


### Function: `sendCriticalPathChangeEmail(Schedule schedule, String oldPath, String newPath, String sponsorEmail)`
**End-User Example:**

Subject: Critical Path Change - Real-time Data-Driven Decision Support System

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

The critical path for your project, Real-time Data-Driven Decision Support System, has changed.

Previous Critical Path: Data Integration
New Critical Path: User Acceptance Testing

Please review the schedule impact and assess any changes to project timeline and dependencies.

Best regards,
e-SO Team

---

## 3. `OtpService.java`


### Function: `sendStyledEmail(email, "OTP for Account Request", "Potential Client", body)`
**End-User Example:**

Subject: OTP for Account Request

Recipient: victor.mahluza@email.com

Body:
Hi Victor Mahluza,

Your One-Time Password (OTP) for your account request is: 482193

Please use this OTP to complete your registration. This code is valid for 10 minutes.

If you did not request this, please ignore this email.

Thank you,
The e-SO Team

---

## 4. `SubObjectiveService.java`


### Function: `sendStyledEmail(sponsorEmail, "SubObjective Added", sponsorName, message)`
**End-User Example:**

Subject: SubObjective Added

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

A new sub-objective, "Develop Data Visualization Module", has been added to the strategic objective "Enhance Reporting" in your project, Real-time Data-Driven Decision Support System.

Best regards,
e-SO Team


### Function: `sendStyledEmail(sponsorEmail, "SubObjective 100%", sponsorName, message)`
**End-User Example:**

Subject: SubObjective 100%

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

The sub-objective "Develop Data Visualization Module" of the strategic objective "Enhance Reporting" in your project, Real-time Data-Driven Decision Support System, has reached 100% completion.

Congratulations on this achievement!

Best regards,
e-SO Team

---

## 5. `UserRequirementService.java`


### Function: `sendAssessmentEmail(...)`
**End-User Example:**

Subject: User Requirement Assessment for Real-time Data-Driven Decision Support System

Recipient: sarah.ndlovu@email.com, jimmy.mahluza@email.com

Body:
Hi Sarah Ndlovu and Jimmy Mahluza,

A User Requirement assessment has been completed for your project, Real-time Data-Driven Decision Support System.

Overall Score: 82.0

Please review the attached assessment report for details. Immediate attention is required as the score is below the 85% threshold.

Best regards,
e-SO Team

---

## 6. `ProjectReadinessService.java`


### Function: `sendAssessmentEmail(...)`
**End-User Example:**

Subject: Project Readiness Assessment for Real-time Data-Driven Decision Support System

Recipient: sarah.ndlovu@email.com, jimmy.mahluza@email.com

Body:
Hi Sarah Ndlovu and Jimmy Mahluza,

The Project Readiness Assessment has been completed for your project, Real-time Data-Driven Decision Support System.

Overall Score: 88.0

Please find the attached document containing the detailed assessment report.

Should you have any questions or require further clarification regarding the assessment, feel free to reach out.

Thank you for your attention to this matter, and we wish you continued success with your project.

---

## 7. `StrategyService.java`


### Function: `sendStyledEmail(user.getUserEmail(), subject, userName, emailBody.toString())`
**End-User Example:**

Subject: Strategic Objectives Update

Recipient: victor.mahluza@email.com

Body:
Hi Victor Mahluza,

Here is a summary of your current strategic objectives for the project "Real-time Data-Driven Decision Support System":

1. Enhance Reporting
2. Improve Data Accuracy
3. Increase User Adoption

Please review these objectives and take the necessary actions to ensure they are aligned with the project goals.

Best regards,
e-SO Team

---

## 8. `PayStackService.java`


### Function: `sendStyledEmail(recipientEmail, subject, recipientName, emailBody.toString(), file)`
**End-User Example:**

Subject: Payment Receipt - Transaction #123456

Recipient: customer@email.com

Body:
Dear Victor Mahluza,

Thank you for your payment. Please find your payment receipt attached.

Transaction Details:
Transaction ID: 123456
Reference: TXN-20250714
Amount: $1,200.00
Status: Successful
Date: July 14, 2025
Payment Method: Credit Card

Please keep this receipt for your records. If you have any questions about this transaction, please contact our support team.

Best regards,
e-SO Team


### Function: `sendStyledEmail(recipientEmail, subject, recipientName, emailBody.toString(), pdfFile)`
**End-User Example:**

Subject: Payment Receipt - Transaction #123456

Recipient: customer@email.com

Body:
Dear Victor Mahluza,

Thank you for your payment. Please find your payment receipt attached.

Transaction Details:
Transaction ID: 123456
Reference: TXN-20250714
Amount: $1,200.00
Status: Successful
Date: July 14, 2025
Payment Method: Credit Card

Please keep this receipt for your records. If you have any questions about this transaction, please contact our support team.

Best regards,
e-SO Team

---

## 9. `StrategicObjectiveScheduler.java`


### Function: `sendStyledEmail(userData.getSponsorEmail(), subject, userData.getSponsorFullName(), body)`
**End-User Example:**

Subject: Strategic Objective Due Notification

Recipient: jimmy.mahluza@email.com

Body:
Hi Jimmy Mahluza,

This is to notify you that the strategic objective "Enhance Reporting" for your project, Real-time Data-Driven Decision Support System, is due on July 20, 2025.

Please review and take the necessary actions.

Best regards,
e-SO Team

---

**Note:**
- The actual HTML and text content for each email is constructed in the respective service methods, often using StringBuilder or String.format for dynamic content.
- For full templates, refer to the source code in each service for the exact HTML structure.
