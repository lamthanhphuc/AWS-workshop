---
title: "Clean Up"
date: 2025-12-08
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

Congratulations on successfully deploying the entire Serverless Student Management system!  
To avoid unnecessary charges on your AWS account, follow the **Clean Up** steps below in _top-to-bottom order_ (the reverse order of creation).

### 1. Delete CodePipeline & CodeBuild Projects

- Go to **CodePipeline** → Select the pipeline `student-management-pipeline` → **Release change** (if it’s running) → **Delete**
- Go to **CodeBuild** → Delete the 2 projects:
  - `student-management-frontend-build`
  - `student-management-backend-build`

### 2. Delete CloudFront Distribution

- Go to **CloudFront** → Select your distribution → **Disable** → Wait ~10–15 minutes → **Delete**
- (You cannot delete immediately if it’s still _In Progress_ → you must Disable first)

### 3. Delete Amplify App (if using Amplify Console)

- Go to **AWS Amplify** → Select the app → **Actions** → **Delete app** → Type `delete` to confirm

### 4. Delete API Gateway

- Go to **API Gateway** → Select `student-management-api` (HTTP API) → **Delete**

### 5. Delete Lambda Function

- Go to **Lambda** → Select the function `student-management-api` → **Actions** → **Delete**

### 6. Delete AppSync API (if you created the real-time chat)

- Go to **AppSync** → Select `student-management-chat` → **Delete**

### 7. Delete Cognito User Pool

- Go to **Cognito** → **User Pools** → Select your pool → **Delete user pool**
  > Note: If any user is still signed in, you must force delete or delete the users first.

### 8. Delete DynamoDB Tables

- Go to **DynamoDB** → **Tables** → Delete the following 4 tables:
  - `student-management-users`
  - `student-management-classes`
  - `student-management-subjects`
  - `student-management-notifications`
  - (If you have a Messages table for AppSync → delete it as well)

### 9. Delete WAF Web ACL

- Go to **WAF & Shield** → **Web ACLs** → Region **Global (CloudFront)** → Select `student-management-waf` → **Delete**

### 10. Delete Route53 Hosted Zone (if you created one)

- Go to **Route53** → **Hosted zones** → Select your domain `yourdomain.com` → \*\*Delete hosted zone`
  > Only delete if you created a new hosted zone during the workshop — do NOT delete if it’s a real domain you're using!

### 11. Delete ACM Certificate (if created in us-east-1)

- Go to **Certificate Manager** → Region **us-east-1** → Select the certificate → **Delete**

### 12. Delete S3 Buckets (Artifacts & Amplify if any)

- Delete the buckets:
  - `student-management-artifacts-{account-id}`
  - Amplify-generated buckets (usually named like `amplify-...`)
    > Before deleting: **Empty bucket** → then Delete

### 13. Delete CloudWatch Log Groups (optional, for account cleanliness)

- Go to **CloudWatch** → **Log groups** → Delete:
  - `/aws/lambda/student-management-api`
  - `/aws/apigateway/...`
  - CodeBuild/CodePipeline log groups if desired

### 14. Delete IAM Roles/Policies (optional but recommended)

- Find and delete the roles you created:
  - Lambda execution role
  - CodePipeline/CodeBuild roles
  - Amplify service roles (if any)

After completing the steps above, your AWS account will no longer incur any charges from this workshop (except for a few remaining cents within the current billing cycle).
