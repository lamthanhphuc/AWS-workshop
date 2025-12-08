---
title: "Các bước chuẩn bị"
date: "2025-12-07"
weight: 2
chapter: false
pre: "<b> 5.2. </b>"
---

Để triển khai thành công Workshop **Serverless Student Management System**, cần chuẩn bị đầy đủ môi trường kỹ thuật, tài khoản AWS và các dịch vụ nền tảng theo kiến trúc serverless – event-driven.  
Trang này hướng dẫn những bước thiết yếu trước khi bắt đầu phát triển backend, frontend và các thành phần sự kiện.

---

## Chuẩn bị tài khoản AWS

### Tạo và cấu hình tài khoản AWS

- Tạo tài khoản AWS cá nhân hoặc sử dụng AWS Educate/AWS Academy.
- Kích hoạt **AWS Free Tier** để tối ưu chi phí trong giai đoạn workshop.
- Bật **MFA** để bảo mật tài khoản root.
- Tạo **IAM User** dành cho các thành viên nhóm và phân vai trò theo nguyên tắc **Least Privilege**.

![IAM User](/images/5-Workshop/5.2-Prerequisite/IAMUser.png)

<center><i>Hình 1: Các IAM user mẫu.</i></center>

---

## Thiết lập các quyền IAM (IAM Permissions)

Để triển khai đầy đủ hệ thống serverless của dự án, IAM User cần quyền thao tác với:

- AWS Lambda
- DynamoDB
- API Gateway
- Cognito
- S3
- CloudWatch
- Amplify
- IAM (giới hạn ở PassRole và các thao tác tạo role phục vụ Lambda)

Một ví dụ policy cấp quyền rộng cho mục đích workshop:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ServerlessStudentManagementWorkshopAccess",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "cloudwatch:*",
        "logs:*",
        "s3:*",
        "lambda:*",
        "dynamodb:*",
        "apigateway:*",
        "cognito-idp:*",
        "cognito-identity:*",
        "route53:*",
        "acm:*",
        "waf:*",
        "cloudfront:*",
        "amplify:*",
        "codebuild:*",
        "codedeploy:*",
        "codepipeline:*"
      ],
      "Resource": "*"
    }
  ]
}
```
