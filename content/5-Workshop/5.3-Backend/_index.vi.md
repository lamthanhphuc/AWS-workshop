---
title : "Triển khai Backend: DynamoDB, Lambda, API Gateway, Cognito"
date: "2025-12-07"
weight : 3
chapter : false
pre : "<b> 5.3. </b>"
---


---

# 1. Amazon DynamoDB

DynamoDB là cơ sở dữ liệu NoSQL serverless, lưu trữ tất cả dữ liệu của hệ thống:

- Thông tin sinh viên  
- Lớp học, môn học  
- Giảng viên  
- Điểm số  
- Lịch sử chat, sự kiện hệ thống  
- Dữ liệu ML phục vụ gợi ý (Personalize)  

Hệ thống backend Spring Boot tương tác DynamoDB qua:

- AWS SDK for Java 17  
- Spring Data DynamoDB hoặc repository do team tự viết  
- Presigned URL (upload tài liệu, hồ sơ)  
- EventBridge (phát sinh sự kiện academic event → xử lý qua Lambda)

---

##  **Thiết kế bảng DynamoDB (Single-Table Design)**

**Bảng:** `Student-Management-Database`

| Thành phần | Ý nghĩa |
|-----------|---------|
| PK        | USER#, CLASS#, SUBJECT#, TEACHER#, GRADE# |
| SK        | PROFILE, INFO, STUDENT#, SUBJECT#, CLASS# |
| GSI1PK    | ROLE#, TYPE#, EMAIL#, CLASS# |
| GSI1SK    | NAME#, CREATED_AT#, SUBJECT# |

**Billing mode:** On-Demand → phù hợp workshop (không cần cấu hình capacity).

![DynamoDB](/images/5-Workshop/5.2-Prerequisite/DynamoDB.png)

---

##  **Triển khai DynamoDB qua AWS CLI**

###  Tạo bảng

```bash
aws dynamodb create-table \
  --table-name Student-Management-Database \
  --attribute-definitions \
      AttributeName=PK,AttributeType=S \
      AttributeName=SK,AttributeType=S \
  --key-schema \
      AttributeName=PK,KeyType=HASH \
      AttributeName=SK,KeyType=RANGE \
  --billing-mode PAY_PER_REQUEST
```
# 2. Amazon Cognito

Cognito cung cấp:

- Quản lý tài khoản

- Đăng nhập bằng email

- JWT Authentication dùng Spring Security

- Phân quyền theo Group (student, lecturer, admin)

- Backend Spring Boot dùng:

- Cognito JWT Filter

- Spring Security @PreAuthorize("hasRole('ADMIN')")

##  **Triển khai Cognito qua AWS CLI**

### Tạo User Pool
```bash
aws cognito-idp create-user-pool \
  --pool-name Student-App-Pool \
  --auto-verified-attributes email
```
### Tạo App Client
```bash
aws cognito-idp create-user-pool-client \
  --user-pool-id <UserPoolId> \
  --client-name StudentAppClient \
  --no-generate-secret
  ```

# 3. Amazon S3
S3 được dùng cho:

- Lưu avatar sinh viên

- Tài liệu lớp học

- Build artifacts (React/Vite)

- Deploy website qua S3 + CloudFront

- Log & asset học liệu



## Triển khai S3 qua AWS CLI
### Tạo Bucket
```bash
aws s3api create-bucket \
  --bucket aws-sam-cli-managed-default-samclisourcebucket-qsrwrbr9usyq \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
  ```

# 4. Amazon API Gateway + Lambda
API Gateway đóng vai trò:

- Cổng API cho toàn hệ thống

- Xác thực Cognito Authorizer

- Route đến Lambda để xử lý nghiệp vụ

- Logging CloudWatch

- Tích hợp CORS

- Backend được triển khai qua:

- Lambda (Java 17, Maven build)

- API Gateway REST API

- Lambda Function URL (nội bộ)



##  Triển khai Lambda & API Gateway bằng AWS SAM

AWS SAM giúp triển khai backend serverless dễ dàng hơn nhờ:

- Quản lý Lambda, API Gateway, IAM Role trong 1 file template.yaml

- Build Java bằng Maven tự động (sam build)

- Triển khai full stack bằng một lệnh (sam deploy --guided)
### File template.yaml (SAM)
Tạo file:
```yaml

AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Student Management System - Backend (Lambda + API Gateway)

Globals:
  Function:
    Timeout: 15
    MemorySize: 512
    Runtime: java17

Resources:

  StudentServiceFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: com.example.Handler::handleRequest
      CodeUri: ./
      Policies:
        - AWSLambdaBasicExecutionRole
        - AmazonDynamoDBFullAccess
      Events:
        GetStudents:
          Type: Api
          Properties:
            Path: /students
            Method: GET
            Auth:
              Authorizer: CognitoAuthorizer
    CognitoAuthorizer:
        Type: AWS::Serverless::Api
        Properties:
        StageName: prod
        Auth:
            Authorizers:
            CognitoAuthorizer:
                UserPoolArn: arn:aws:cognito-idp:<region>:<account-id>:userpool/<UserPoolId>
```
### Build Lambda bằng SAM

SAM tự chạy Maven, tự đóng gói JAR:
```bash
sam build
```
### Deploy bằng SAM
```bash
sam deploy --guided
```

Lần đầu bạn nhập:

- Stack Name: student-management-backend

- Region: ap-southeast-1

- Allow SAM to create IAM roles? → Y

- Save arguments? → Y

Sau khi deploy thành công, SAM sẽ trả về:

- API Endpoint

- Lambda ARN

- CloudFormation Stack
# Tổng kết

Các thành phần này tạo nền tảng để xây dựng hệ thống Serverless – Realtime – Event-Driven cho Student Management System.