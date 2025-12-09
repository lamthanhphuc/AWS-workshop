---
title: "Báo cáo thực tập"
date: "2025-12-07"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

# AWS First Cloud AI Journey – Project Plan  
**Project team – FPT University – Serverless Student Management System**  
**[2025-12-07]**

---

## Table of Contents
1. [1. Tổng quan & Động lực](#tong-quan-va-dong-luc)
    - [1.1 Tóm tắt dự án](#tom-tat-du-an)
    - [1.2 Tiêu chí thành công](#tieu-chi-thanh-cong)
    - [1.3 Giả định](#gia-dinh)
2. [2. Kiến trúc giải pháp / Sơ đồ hệ thống](#kien-truc-giai-phap--so-do-he-thong)
    - [2.1 Sơ đồ kiến trúc kỹ thuật](#so-do-kien-truc-ky-thuat)
    - [2.2 Kế hoạch kỹ thuật](#ke-hoach-ky-thuat)
    - [2.3 Kế hoạch dự án](#ke-hoach-du-an)
    - [2.4 Xem xét bảo mật](#xem-xet-bao-mat)
3. [3. Hoạt động & Sản phẩm bàn giao](#hoat-dong--san-pham-ban-giao)
    - [3.1 Hoạt động & Sản phẩm bàn giao](#hoat-dong--san-pham-ban-giao-1)
    - [3.2 Ngoài phạm vi](#ngoai-pham-vi)
    - [3.3 Lộ trình lên production](#lo-trinh-len-production)
4. [4. Dự toán chi phí AWS theo dịch vụ](#du-toan-chi-phi-aws-theo-dich-vu)
5. [5. Nhóm dự án](#nhom-du-an)

---

## 1. Tổng quan & Động lực {#tong-quan-va-dong-luc}

### 1.1 Tóm tắt dự án {#tom-tat-du-an}
Serverless Student Management System là nền tảng quản lý sinh viên dựa trên AWS Serverless, hướng tới mục tiêu xây dựng hệ thống quản trị học tập – giảng dạy hiện đại, chi phí thấp và dễ mở rộng. Nền tảng giúp các tổ chức giáo dục quản lý sinh viên, khóa học, điểm số và bài tập dựa trên các dịch vụ như API Gateway, Lambda, DynamoDB, Cognito, Amplify.

**Customer background:**  
Dự án phục vụ cho giáo viên, sinh viên và những người đang học hoặc làm việc với AWS Cloud muốn trải nghiệm mô hình serverless thực tế.  

**Business & technical objectives:**  
- Xây dựng ứng dụng quản lý sinh viên theo mô hình serverless thật 100%  
- Tối ưu chi phí (dưới $20/tháng)  
- Tích hợp CI/CD đầy đủ từ GitLab → CodePipeline  
- Xây dựng full-stack (React/Amplify + API Gateway + Lambda + DynamoDB)  

**Use cases:**  
- Quản lý sinh viên (CRUD, search, import)  
- Quản lý khóa học, bài tập, điểm số  
- Dashboard cho giáo viên/admin  
- Xác thực phân quyền với Cognito  

**Professional services delivered:**  
- Thiết kế kiến trúc serverless theo AWS Well-Architected  
- Triển khai backend + frontend + CI/CD  
- Viết tài liệu, test, demo, monitoring  

---

### 1.2 Tiêu chí thành công {#tieu-chi-thanh-cong}
- 50+ REST API endpoints hoạt động ổn định trên API Gateway  
- 1 AWS Lambda functions có unit test >80%  
- 5 module chính hoàn chỉnh: Students, Courses, Grades, Assignments, Dashboard  
- Frontend React/TypeScript triển khai qua Amplify, 15+ pages  
- Thời gian phản hồi API <500ms  
- Uptime ≥ 99%  
- CI/CD tự động từ GitLab → CodeBuild → CodeDeploy/Amplify → Production  
- Chi phí hạ tầng duy trì < $20/tháng  

---

### 1.3 Giả định {#gia-dinh}
- Sử dụng AWS Free Tier trong suốt quá trình triển khai  
- Tất cả môi trường sử dụng dịch vụ serverless (không dùng EC2)  
- Người dùng (giáo viên/sinh viên) < 200 active users/tháng  
- Số lượng API calls dưới 500k/tháng  
- Dữ liệu DynamoDB dưới 10GB  
- Nhóm dự án có kiến thức cơ bản về React, Node.js và AWS Cloud  
- Các dịch vụ như Route53 domain, WAF có thể phát sinh chi phí nhưng nằm trong scope  

---

## 2. Kiến trúc giải pháp / Sơ đồ hệ thống {#kien-truc-giai-phap--so-do-he-thong}

### 2.1 Sơ đồ kiến trúc kỹ thuật {#so-do-kien-truc-ky-thuat}

![Platform Architecture Diagram](/images/2-Proposal/Solution.drawio.png)

### 2.2 Kế hoạch kỹ thuật {#ke-hoach-ky-thuat}
Hệ thống được xây dựng theo kiến trúc AWS Serverless, gồm 5 tầng chính:  
- **Edge Layer:** Route53, CloudFront, WAF  
- **Frontend Layer:** Amplify, Cognito  
- **Backend Layer:** API Gateway, Lambda, DynamoDB  
- **Monitoring:** CloudWatch  
- **CI/CD:** GitLab, CodePipeline, CodeBuild, CodeDeploy, S3  

Các API chính:  
- `/students/*` (CRUD, search, import/export)  
- `/courses/*`  
- `/grades/*`  
- `/assignments/*`  
- `/auth/*`  

### 2.3 Kế hoạch dự án {#ke-hoach-du-an}
Dự án thực hiện trong **5 tuần**, bao gồm:  
- **Tuần 1:** Setup AWS, IaC, DynamoDB, Cognito, Git repo  
- **Tuần 2:** Backend & Frontend Phase 1 (Lambda + API Gateway + UI cơ bản)  
- **Tuần 3:** Backend & Frontend Phase 2 (15+ pages, integration test, CloudFront + WAF)  
- **Tuần 4:** CI/CD (CodeBuild, CodeDeploy, CodePipeline)  
- **Tuần 5:** Testing, optimization, documentation, demo  

### 2.4 Xem xét bảo mật {#xem-xet-bao-mat}
- IAM least privilege  
- Cognito User Pool + JWT auth  
- API Gateway throttling (rate limit)  
- WAF rules (SQLi/XSS, IP block, rate limiting)  
- DynamoDB encryption  
- CloudWatch alarms  

---

## 3. Hoạt động & Sản phẩm bàn giao {#hoat-dong--san-pham-ban-giao}

### 3.1 Hoạt động & Sản phẩm bàn giao {#hoat-dong--san-pham-ban-giao-1}
- AWS Infrastructure (DynamoDB, Cognito, API Gateway, Lambda, S3, CloudWatch)  
- React/TypeScript frontend + Amplify hosting  
- 1 Lambda functions + 50+ API endpoints  
- CI/CD pipelines  
- Documentation + architecture diagrams  
- End-to-end testing, load testing  
- Demo video  

### 3.2 Ngoài phạm vi {#ngoai-pham-vi}
- Mobile App (iOS/Android)  
- Email/SMS OTP authentication nâng cao  
- AI recommender engine/ML scoring  
- Multi-region failover  
- Enterprise SSO (SAML, OIDC)  

### 3.3 Lộ trình lên production {#lo-trinh-len-production}
- Commit code → GitLab  
- CodePipeline (source)  
- CodeBuild build frontend/backend  
- CodeDeploy deploy Lambda & Amplify  
- CloudWatch logging & monitoring  
- DNS routing via Route53 + CloudFront  

---

## 4. Dự toán chi phí AWS theo dịch vụ {#du-toan-chi-phi-aws-theo-dich-vu}
| Service | Monthly Estimate | Notes |
|--------|------------------|-------|
| API Gateway | ~$3.8 | 100k–500k requests |
| Lambda | ~$0.5 | Free Tier includes 1M requests |
| DynamoDB | ~$1 | <10GB data |
| Cognito | $0 | <10k MAU |
| S3 | ~$0.13 | artifacts & hosting |
| CloudWatch | ~$4 | logs & metrics |
| Amplify | ~$1 | hosting frontend |
| Route53 | ~$1 | DNS hosted zone |
| WAF | ~$7 | Web ACL |
| CodeBuild | ~$0.9 | ~100 build minutes |
| CodePipeline | ~$1 | 1 pipeline |
| CloudFront | ~$1 | CDN traffic |

**Total: ~ $7 – $20 / month**  
**3 months: ~$21 – $60 (có Free Tier giảm còn $15–30)**  

---

## 5. Nhóm dự án {#nhom-du-an}
- **1 Backend Developer (AWS Lambda, API Gateway, DynamoDB)**  
- **1 Frontend Developer (React/TypeScript, Amplify)**  
- **1 DevOps Engineer (CI/CD, IAM, CodePipeline, CloudWatch)**  
- **1 Project Lead (Architecture, QA, Testing, Documentation)**  



