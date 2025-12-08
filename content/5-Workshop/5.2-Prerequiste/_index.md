---
title: "Environment Preparation & AWS Account Setup"
date: "2025-12-07"
weight: 2
chapter: false
pre: "<b> 5.2. </b>"
---


To successfully deploy the **Serverless Student Management System** Workshop, it is necessary to fully prepare the technical environment, AWS account and platform services according to the serverless – event-driven architecture.
This page guides the essential steps before starting to develop the backend, frontend and event components.

---

## Prepare AWS account

### Create and configure AWS account
- Create a personal AWS account or use AWS Educate/AWS Academy.
- Activate **AWS Free Tier** to optimize costs during the workshop period.
- Enable **MFA** to secure the root account.
- Create **IAM Users** for team members and assign roles according to the **Least Privilege** principle.

![IAM User](/images/5-Workshop/5.2-Prerequisite/IAMUser.png)  
*Figure 1: Sample IAM users.*

---

## Setting up IAM Permissions

To fully deploy the project's serverless system, the IAM User needs permission to operate with:

- AWS Lambda
- DynamoDB
- API Gateway
- Cognito
- AppSync
- EventBridge
- SES
- S3
- CloudWatch
- Amplify
- IAM (limited to PassRole and Lambda role creation operations)

![Permissions](/images/5-Workshop/5.2-Prerequisite/Permissions.png)
*Figure 2: Sample Permissions.*

 An example of a broad policy for workshop purposes:

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
        "appsync:*",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:PutRolePolicy",
        "iam:AttachRolePolicy",
        "iam:DeleteRolePolicy",
        "iam:PassRole",
        "events:*",
        "ses:*",
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
