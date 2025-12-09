---
title: "Event 3"
date: "2006-01-02"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #3 - Security on AWS (Well-Architected Security Pillar)"

### Event Objectives

- Introduce AWS Well-Architected Security Pillar and 5 security pillars
- Share best practices for Identity & Access Management (IAM)
- Guide on implementing Detection and Continuous Monitoring
- Practice Infrastructure Protection and Data Protection
- Build Incident Response playbooks and automation

### Key Highlights

#### 8:30 – 8:50 AM | Opening & Security Foundation

- Role of **Security Pillar** in Well-Architected Framework
- Core principles: **Least Privilege** – **Zero Trust** – **Defense in Depth**
- **Shared Responsibility Model**: Defining responsibilities between AWS and customers
- Top security threats in cloud environment in Vietnam

#### Pillar 1 — Identity & Access Management (8:50 – 9:30 AM)

**Modern IAM Architecture**
- **IAM fundamentals**: Users, Roles, Policies – avoid long-term credentials
- **IAM Identity Center**: SSO implementation, permission sets
- **SCP & permission boundaries**: Multi-account governance
- **MFA, credential rotation, Access Analyzer**: Security best practices
- **Mini Demo**: Validate IAM Policy + simulate access scenarios

#### Pillar 2 — Detection (9:30 – 9:55 AM)

**Detection & Continuous Monitoring**
- **CloudTrail**: Organization-level audit logging
- **GuardDuty**: Threat detection and intelligent monitoring
- **Security Hub**: Centralized security posture management
- **Logging strategy**: VPC Flow Logs, ALB logs, S3 access logs
- **Alerting & automation**: EventBridge integration
- **Detection-as-Code**: Infrastructure and security rules as code

#### Pillar 3 — Infrastructure Protection (10:10 – 10:40 AM)

**Network & Workload Security**
- **VPC segmentation**: Public vs private subnet placement strategies
- **Security Groups vs NACLs**: Layered security model
- **WAF + Shield + Network Firewall**: Protection against attacks
- **Workload protection**: EC2, ECS/EKS security fundamentals

#### Pillar 4 — Data Protection (10:40 – 11:10 AM)

**Encryption, Keys & Secrets Management**
- **AWS KMS**: Key policies, grants, automatic rotation
- **Encryption strategies**: 
  - At-rest: S3, EBS, RDS, DynamoDB
  - In-transit: TLS/SSL, VPN, encryption in application layer
- **Secrets Manager & Parameter Store**: Secrets rotation patterns
- **Data classification & access guardrails**: Compliance requirements

#### Pillar 5 — Incident Response (11:10 – 11:40 AM)

**IR Playbook & Automation**
- **Incident Response lifecycle**: Preparation, Detection, Analysis, Containment, Eradication, Recovery
- **Common IR playbooks**:
  - Compromised IAM credentials
  - S3 bucket public exposure
  - EC2 malware detection
- **Response automation**: Lambda/Step Functions for automated remediation
- **Evidence collection**: Snapshot, isolation, forensics

### Key Takeaways

#### Security Foundation

- **Well-Architected Security Pillar**: Comprehensive framework for cloud security
- **Security principles**: Least Privilege, Zero Trust, Defense in Depth
- **Shared Responsibility Model**: Understanding AWS and customer responsibilities
- **Threat landscape**: Top security threats in cloud environment

#### Identity & Access Management

- **IAM best practices**: Avoid long-term credentials, use roles
- **IAM Identity Center**: Centralized SSO and permission management
- **Multi-account governance**: SCPs and permission boundaries
- **Access validation**: IAM Access Analyzer and policy simulation

#### Detection & Monitoring

- **Comprehensive logging**: CloudTrail, VPC Flow Logs, application logs
- **Threat detection**: GuardDuty intelligent monitoring
- **Centralized security**: Security Hub for posture management
- **Automated response**: EventBridge integration with Lambda

#### Infrastructure Protection

- **Network segmentation**: Proper VPC architecture design
- **Layered security**: Security Groups + NACLs + WAF/Shield
- **Attack protection**: DDoS protection, web application firewall
- **Workload security**: Container and compute security

#### Data Protection

- **Encryption everywhere**: At-rest and in-transit encryption
- **Key management**: KMS best practices, rotation strategies
- **Secrets management**: Automated rotation with Secrets Manager
- **Data classification**: Compliance and access control

#### Incident Response

- **IR preparedness**: Playbooks and response procedures
- **Automation**: Auto-remediation with serverless
- **Forensics**: Evidence collection and analysis
- **Continuous improvement**: Post-incident reviews

### Applying to Work

- **Implement IAM best practices**: Review and refactor IAM policies
- **Enable comprehensive logging**: CloudTrail, GuardDuty, Security Hub
- **Apply encryption**: Enable encryption for all data stores
- **Develop IR playbooks**: Create response procedures for common incidents
- **Automate security**: Build automated security responses
- **Security training**: Share knowledge with team about security best practices

### Event Experience

Attending the **"AWS Cloud Mastery Series #3 - Security on AWS"** workshop was a focused and in-depth morning on cloud security, helping me better understand how to build a secure system on AWS. Key experiences included:

#### Learning from Security experts
- Speakers from AWS shared the **Well-Architected Security Pillar** with 5 important security pillars.
- Through demos and case studies, I gained a better understanding of the **threat landscape** and common **security threats**.

#### Comprehensive security practice
- Participating in **IAM** sessions helped me understand modern identity management and **Zero Trust** approach.
- Learned how to implement **detection and monitoring** with CloudTrail, GuardDuty, Security Hub.
- Practiced **infrastructure protection** with VPC segmentation, WAF, Shield.

#### Data Protection and Encryption
- Deep dive into **AWS KMS** and encryption strategies for every layer.
- Learned how to manage **secrets** and implement automatic rotation.
- Understood data classification and compliance requirements.

#### Incident Response
- Workshop provided practical **IR playbooks** for common scenarios.
- Learned how to **automate response** with Lambda and Step Functions.
- Understood forensics process and evidence collection.

#### Networking and discussions
- Exchanged ideas with security professionals about real-world security challenges.
- Discussed **security threats** specific to Vietnam.
- Networked with security engineers and architects.

#### Lessons learned
- **Security is everyone's responsibility**: Not just the security team's job.
- **Defense in depth**: Multiple layers of security controls.
- **Automation is critical**: Automate detection and response.
- **Continuous monitoring**: Security is a continuous process, not a one-time setup.
- **Preparedness matters**: IR playbooks and automation help respond faster.