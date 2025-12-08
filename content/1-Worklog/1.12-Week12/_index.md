---
title: "Week 12 Worklog"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---

### Week 12 Objectives:

* Deploy Backend to AWS (API Gateway + Lambda + DynamoDB).
* Finalize Proposal document and Architecture diagrams.
* Prepare Demo and Presentation.

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Deploy Backend to AWS: <br>&emsp; + SAM deploy to AWS <br>&emsp; + Configure API Gateway stages <br>&emsp; + Setup Cognito User Pool <br>&emsp; + Environment variables | 24/11/2025 | 24/11/2025 | [AWS SAM Deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) |
| 3   | - Configure Production: <br>&emsp; + Enable CloudWatch logging <br>&emsp; + Setup alarms and monitoring <br>&emsp; + API throttling and quotas <br>&emsp; + Security review | 25/11/2025 | 25/11/2025 | [CloudWatch Monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 4   | - Finalize Proposal document: <br>&emsp; + Update Architecture diagram <br>&emsp; + Estimate actual costs <br>&emsp; + Risk assessment <br>&emsp; + Implementation roadmap | 26/11/2025 | 26/11/2025 | [Proposal Document](../../2-Proposal/) |
| 5   | - Prepare Demo: <br>&emsp; + Test all API endpoints <br>&emsp; + Prepare demo scenarios <br>&emsp; + Record demo video (if needed) <br>&emsp; + Troubleshoot issues | 27/11/2025 | 27/11/2025 | [Postman API Testing](https://www.postman.com/api-platform/api-testing/) |
| 6   | - Prepare Presentation: <br>&emsp; + Slide deck <br>&emsp; + Technical deep-dive <br>&emsp; + Q&A preparation <br>&emsp; + Final review | 28/11/2025 | 28/11/2025 | [AWS Serverless Samples](https://github.com/aws-samples/serverless-patterns) |

### Week 12 Achievements:

* Successfully deployed to AWS:
  * API Gateway endpoint: `https://xxx.execute-api.region.amazonaws.com/prod`
  * Lambda functions deployed and running
  * DynamoDB tables with data
  * Cognito authentication integrated

* Monitoring and Security:
  * CloudWatch dashboards
  * API Gateway throttling: 1000 req/sec
  * Cognito JWT authentication
  * IAM least privilege policies

* **Summary of completed Backend APIs:**

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

* Proposal Document completed:
  * Architecture diagram updated
  * Estimated cost: ~$7-20/month
  * 8 main risks assessed
  * 14-week roadmap

* **Summary of 12-week internship:**
  * Tuần 1-7: Học AWS fundamentals (VPC, EC2, S3, IAM, RDS, Security)
  * Tuần 8-12: Phát triển Backend APIs cho Serverless Student Management System
  * Hoàn thành 11 REST APIs với Lambda + DynamoDB
  * Deploy production-ready system trên AWS




