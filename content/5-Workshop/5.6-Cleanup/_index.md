---
title : "Resource Cleanup"
date: "2025-12-07"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Overview

After completing the **Serverless Student Management System** workshop, the final step is to **clean up all AWS resources** to avoid unexpected costs.

This page guides you through deleting the services you created:
**DynamoDB, Lambda, API Gateway, Cognito, S3, EventBridge, SES, AppSync, Amplify, CloudWatch Logs**, and other resources.

---

# 1. Delete DynamoDB Table

Go to **DynamoDB Console → Tables**:

- Select table:

- `Student-Management-Database`
- Actions → **Delete Table**
- Confirm

> ⚠️ Note: Deleting a table is irreversible.

---

# 2. Delete Lambda Functions

Go to **AWS Lambda Console → Functions** and delete:

- Backend API functions (auth, student, lecturer, admin)
- Event-driven functions
- Email sending function (SES Handler)
- ML function (Personalize Integration)
- AppSync resolver function (if any)

> If using Lambda Layers → delete the entire Layer.

---

# 3. Delete API Gateway

### For REST API

Go to **API Gateway Console → APIs → REST APIs**:

- Select main backend API

- Actions → **Delete**

### For WebSocket / Realtime (if using AppSync Chat)

- Delete WebSocket APIs (if deployed)

---

# 4. Delete Cognito User Pool

Go to **Cognito Console → User Pools**:

- Select User Pool (e.g. `StudentUserPool`)
- Delete

Don't forget to delete:

- App Client
- Cognito Domain (if you have a custom domain)

---

# 5. Delete S3 Buckets

Includes the following buckets:

- Bucket containing frontend build files (Amplify/S3 website)
- Bucket storing student images
- Bucket storing assignment submissions
- Bucket storing training dataset of Personalize (if available)

Do:

1. Empty Bucket
2. Delete Bucket

> S3 does not allow deleting bucket if there are files.

---

# 6. Delete Amplify Project (Frontend)

Go to **Amplify Console → Apps**:

- Select App Frontend
- Actions → **Delete App**

Automatically delete:

- Backend Environment
- Branch configs
- Build logs
- Hosting resources

---

# 7. Delete AppSync (if using Realtime Chat)

Go to **AppSync Console → APIs**:

- Select `StudentChatAPI` (or your name)
- Delete API

---

# 8. Delete EventBridge

### Delete Event Bus

EventBridge Console → Event Buses:

- Delete `student-event-bus`

### Delete Rules

- Delete email rule
- Delete ML training rule
- Delete nightly update scheduler

---

# 9. Delete SES configuration

In **SES Console**:

- Delete verified identity (email or domain)
- Delete custom MAIL FROM domain (if any)
- Delete configuration sets (if created)
- Check email sending queue

> SES in sandbox is free but must be cleaned to avoid future errors.

---

# 10. Delete Personalize (ML Ranking)

In **Amazon Personalize Console**:

- Delete:
- Dataset Group
- Datasets
- Solutions
- Campaigns
- Event Tracker
- Batch Inference Jobs

> Personalize resources are very expensive, need to be cleaned.

---

#11. Delete CloudWatch Logs

Go to **CloudWatch Logs → Log Groups**:

Erase:

- `/aws/lambda/...`
- `/aws/appsync/...`
- `/aws/events/...`
- `/aws/api-gateway/...`

---

#12. Delete self-created IAM Roles & Policies

Delete roles:

- Lambda Execution Roles
- AppSync role
- SES sending role
- Personalize import job role
- DynamoDB access roles
- Amplify service role

> ⚠️ Do not delete AWS-managed roles.

---

# 13. Summary

The **“Cleanup Resources”** page helps you:

- Delete all services used in the workshop
- Prevent unwanted costs
- Clean up the environment to prepare for the next workshop

This is the final step of the **Serverless Student Management System** Workshop.

---