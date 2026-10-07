# Cloud Security Posture Management (CSPM)

An automated **Cloud Security Posture Management (CSPM)** solution for detecting and remediating insecure configurations in newly created **Amazon S3 buckets**.

The project uses an event-driven AWS architecture to automatically detect S3 bucket creation, apply security controls to block public access, and notify the user through email.

---

## 📌 Project Overview

Cloud misconfigurations can expose cloud resources to unauthorized access. This project demonstrates a lightweight CSPM solution that automatically secures newly created Amazon S3 buckets.

Whenever a new S3 bucket is created:

1. **AWS CloudTrail** captures the API activity.
2. **Amazon EventBridge** detects the `CreateBucket` event.
3. **AWS Lambda** is triggered automatically.
4. Lambda extracts the bucket name and applies security settings to block public access.
5. **Amazon SNS** sends an email notification to the user.
6. The user receives an alert confirming the security action.

This provides an automated approach to cloud security monitoring and remediation.

---

## 🎯 Objectives

- Automatically detect newly created S3 buckets.
- Monitor S3 API activity using AWS CloudTrail.
- Detect `CreateBucket` events using Amazon EventBridge.
- Automatically remediate insecure S3 configurations.
- Block public access to S3 buckets.
- Send real-time email notifications using Amazon SNS.
- Implement appropriate IAM permissions for the Lambda function.
- Demonstrate event-driven cloud security automation.

---

## 🏗️ Architecture

