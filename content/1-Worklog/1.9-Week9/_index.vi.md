---
title: "Worklog Tuần 9"
date: "2006-01-02"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Phát triển Backend APIs cho Class Management.
* Phát triển API List Students in Class.
* Thiết lập Lambda functions và DynamoDB.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Thiết lập môi trường phát triển: <br>&emsp; + Cấu hình AWS SAM/CDK <br>&emsp; + Tạo DynamoDB tables <br>&emsp; + Setup API Gateway | 03/11/2025 | 03/11/2025 | [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/) |
| 3   | - Phát triển API Create Class: <br>&emsp; + `POST /lecturer/classes` <br>&emsp; + Lambda handler function <br>&emsp; + Input validation <br>&emsp; + Giới hạn số lượng class per lecturer | 04/11/2025 | 04/11/2025 | [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html) |
| 4   | - Phát triển API List & Edit Class: <br>&emsp; + `GET /lecturer/classes` - Query DynamoDB với GSI <br>&emsp; + `PUT /lecturer/classes/{id}` - Update class info <br>&emsp; + Authorization check | 05/11/2025 | 05/11/2025 | [DynamoDB Query Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html) |
| 5   | - Phát triển API Deactivate Class: <br>&emsp; + `DELETE /lecturer/classes/{id}` <br>&emsp; + Soft delete: Update status từ 1 → 0 <br>&emsp; + Cascade logic cho related data | 06/11/2025 | 06/11/2025 | [DynamoDB Update Operations](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html) |
| 6   | - Phát triển API List Students: <br>&emsp; + `GET /lecturer/students/{class_id}` <br>&emsp; + Query students theo class_id <br>&emsp; + Pagination và filtering <br>&emsp; + Unit testing | 07/11/2025 | 07/11/2025 | [DynamoDB Pagination](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.Pagination.html) |

### Kết quả đạt được tuần 9:

* Thiết lập môi trường phát triển:
  * AWS SAM project structure
  * DynamoDB tables: Classes, Students, Enrollments
  * API Gateway với Cognito authorizer

* Hoàn thành Class Management APIs:
  ```
  POST   /lecturer/classes          → Create Class (với limit)
  GET    /lecturer/classes          → List Classes
  PUT    /lecturer/classes/{id}     → Edit Class
  DELETE /lecturer/classes/{id}     → Deactivate Class (soft delete)
  ```

* Hoàn thành Student API:
  ```
  GET /lecturer/students/{class_id} → List Students in Class
  ```

* Implementation details:
  * Soft delete pattern: status field (1=active, 0=inactive)
  * GSI cho query optimization
  * Input validation với JSON Schema
  * Error handling và logging




