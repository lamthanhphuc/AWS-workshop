---
title: "Worklog Tuần 12"
date: "2006-01-02"
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12:

* Deploy Backend lên AWS (API Gateway + Lambda + DynamoDB).
* Hoàn thiện Proposal document và Architecture diagrams.
* Chuẩn bị Demo và Presentation.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Deploy Backend lên AWS: <br>&emsp; + SAM deploy to AWS <br>&emsp; + Configure API Gateway stages <br>&emsp; + Setup Cognito User Pool <br>&emsp; + Environment variables | 24/11/2025 | 24/11/2025 | [AWS SAM Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |
| 3   | - Cấu hình Production: <br>&emsp; + Enable CloudWatch logging <br>&emsp; + Setup alarms và monitoring <br>&emsp; + API throttling và quotas <br>&emsp; + Security review | 25/11/2025 | 25/11/2025 | [CloudWatch Monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 4   | - Hoàn thiện Proposal document: <br>&emsp; + Cập nhật Architecture diagram <br>&emsp; + Ước tính chi phí thực tế <br>&emsp; + Đánh giá rủi ro <br>&emsp; + Lộ trình triển khai | 26/11/2025 | 26/11/2025 | [Proposal Document](../../2-Proposal/) |
| 5   | - Chuẩn bị Demo: <br>&emsp; + Test tất cả API endpoints <br>&emsp; + Chuẩn bị demo scenarios <br>&emsp; + Record demo video (nếu cần) <br>&emsp; + Troubleshoot issues | 27/11/2025 | 27/11/2025 | [Postman API Testing](https://www.postman.com/api-platform/api-testing/) |
| 6   | - Chuẩn bị Presentation: <br>&emsp; + Slide deck <br>&emsp; + Technical deep-dive <br>&emsp; + Q&A preparation <br>&emsp; + Final review | 28/11/2025 | 28/11/2025 | [AWS Serverless Samples](https://github.com/aws-samples/serverless-patterns) |

### Kết quả đạt được tuần 12:

* Deploy thành công lên AWS:
  * API Gateway endpoint: `https://xxx.execute-api.region.amazonaws.com/prod`
  * Lambda functions deployed và hoạt động
  * DynamoDB tables với data
  * Cognito authentication integrated

* Monitoring và Security:
  * CloudWatch dashboards
  * API Gateway throttling: 1000 req/sec
  * Cognito JWT authentication
  * IAM least privilege policies

* **Tổng kết Backend APIs hoàn thành:**

  | # | API | Method | Endpoint |
  |---|-----|--------|----------|
  | 1 | Create Class | POST | `/lecturer/classes` |
  | 2 | List Classes | GET | `/lecturer/classes` |
  | 3 | Edit Class | PUT | `/lecturer/classes/{id}` |
  | 4 | Deactivate Class | DELETE | `/lecturer/classes/{id}` |
  | 5 | List Students | GET | `/lecturer/students/{class_id}` |
  | 6 | Create Assignment | POST | `/lecturer/assignments` |
  | 7 | List Assignments | GET | `/lecturer/classes/{class_id}/assignments` |
  | 8 | Edit Assignment | PUT | `/lecturer/assignments/{id}` |
  | 9 | Delete Assignment | DELETE | `/lecturer/assignments/{id}` |
  | 10 | Update Grades | POST | `/lecturer/assignments/{assignment_id}/update-grades` |
  | 11 | Create Post | POST | `/classes/{class_id}/posts` |

* Proposal Document hoàn thiện:
  * Architecture diagram cập nhật
  * Chi phí ước tính: ~$7-20/tháng
  * Đánh giá 8 rủi ro chính
  * Lộ trình 14 tuần

* **Tổng kết 12 tuần thực tập:**
  * Tuần 1-7: Học AWS fundamentals (VPC, EC2, S3, IAM, RDS, Security)
  * Tuần 8-12: Phát triển Backend APIs cho Serverless Student Management System
  * Hoàn thành 11 REST APIs với Lambda + DynamoDB
  * Deploy production-ready system trên AWS




