---
title: "Week 11 Worklog"
date: "2025-11-17"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Develop Post/Comment API for classroom interaction.
* Integration Testing for all APIs.
* Write API Documentation (OpenAPI/Swagger).

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Design Post/Comment schema: <br>&emsp; + DynamoDB table structure <br>&emsp; + Parent-child relationship for comments <br>&emsp; + Timestamp and ordering | 17/11/2025 | 17/11/2025 | [DynamoDB Hierarchical Data](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html) |
| 3   | - Develop API Create Post: <br>&emsp; + `POST /classes/{class_id}/posts` <br>&emsp; + Lambda handler for lecturer posts <br>&emsp; + Content validation | 18/11/2025 | 18/11/2025 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 4   | - Develop API Comment (if any): <br>&emsp; + Reply to post <br>&emsp; + Nested comments structure <br>&emsp; + Permission checking (Lecturer vs Student) | 19/11/2025 | 19/11/2025 | [Amazon Cognito Authorization](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-user-groups.html) |
| 5   | - Integration Testing: <br>&emsp; + Test all API endpoints <br>&emsp; + End-to-end testing <br>&emsp; + Performance testing <br>&emsp; + Fix bugs | 20/11/2025 | 20/11/2025 | [AWS SAM Testing](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-test-and-debug.html) |
| 6   | - API Documentation: <br>&emsp; + OpenAPI/Swagger specification <br>&emsp; + Request/Response examples <br>&emsp; + Error codes documentation <br>&emsp; + Postman collection | 21/11/2025 | 21/11/2025 | [API Gateway Export](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-export-api.html) |

### Week 11 Achievements:

* Completed Post/Comment API:
  ```
  POST /classes/{class_id}/posts → Create Post (by Lecturer)
  ```

* Summary of all completed Backend APIs:

  | API | Method | Endpoint | Description |
  |-----|--------|----------|-------------|
  | Create Class | POST | `/lecturer/classes` | Create class (with limit) |
  | List Classes | GET | `/lecturer/classes` | List classes |
  | Edit Class | PUT | `/lecturer/classes/{id}` | Update class |
  | Deactivate Class | DELETE | `/lecturer/classes/{id}` | Soft delete (status: 1→0) |
  | List Students | GET | `/lecturer/students/{class_id}` | List students in class |
  | Create Assignment | POST | `/lecturer/assignments` | Create assignment |
  | List Assignments | GET | `/lecturer/classes/{class_id}/assignments` | List assignments |
  | Edit Assignment | PUT | `/lecturer/assignments/{id}` | Update assignment |
  | Delete Assignment | DELETE | `/lecturer/assignments/{id}` | Delete assignment |
  | Update Grades | POST | `/lecturer/assignments/{assignment_id}/update-grades` | Grade/edit grades |
  | Create Post | POST | `/classes/{class_id}/posts` | Create post |

* Integration Testing completed:
  * Unit tests: 95% coverage
  * Integration tests passed
  * API Gateway + Lambda + DynamoDB working

* API Documentation:
  * OpenAPI 3.0 specification
  * Postman collection export