```text
                 ┌─────────────────────┐
                 │    Amazon S3        │
                 │  New Bucket Created │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    AWS CloudTrail   │
                 │   API Activity Log  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Amazon EventBridge│
                 │  Detect CreateBucket│
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │     AWS Lambda      │
                 │  Python Remediation │
                 └─────────┬───────────┘
                           │
                ┌──────────┴───────────┐
                ▼                      ▼
      ┌──────────────────┐   ┌──────────────────┐
      │   Amazon S3      │   │    Amazon SNS    │
      │ Block Public     │   │  Email Alert     │
      │     Access       │   │                  │
      └──────────────────┘   └────────┬─────────┘
                                      │
                                      ▼
                              ┌────────────────┐
                              │  User Email    │
                              │ Notification   │
                              └────────────────┘

☁️ AWS Services Used
AWS Service	Role in the Project
Amazon S3	Cloud resource being monitored and secured
AWS CloudTrail	Captures AWS API activity
Amazon EventBridge	Detects S3 bucket creation events
AWS Lambda	Performs automated security remediation
Amazon SNS	Sends email security notifications
AWS IAM	Provides required permissions to Lambda
Amazon CloudWatch	Used to view Lambda execution logs


🔄 System Workflow
Step 1 — Create an S3 Bucket
A new Amazon S3 bucket is created to simulate a real-world cloud resource creation scenario.
Step 2 — Capture the API Activity
AWS CloudTrail captures the API activity associated with the S3 bucket creation.
Step 3 — Detect the Event
Amazon EventBridge monitors CloudTrail events and detects the S3 CreateBucket event.
Step 4 — Trigger Lambda
Once the matching event is detected, EventBridge automatically invokes the AWS Lambda function.
Step 5 — Extract Bucket Information
The Lambda function extracts the bucket name from the event received from EventBridge.
Step 6 — Apply Security Remediation
The Lambda function applies security settings to block public access to the S3 bucket.
Step 7 — Send Notification
After performing the security action, Lambda publishes a notification to an Amazon SNS topic.
Step 8 — Receive Email Alert
The confirmed SNS email subscription receives the security notification.
⚙️ EventBridge Configuration
The EventBridge rule is configured to detect S3 bucket creation events generated through CloudTrail.
Event Pattern
{
  "source": ["aws.s3"],
  "detail-type": ["AWS API Call via CloudTrail"],
  "detail": {
    "eventSource": ["s3.amazonaws.com"],
    "eventName": ["CreateBucket"]
  }
}

The rule specifically looks for:
- AWS S3 as the event source
- CloudTrail API events
- S3 as the event service
- CreateBucket as the API event
🐍 AWS Lambda
The remediation function is implemented using Python.
The Lambda function performs the following tasks:
1. Receives the event from EventBridge.
2. Extracts the S3 bucket name.
3. Applies security settings to block public access.
4. Publishes an alert through Amazon SNS.
This provides automated remediation without requiring manual intervention after the bucket creation event.
🔐 IAM Configuration
An IAM role is assigned to the Lambda function.
The role provides permissions required to:
- Modify S3 bucket configurations.
- Publish messages to Amazon SNS.
IAM is used to control what actions the Lambda function is allowed to perform within the AWS environment.
📢 SNS Notification
An Amazon SNS topic was created for CSPM security alerts.
An email subscription was configured and confirmed so that the user can receive notifications generated by the Lambda function.
The notification provides information about the affected bucket and the security action performed.
🛡️ Security Remediation
The primary security control implemented in this project is:
S3 Public Access Blocking
When the relevant bucket creation event is detected, Lambda applies security settings to block public access.
This helps prevent an S3 bucket from remaining publicly accessible due to an insecure configuration.
🧪 Testing
The complete system was tested by creating a new S3 bucket.
Test Bucket
final-test-bucket-789

Expected Workflow
S3 Bucket Created
       ↓
CloudTrail Captures Event
       ↓
EventBridge Detects CreateBucket
       ↓
Lambda Function Triggered
       ↓
Security Configuration Applied
       ↓
Public Access Blocked
       ↓
SNS Notification Generated
       ↓
Email Alert Received

Observed Results
The test successfully demonstrated that:
- CloudTrail captured the bucket creation event.
- EventBridge detected the CreateBucket event.
- Lambda was triggered.
- Security settings were applied.
- Public access was blocked.
- An email notification was successfully received.
📧 Sample Security Alert
A CSPM security alert was received through email.
Example information included in the alert:
CSPM ALERT

Project: CSPM Auto Remediation

Bucket: final-test-bucket-789
Region: ap-south-1

Issue: PUBLIC ACCESS DETECTED
Fix: Converted to PRIVATE

Trigger: AWS Lambda

The notification provides the user with information about the affected resource, detected issue, remediation action, and trigger.
📊 Key Features
- ✅ Automated S3 security monitoring
- ✅ Event-driven security architecture
- ✅ CloudTrail API activity monitoring
- ✅ EventBridge event detection
- ✅ Python-based AWS Lambda remediation
- ✅ Automatic S3 public-access blocking
- ✅ SNS email notifications
- ✅ IAM-based access control
- ✅ CloudWatch execution logging
- ✅ Automated security remediation
- ✅ Real-time security response
🛠️ Technologies Used
Cloud Platform
- Amazon Web Services (AWS)
Programming Language
- Python
AWS Services
- Amazon S3
- AWS Lambda
- AWS CloudTrail
- Amazon EventBridge
- Amazon SNS
- AWS IAM
- Amazon CloudWatch
Security Concepts
- Cloud Security Posture Management (CSPM)
- Cloud Security Automation
- Security Monitoring
- Automated Remediation
- Access Control
- Event-Driven Security
- Cloud Misconfiguration Detection
📁 Repository Structure
cloud-security-posture-management-cspm/
│
├── README.md
│
└── CSPM_Project_Report.pdf

📄 Project Documentation
The complete project report is available here:
[View CSPM Project Report](./CSPM_Project_Report.pdf)
The report contains:
- Project objective
- Architecture overview
- S3 bucket creation
- Lambda implementation
- EventBridge rule configuration
- CloudTrail configuration
- SNS notification setup
- IAM role configuration
- Testing and output
- Email notification evidence
- AWS resource cleanup
🧪 Validation and Results
The project was validated using an actual AWS workflow.
The test demonstrated the complete security lifecycle:
Detection
   ↓
Event Processing
   ↓
Automated Remediation
   ↓
Notification

The successful test confirmed that the system could detect a newly created S3 bucket, trigger the Lambda function, apply security settings, and notify the user.
🎓 Learning Outcomes
This project provided practical experience in cloud security and AWS automation.
Cloud Security
- Understanding the fundamentals of Cloud Security Posture Management.
- Identifying the security risks associated with cloud resource configurations.
- Understanding automated cloud security remediation.
AWS
- Working with Amazon S3.
- Configuring AWS CloudTrail.
- Creating Amazon EventBridge rules.
- Developing and configuring AWS Lambda functions.
- Configuring Amazon SNS notifications.
- Managing IAM roles and permissions.
- Viewing Lambda execution logs through CloudWatch.
Security Automation
- Designing an event-driven security workflow.
- Detecting cloud events automatically.
- Implementing automated remediation.
- Connecting multiple AWS services into a security pipeline.
- Validating security actions through testing.
Practical Skills
- Cloud architecture design
- Event-driven architecture
- Python-based serverless development
- AWS service integration
- IAM permission management
- Security monitoring
- Troubleshooting and testing
- Cloud resource cleanup
🚀 Future Scope
The current implementation focuses on detecting S3 bucket creation and automatically blocking public access.
The CSPM solution can be extended in several ways.
1. Additional AWS Resources
Extend monitoring beyond S3 to other AWS resources such as:
- EC2
- IAM
- RDS
- VPC
- Lambda
- CloudTrail
2. More Security Checks
Additional cloud security configuration checks can be introduced to detect different types of misconfigurations.
3. Automated Remediation
More remediation actions can be added so that different security issues can be automatically corrected.
4. Security Dashboard
A centralized dashboard could be developed to display:
- Detected security issues
- Affected resources
- Severity
- Remediation status
- Security events
- Historical activity
5. Security Reports
The system could generate centralized security reports for better monitoring and auditing.
6. Additional Notification Channels
The notification system could be extended to support additional communication channels.
7. Multi-Service CSPM
The project could be expanded into a broader CSPM platform capable of continuously monitoring multiple AWS services and security configurations.
🧹 Resource Cleanup
After implementation and testing, the AWS resources used for the project were deleted to avoid unnecessary AWS charges.
The cleaned-up resources included:
- S3 buckets
- AWS Lambda function
- Amazon EventBridge rule
- Amazon SNS topic
- AWS CloudTrail trail
- IAM roles
- KMS keys
Resource cleanup was performed after testing to ensure that unused AWS resources did not continue generating charges.
💡 Project Highlights
Automated Detection
The system automatically detects the creation of a new S3 bucket through CloudTrail and EventBridge.
Automated Remediation
AWS Lambda automatically applies the required security configuration.
Real-Time Notification
Amazon SNS provides an email notification after the security action.
Event-Driven Architecture
The project demonstrates how AWS services can be connected to create an automated security workflow.
Practical Cloud Security
The project provides hands-on implementation of cloud security monitoring, access control, event detection, and automated remediation.
📌 Project Summary
Cloud Security Posture Management (CSPM) is an automated AWS security solution designed to detect and remediate insecure configurations in newly created Amazon S3 buckets.
The solution uses:
AWS CloudTrail
↓
Amazon EventBridge
↓
AWS Lambda
↓
Amazon S3 Security Remediation
↓
Amazon SNS
↓
Email Notification
The project demonstrates how an event-driven architecture can be used to automate cloud security monitoring and remediation.
👩‍💻 Author
Sonali Gupta
BCA — Data Science Major | Cybersecurity Minor
📚 Project Type
Cloud Security | AWS | CSPM | Security Automation | Serverless Computing
⭐ Skills Demonstrated
AWS Cloud Security CSPM Amazon S3 AWS Lambda Python AWS CloudTrail Amazon EventBridge Amazon SNS IAM CloudWatch Security Automation Automated Remediation
