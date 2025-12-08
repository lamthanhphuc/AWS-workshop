---
title: "Worklog Tuần 8"
date: "2025-10-27"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Phân tích yêu cầu và thiết kế kiến trúc hệ thống Serverless Student Management.
* Thiết kế API endpoints cho module Class Management.
* Thiết kế cấu trúc DynamoDB tables.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Phân tích yêu cầu dự án: <br>&emsp; + Xác định actors: Lecturer, Student <br>&emsp; + Liệt kê use cases chính <br>&emsp; + Vẽ Use Case Diagram | 27/10/2025 | 27/10/2025 | [Proposal Document](../../2-Proposal/) |
| 3   | - Thiết kế kiến trúc hệ thống: <br>&emsp; + Vẽ Architecture Diagram <br>&emsp; + Xác định AWS services sử dụng <br>&emsp; + Thiết kế data flow | 28/10/2025 | 28/10/2025 | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) |
| 4   | - Thiết kế API endpoints cho Class Management: <br>&emsp; + `POST /lecturer/classes` - Create Class <br>&emsp; + `GET /lecturer/classes` - List Classes <br>&emsp; + `PUT /lecturer/classes/{id}` - Edit Class <br>&emsp; + `DELETE /lecturer/classes/{id}` - Deactivate Class | 29/10/2025 | 29/10/2025 | [REST API Best Practices](https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-develop.html) |
| 5   | - Thiết kế DynamoDB Tables: <br>&emsp; + Table `Classes`: PK, SK, GSI design <br>&emsp; + Table `Students`: Schema và indexes <br>&emsp; + Single-table design patterns | 30/10/2025 | 30/10/2025 | [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html) |
| 6   | - Thiết kế API Request/Response: <br>&emsp; + Định nghĩa JSON schemas <br>&emsp; + Error handling patterns <br>&emsp; + Validation rules | 31/10/2025 | 31/10/2025 | [API Gateway Request Validation](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-request-validation.html) |

### Kết quả đạt được tuần 8:

* Hoàn thành phân tích yêu cầu:
  * Xác định 2 actors chính: Lecturer và Student
  * 15+ use cases cho hệ thống
  * Use Case Diagram hoàn chỉnh

* Thiết kế kiến trúc Serverless:
  * API Gateway + Lambda + DynamoDB
  * Cognito cho authentication
  * AppSync cho real-time features

* Thiết kế Class Management APIs:
  * `POST /lecturer/classes` - Tạo lớp học mới (có giới hạn số lượng)
  * `GET /lecturer/classes` - Danh sách lớp học của lecturer
  * `PUT /lecturer/classes/{id}` - Cập nhật thông tin lớp
  * `DELETE /lecturer/classes/{id}` - Soft delete (status: 1 → 0)

* Thiết kế DynamoDB schema:
  * Single-table design cho performance
  * GSI cho query patterns
  * Status field cho soft delete




