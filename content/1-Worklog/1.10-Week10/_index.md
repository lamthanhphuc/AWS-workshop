---
title: "Week 10 Worklog"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Develop Backend APIs for Assignment Management (CRUD).
* Develop API to Create/Update Grade (grading and editing grades).
* Build business logic and validation.

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Design Assignment schema: <br>&emsp; + DynamoDB table structure <br>&emsp; + Relationship with Class and Student <br>&emsp; + Grade storage design | 10/11/2025 | 10/11/2025 | [DynamoDB Design Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-modeling-nosql.html) |
| 3   | - Develop API Create Assignment: <br>&emsp; + `POST /lecturer/assignments` <br>&emsp; + Lambda handler <br>&emsp; + Validate assignment data <br>&emsp; + Link to class_id | 11/11/2025 | 11/11/2025 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 4   | - Develop API List & Edit Assignment: <br>&emsp; + `GET /lecturer/classes/{class_id}/assignments` <br>&emsp; + `PUT /lecturer/assignments/{id}` <br>&emsp; + Query and update operations | 12/11/2025 | 12/11/2025 | [API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html) |
| 5   | - Develop API Delete Assignment: <br>&emsp; + `DELETE /lecturer/assignments/{id}` <br>&emsp; + Cascade delete grades <br>&emsp; + Authorization check | 13/11/2025 | 13/11/2025 | [DynamoDB Transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html) |
| 6   | - Develop API Create/Update Grade: <br>&emsp; + `POST /lecturer/assignments/{assignment_id}/update-grades` <br>&emsp; + Batch update grades for multiple students <br>&emsp; + Validation score range <br>&emsp; + Unit testing | 14/11/2025 | 14/11/2025 | [DynamoDB BatchWriteItem](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.BatchOperations) |

### Week 10 Achievements:

* Designed Assignment schema:
  * Assignments table with composite key
  * Grade embedded in student-assignment relationship
  * GSI for query by class_id

* Completed Assignment Management APIs:
  ```
  POST   /lecturer/assignments                    → Create Assignment
  GET    /lecturer/classes/{class_id}/assignments → List Assignments
  PUT    /lecturer/assignments/{id}               → Edit Assignment
  DELETE /lecturer/assignments/{id}               → Delete Assignment
  ```

* Completed Grade API:
  ```
  POST /lecturer/assignments/{assignment_id}/update-grades
  → Create/Update Grade (merge grading and editing grades)
  ```

* Implementation details:
  * Batch write for bulk grade updates
  * Score validation (0-10 or custom range)
  * Cascade delete when deleting assignment
  * Audit trail for grade changes




