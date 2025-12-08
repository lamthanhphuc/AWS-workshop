---
title: "Week 9 Worklog"
date: "2025-11-03"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Develop Backend APIs for Class Management.
* Develop API to List Students in Class.
* Set up Lambda functions and DynamoDB.

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Set up development environment: <br>&emsp; + Configure AWS SAM/CDK <br>&emsp; + Create DynamoDB tables <br>&emsp; + Set up API Gateway | 03/11/2025 | 03/11/2025 | [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/) |
| 3   | - Develop API Create Class: <br>&emsp; + `POST /lecturer/classes` <br>&emsp; + Lambda handler function <br>&emsp; + Input validation <br>&emsp; + Limit number of classes per lecturer | 04/11/2025 | 04/11/2025 | [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html) |
| 4   | - Develop API List & Edit Class: <br>&emsp; + `GET /lecturer/classes` - Query DynamoDB with GSI <br>&emsp; + `PUT /lecturer/classes/{id}` - Update class info <br>&emsp; + Authorization check | 05/11/2025 | 05/11/2025 | [DynamoDB Query Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html) |
| 5   | - Develop API Deactivate Class: <br>&emsp; + `DELETE /lecturer/classes/{id}` <br>&emsp; + Soft delete: Update status from 1 → 0 <br>&emsp; + Cascade logic for related data | 06/11/2025 | 06/11/2025 | [DynamoDB Update Operations](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html) |
| 6   | - Develop API List Students: <br>&emsp; + `GET /lecturer/students/{class_id}` <br>&emsp; + Query students by class_id <br>&emsp; + Pagination and filtering <br>&emsp; + Unit testing | 07/11/2025 | 07/11/2025 | [DynamoDB Pagination](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.Pagination.html) |

### Week 9 Achievements:

* Set up development environment:
  * AWS SAM project structure
  * DynamoDB tables: Classes, Students, Enrollments
  * API Gateway with Cognito authorizer

* Completed Class Management APIs:
  ```
  POST   /lecturer/classes          → Create Class (with limit)
  GET    /lecturer/classes          → List Classes
  PUT    /lecturer/classes/{id}     → Edit Class
  DELETE /lecturer/classes/{id}     → Deactivate Class (soft delete)
  ```

* Completed Student API:
  ```
  GET /lecturer/students/{class_id} → List Students in Class
  ```

* Implementation details:
  * Soft delete pattern: status field (1=active, 0=inactive)
  * GSI for query optimization
  * Input validation with JSON Schema
  * Error handling and logging




