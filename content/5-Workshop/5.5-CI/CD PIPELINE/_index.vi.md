---
title : "CI/CD_Pipeline"
date: "2025-12-07"  
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tổng quan

Phần này hướng dẫn bạn thiết lập quy trình CI/CD tự động cho hệ thống Serverless Student Management System trên AWS. Bạn sẽ sử dụng các dịch vụ như Amazon S3 để lưu trữ artifacts, AWS CodeBuild để build ứng dụng frontend và backend, và AWS CodePipeline để tự động hóa toàn bộ quá trình build, test, deploy. Việc áp dụng CI/CD giúp đảm bảo ứng dụng luôn được cập nhật nhanh chóng, giảm thiểu lỗi thủ công, nâng cao hiệu quả phát triển và vận hành hệ thống trên nền tảng AWS.

Sơ đồ tổng quan:

![Event-driven Architecture](/images/5-Workshop/5.5-CICD_Pipeline/architecture.png)

---

## 1. Tạo S3 Bucket cho Artifacts

```
Bucket name: student-management-artifacts-{account-id}
Region: ap-southeast-1
Versioning: Enabled
```

## 2. Tạo CodeBuild Project

1. Vào **AWS Console → CodeBuild → Create build project**

**Frontend Build:**
```
Project name: student-management-frontend-build
Source: GitLab (hoặc GitHub)
Environment:
  - Managed image
  - Operating system: Ubuntu
  - Runtime: Standard
  - Image: aws/codebuild/standard:7.0
Buildspec: Use buildspec file
```

**buildspec-frontend.yml:**
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - cd serverless-student-management-system-front-end
      - npm ci
  
  build:
    commands:
      - npm run build

artifacts:
  files:
    - '**/*'
  base-directory: serverless-student-management-system-front-end/build

cache:
  paths:
    - 'serverless-student-management-system-front-end/node_modules/**/*'
```

**Backend Build:**
```
Project name: student-management-backend-build
```

**buildspec-backend.yml:**
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto17
  
  build:
    commands:
      - cd backend-project
      - mvn clean package -DskipTests

artifacts:
  files:
    - backend-project/target/*.jar
    - appspec.yml
  discard-paths: no

cache:
  paths:
    - '/root/.m2/**/*'
```

## 3. Tạo CodePipeline

1. Vào **AWS Console → CodePipeline → Create pipeline**

**Pipeline settings:**
- Pipeline name: `student-management-pipeline`
- Service role: New service role

**Source stage:**
- Source provider: GitLab (hoặc GitHub)
- Repository: your-repo
- Branch: main
- Change detection: GitLab webhooks

**Build stage:**
- Build provider: AWS CodeBuild
- Project name: student-management-frontend-build

**Deploy stage:**
- Deploy provider: Amazon S3 (cho frontend artifacts)
- Hoặc: AWS Amplify (nếu dùng Amplify)

---


#### Tổng kết
Qua phần này, bạn đã biết cách thiết lập quy trình CI/CD tự động cho hệ thống Serverless Student Management System bằng các dịch vụ AWS như S3, CodeBuild và CodePipeline. Bạn đã thực hành tạo S3 bucket lưu trữ artifacts, cấu hình các project build cho frontend và backend, cũng như xây dựng pipeline tự động hóa toàn bộ quá trình build, test và deploy. Việc áp dụng CI/CD giúp đảm bảo hệ thống luôn được cập nhật, triển khai nhanh chóng, giảm thiểu lỗi thủ công và nâng cao hiệu quả phát triển phần mềm.
