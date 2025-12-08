---
title : "Backend Deployment: DynamoDB, Lambda, API Gateway, Cognito"
date: "2025-12-07"
weight : 3
chapter : false
pre : "<b> 5.3. </b>"
---


---

# 1. Amazon DynamoDB

DynamoDB is a serverless NoSQL database that stores all system data:

- Student information
- Classes, subjects
- Lecturers
- Scores
- Chat history, system events
- ML data for recommendations (Personalize)

Spring Boot backend system interacts with DynamoDB via:

- AWS SDK for Java 17
- Spring Data DynamoDB or repository written by the team
- Presigned URL (upload documents, profiles)
- EventBridge (generate academic events → process via Lambda)

---

## **DynamoDB Table Design (Single-Table Design)**

**Table:** `Student-Management-Database`

| Components | Meaning |
|-----------|---------|
| PK | USER#, CLASS#, SUBJECT#, TEACHER#, GRADE# |
| SK | PROFILE, INFO, STUDENT#, SUBJECT#, CLASS# |
| GSI1PK | ROLE#, TYPE#, EMAIL#, CLASS# |
| GSI1SK | NAME#, CREATED_AT#, SUBJECT# |

**Billing mode:** On-Demand → suitable for workshops (no capacity configuration required).

![DynamoDB](/images/5-Workshop/5.2-Prerequisite/DynamoDB.png)

---

## **Deploy DynamoDB via AWS CLI**

### Create table

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
#2. Amazon Cognito

Cognito offers:

- Account management

- Login with email

- JWT Authentication uses Spring Security

- Decentralize rights by Group (student, tutor, admin)

- Backend Spring Boot uses:

- Cognito JWT Filter

- Spring Security @PreAuthorize("hasRole('ADMIN')")

## **Deploy Cognito via AWS CLI**

### Create User Pool
```bash
aws cognito-idp create-user-pool \
  --pool-name Student-App-Pool \
  --auto-verified-attributes email
```
### Create App Client
```bash
aws cognito-idp create-user-pool-client \
  --user-pool-id <UserPoolId> \
  --client-name StudentAppClient \
  --no-generate-secret
  ```

# 3. Amazon S3
S3 is used for:

- Save student avatars

- Class materials

- Build artifacts (React/Vite)

- Deploy website via S3 + CloudFront

- Log & learning assets

## Deploy S3 via AWS CLI
### Create Bucket
```bash
aws s3api create-bucket \
  --bucket aws-sam-cli-managed-default-samclisourcebucket-qsrwrbr9usyq \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1
  ```

# 4. Amazon API Gateway + Lambda
API Gateway plays the role of:

- API Gateway for the entire system

- Cognito Authorizer authentication

- Route to Lambda to process business

- CloudWatch logging

- CORS integration

- Backend deployed via:

- Lambda (Java 17, Maven build)

- API Gateway REST API

- Lambda Function URL (internal)

## Deploy Lambda & API Gateway using AWS SAM

AWS SAM makes it easier to deploy serverless backends by:

- Manage Lambda, API Gateway, IAM Role in a template.yaml file

- Build Java using Maven automatically (sam build)

- Deploy full stack with one command (sam deploy --guided)
### File template.yaml (SAM)
Create files:
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
### Build Lambda using SAM

SAM runs Maven itself, packages the JAR itself:
```bash
sam build
```
### Deploy using SAM
```bash
sam deploy --guided
```

The first time you enter:

- Stack Name: student-management-backend

- Region: ap-southeast-1

- Allow SAM to create IAM roles? → Y

- Save arguments? → Y

After successful deployment, SAM will return:

- API Endpoint

- Lambda ARN

- CloudFormation Stack
# Summary

These components create the foundation for building a Serverless – Realtime – Event-Driven system for Student Management System.