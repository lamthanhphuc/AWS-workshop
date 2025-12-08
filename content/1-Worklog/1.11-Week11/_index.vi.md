---
title: "Worklog Tuần 11"
date: "2025-11-17"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Phát triển API Post/Comment cho tương tác trong lớp học.
* Integration Testing cho toàn bộ APIs.
* Viết API Documentation (OpenAPI/Swagger).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Thiết kế Post/Comment schema: <br>&emsp; + DynamoDB table structure <br>&emsp; + Parent-child relationship cho comments <br>&emsp; + Timestamp và ordering | 17/11/2025 | 17/11/2025 | [DynamoDB Hierarchical Data](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-adjacency-graphs.html) |
| 3   | - Phát triển API Create Post: <br>&emsp; + `POST /classes/{class_id}/posts` <br>&emsp; + Lambda handler cho lecturer posts <br>&emsp; + Content validation | 18/11/2025 | 18/11/2025 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 4   | - Phát triển API Comment (nếu có): <br>&emsp; + Reply to post <br>&emsp; + Nested comments structure <br>&emsp; + Permission checking (GV vs SV) | 19/11/2025 | 19/11/2025 | [Amazon Cognito Authorization](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-user-groups.html) |
| 5   | - Integration Testing: <br>&emsp; + Test tất cả API endpoints <br>&emsp; + End-to-end testing <br>&emsp; + Performance testing <br>&emsp; + Fix bugs | 20/11/2025 | 20/11/2025 | [AWS SAM Testing](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-test-and-debug.html) |
| 6   | - API Documentation: <br>&emsp; + OpenAPI/Swagger specification <br>&emsp; + Request/Response examples <br>&emsp; + Error codes documentation <br>&emsp; + Postman collection | 21/11/2025 | 21/11/2025 | [API Gateway Export](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-export-api.html) |

### Kết quả đạt được tuần 11:

* Hoàn thành Post/Comment API:
  ```
  POST /classes/{class_id}/posts → Create Post (by Lecturer)
  ```

* Tổng hợp tất cả Backend APIs hoàn thành:

  | API | Method | Endpoint | Mô tả |
  |-----|--------|----------|-------|
  | Create Class | POST | `/lecturer/classes` | Tạo lớp học (có giới hạn) |
  | List Classes | GET | `/lecturer/classes` | Danh sách lớp học |
  | Edit Class | PUT | `/lecturer/classes/{id}` | Cập nhật lớp học |
  | Deactivate Class | DELETE | `/lecturer/classes/{id}` | Soft delete (status: 1→0) |
  | List Students | GET | `/lecturer/students/{class_id}` | DS sinh viên trong lớp |
  | Create Assignment | POST | `/lecturer/assignments` | Tạo bài tập |
  | List Assignments | GET | `/lecturer/classes/{class_id}/assignments` | DS bài tập |
  | Edit Assignment | PUT | `/lecturer/assignments/{id}` | Cập nhật bài tập |
  | Delete Assignment | DELETE | `/lecturer/assignments/{id}` | Xóa bài tập |
  | Update Grades | POST | `/lecturer/assignments/{assignment_id}/update-grades` | Chấm/sửa điểm |
  | Create Post | POST | `/classes/{class_id}/posts` | Tạo bài đăng |

* Integration Testing hoàn thành:
  * Unit tests: 95% coverage
  * Integration tests passed
  * API Gateway + Lambda + DynamoDB working

* API Documentation:
  * OpenAPI 3.0 specification
  * Postman collection export




