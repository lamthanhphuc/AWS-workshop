---
title: "Event 3"
date: "2006-01-02"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch "AWS Cloud Mastery Series #3 - Security on AWS (Well-Architected Security Pillar)"

### Mục Đích Của Sự Kiện

- Giới thiệu AWS Well-Architected Security Pillar và 5 trụ cột bảo mật
- Chia sẻ best practices về Identity & Access Management (IAM)
- Hướng dẫn triển khai Detection và Continuous Monitoring
- Thực hành Infrastructure Protection và Data Protection
- Xây dựng Incident Response playbook và automation


### Nội Dung Nổi Bật

#### 8:30 – 8:50 AM | Opening & Security Foundation

- Vai trò **Security Pillar** trong Well-Architected Framework
- Nguyên tắc cốt lõi: **Least Privilege** – **Zero Trust** – **Defense in Depth**
- **Shared Responsibility Model**: Phân định trách nhiệm giữa AWS và khách hàng
- Top security threats trong môi trường cloud tại Việt Nam

#### Pillar 1 — Identity & Access Management (8:50 – 9:30 AM)

**Modern IAM Architecture**
- **IAM fundamentals**: Users, Roles, Policies – tránh long-term credentials
- **IAM Identity Center**: SSO implementation, permission sets
- **SCP & permission boundaries**: Multi-account governance
- **MFA, credential rotation, Access Analyzer**: Security best practices
- **Mini Demo**: Validate IAM Policy + simulate access scenarios

#### Pillar 2 — Detection (9:30 – 9:55 AM)

**Detection & Continuous Monitoring**
- **CloudTrail**: Organization-level audit logging
- **GuardDuty**: Threat detection và intelligent monitoring
- **Security Hub**: Centralized security posture management
- **Logging strategy**: VPC Flow Logs, ALB logs, S3 access logs
- **Alerting & automation**: EventBridge integration
- **Detection-as-Code**: Infrastructure và security rules as code

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

### Những Gì Học Được

#### Security Foundation

- **Well-Architected Security Pillar**: Framework toàn diện cho cloud security
- **Security principles**: Least Privilege, Zero Trust, Defense in Depth
- **Shared Responsibility Model**: Hiểu rõ trách nhiệm của AWS và khách hàng
- **Threat landscape**: Top security threats trong cloud environment

#### Identity & Access Management

- **IAM best practices**: Avoid long-term credentials, use roles
- **IAM Identity Center**: Centralized SSO và permission management
- **Multi-account governance**: SCPs và permission boundaries
- **Access validation**: IAM Access Analyzer và policy simulation

#### Detection & Monitoring

- **Comprehensive logging**: CloudTrail, VPC Flow Logs, application logs
- **Threat detection**: GuardDuty intelligent monitoring
- **Centralized security**: Security Hub for posture management
- **Automated response**: EventBridge integration với Lambda

#### Infrastructure Protection

- **Network segmentation**: Proper VPC architecture design
- **Layered security**: Security Groups + NACLs + WAF/Shield
- **Attack protection**: DDoS protection, web application firewall
- **Workload security**: Container và compute security

#### Data Protection

- **Encryption everywhere**: At-rest và in-transit encryption
- **Key management**: KMS best practices, rotation strategies
- **Secrets management**: Automated rotation với Secrets Manager
- **Data classification**: Compliance và access control

#### Incident Response

- **IR preparedness**: Playbooks và response procedures
- **Automation**: Auto-remediation với serverless
- **Forensics**: Evidence collection và analysis
- **Continuous improvement**: Post-incident reviews

### Ứng Dụng Vào Công Việc

- **Implement IAM best practices**: Review và refactor IAM policies
- **Enable comprehensive logging**: CloudTrail, GuardDuty, Security Hub
- **Apply encryption**: Enable encryption for all data stores
- **Develop IR playbooks**: Create response procedures cho common incidents
- **Automate security**: Build automated security responses
- **Security training**: Share knowledge với team về security best practices

### Trải nghiệm trong event

Tham gia workshop **"AWS Cloud Mastery Series #3 - Security on AWS"** là một buổi sáng tập trung và chuyên sâu về bảo mật cloud, giúp tôi hiểu rõ hơn về cách xây dựng một hệ thống an toàn trên AWS. Một số trải nghiệm nổi bật:

#### Học hỏi từ các chuyên gia Security
- Các diễn giả từ AWS đã chia sẻ **Well-Architected Security Pillar** với 5 trụ cột bảo mật quan trọng.
- Qua các demo và case studies, tôi hiểu rõ hơn về **threat landscape** và các **security threats** phổ biến.

#### Thực hành bảo mật toàn diện
- Tham gia các phiên về **IAM** giúp tôi hiểu về modern identity management và **Zero Trust** approach.
- Học cách triển khai **detection và monitoring** với CloudTrail, GuardDuty, Security Hub.
- Thực hành **infrastructure protection** với VPC segmentation, WAF, Shield.

#### Data Protection và Encryption
- Tìm hiểu sâu về **AWS KMS** và encryption strategies cho mọi tầng.
- Học cách quản lý **secrets** và implement automatic rotation.
- Hiểu được data classification và compliance requirements.

#### Incident Response
- Workshop cung cấp **IR playbooks** thực tế cho các scenarios phổ biến.
- Học cách **automate response** với Lambda và Step Functions.
- Hiểu được forensics process và evidence collection.

#### Kết nối và trao đổi
- Trao đổi với security professionals về real-world security challenges.
- Thảo luận về **security threats** đặc thù tại Việt Nam.
- Networking với các security engineers và architects.

#### Bài học rút ra
- **Security is everyone's responsibility**: Không chỉ là việc của security team.
- **Defense in depth**: Multiple layers of security controls.
- **Automation is critical**: Automate detection và response.
- **Continuous monitoring**: Security là continuous process, không phải one-time setup.
- **Preparedness matters**: IR playbooks và automation giúp respond nhanh hơn.