---
title : "CloudWatch"
date: "2025-12-07"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Overview

This section guides you through using Amazon CloudWatch to monitor, alert, and manage services in the Serverless Student Management System on AWS. You will learn how to create log groups for Lambda, set up alarms for Lambda errors and performance, API Gateway, and build a visual dashboard to track the entire system's operations. Proactive monitoring helps detect problems early, optimize operations, and effectively control AWS costs.

---

## PART 8: CLOUDWATCH (Monitoring)

### 1. Create Log Groups

Lambda automatically creates log group: `/aws/lambda/student-management-api`

### 2. Create Alarms

1. Go to **CloudWatch → Alarms → Create alarm**

**Lambda Error Alarm:**
```
Metric: AWS/Lambda → Errors
Function: student-management-api
Statistics: Sum
Period: 5 minutes
Threshold: > 5
```

**API Gateway 5xx Alarm:**
```
Metric: AWS/ApiGateway → 5XXError
Statistics: Sum
Period: 5 minutes
Threshold: > 10
```

### 3. Create Dashboard

1. Go to **CloudWatch → Dashboards → Create dashboard**
2. Add widgets:
- Lambda invocations
- Lambda errors
- Lambda duration
- API Gateway requests
- DynamoDB read/write capacity

---

#### Summary

Through this section, you have learned how to use Amazon CloudWatch to monitor, alert, and effectively manage AWS services in a serverless system. Setting up log groups, alarms, and dashboards helps you detect problems early, optimize operations, control costs, and ensure the system always operates stably and securely. This is an important step to maintain service quality and support long-term operations on the AWS platform.