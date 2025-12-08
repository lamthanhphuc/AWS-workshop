---
title: "Worklog Tuần 10"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Phát triển Backend APIs cho Assignment Management (CRUD).
* Phát triển API Create/Update Grade (chấm điểm và sửa điểm).
* Xây dựng business logic và validation.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Thiết kế Assignment schema: <br>&emsp; + DynamoDB table structure <br>&emsp; + Relationship với Class và Student <br>&emsp; + Grade storage design | 10/11/2025 | 10/11/2025 | [DynamoDB Design Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-modeling-nosql.html) |
| 3   | - Phát triển API Create Assignment: <br>&emsp; + `POST /lecturer/assignments` <br>&emsp; + Lambda handler <br>&emsp; + Validate assignment data <br>&emsp; + Link to class_id | 11/11/2025 | 11/11/2025 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 4   | - Phát triển API List & Edit Assignment: <br>&emsp; + `GET /lecturer/classes/{class_id}/assignments` <br>&emsp; + `PUT /lecturer/assignments/{id}` <br>&emsp; + Query và update operations | 12/11/2025 | 12/11/2025 | [API Gateway REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html) |
| 5   | - Phát triển API Delete Assignment: <br>&emsp; + `DELETE /lecturer/assignments/{id}` <br>&emsp; + Cascade delete grades <br>&emsp; + Authorization check | 13/11/2025 | 13/11/2025 | [DynamoDB Transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transactions.html) |
| 6   | - Phát triển API Create/Update Grade: <br>&emsp; + `POST /lecturer/assignments/{assignment_id}/update-grades` <br>&emsp; + Batch update grades cho nhiều students <br>&emsp; + Validation score range <br>&emsp; + Unit testing | 14/11/2025 | 14/11/2025 | [DynamoDB BatchWriteItem](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.BatchOperations) |

### Kết quả đạt được tuần 10:

* Thiết kế Assignment schema:
  * Table Assignments với composite key
  * Grade embedded trong student-assignment relationship
  * GSI cho query by class_id

* Hoàn thành Assignment Management APIs:
  ```
  POST   /lecturer/assignments                    → Create Assignment
  GET    /lecturer/classes/{class_id}/assignments → List Assignments
  PUT    /lecturer/assignments/{id}               → Edit Assignment
  DELETE /lecturer/assignments/{id}               → Delete Assignment
  ```

* Hoàn thành Grade API:
  ```
  POST /lecturer/assignments/{assignment_id}/update-grades
  → Create/Update Grade (hợp nhất chấm điểm và sửa điểm)
  ```

* Implementation details:
  * Batch write cho bulk grade updates
  * Score validation (0-10 hoặc custom range)
  * Cascade delete khi xóa assignment
  * Audit trail cho grade changes




