---
title: "CI/CD Pipeline"
date: "2025-12-07"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Overview

This section guides you through setting up an automated CI/CD process for your Serverless Student Management System on AWS. You will use services like Amazon S3 to store artifacts, AWS CodeBuild to build frontend and backend applications, and AWS CodePipeline to automate the entire build, test, and deploy process. Applying CI/CD helps ensure that applications are always updated quickly, minimize manual errors, and improve the efficiency of developing and operating systems on the AWS platform.

Overview diagram:

![Event-driven Architecture](/images/5-Workshop/5.5-CI-CD/architecture.png)

---

## 1. Create S3 Bucket for Artifacts

```
Bucket name: student-management-artifacts-{account-id}
Region: ap-southeast-1
Versioning: Enabled
```

---

## 2. Create CodeBuild Project

1. Go to **AWS Console → CodeBuild → Create build project**

**Frontend Build:**

```
Project name: student-management-frontend-build
Source: GitLab (or GitHub)
Environment:
- Managed images
- Operating system: Ubuntu
- Runtime: Standard
- Image: aws/codebuild/standard:7.0
Buildspec: Use buildspec file
```

**buildspec-frontend.yml:**

```yaml
version: 0.2

phasing:
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

phasing:
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
  - "/root/.m2/**/*"
```

---

## 3. Create CodePipeline

1. Go to **AWS Console → CodePipeline → Create pipeline**

**Pipeline settings:**

- Pipeline name: `student-management-pipeline`
- Service role: New service role

**Source stage:**

- Source provider: GitLab (or GitHub)
- Repository: your-repo
- Branch: main
- Change detection: GitLab webhooks

**Build stage:**

- Build provider: AWS CodeBuild
- Project name: student-management-frontend-build

**Deploy stage:**

- Deploy provider: Amazon S3 (for frontend artifacts)
- Or: AWS Amplify (if using Amplify)

---

## Summary

Through this section, you have learned how to set up an automated CI/CD process for the Serverless Student Management System using AWS services such as S3, CodeBuild, and CodePipeline. You have practiced creating an S3 bucket to store artifacts, configuring build projects for the frontend and backend, as well as building a pipeline to automate the entire build, test, and deploy process. Applying CI/CD helps ensure the system is always updated, deployed quickly, minimizes manual errors, and improves software development efficiency.
