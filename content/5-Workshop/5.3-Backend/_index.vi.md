---
title : "Triển khai Backend: DynamoDB, Lambda, API Gateway, Cognito"
date: "2006-01-02"
weight : 3
chapter : false
pre : "<b> 5.3. </b>"
---

#  Thiết lập các dịch vụ nền tảng

Thiết lập ban đầu các dịch vụ cốt lõi được sử dụng xuyên suốt dự án.

---

## 🔷 Amazon DynamoDB

Dùng để lưu trữ:

- Thông tin sinh viên
- Lớp học, môn học
- Giảng viên
- Điểm số
- Dữ liệu tương tác phục vụ mô hình ML
- Lịch sử chat, sự kiện hệ thống

### 🗂 **Cấu trúc bảng DynamoDB (Single-Table Design)**

**Bảng:** `Student-Management-Database`

| Thành phần | Ý nghĩa |
|-----------|---------|
| **Partition Key (PK)** | USER#, CLASS#, SUBJECT#, … |
| **Sort Key (SK)** | PROFILE, INFO, STUDENT#, … |
| **GSI1PK** | ROLE#, TYPE#, USER#… |
| **GSI1SK** | NAME#, CLASS#, SUBJECT#… |

**Billing mode:** On-Demand (khuyến nghị, tránh lỗi throttling và tối ưu chi phí)

![DynamoDB](/images/5-Workshop/5.2-Prerequisite/DynamoDB.png)

---

## 🔷 Amazon Cognito

Dùng để xác thực người dùng (Student, Lecturer, Admin).

### ✔ Các bước thiết lập:

- Tạo **User Pool**
- Tạo **App Client (No secret)**
- Cho phép đăng nhập bằng **email**
- Tạo nhóm (Group): `student`, `lecturer`, `admin`  
- Tích hợp JWT vào API Gateway (Cognito Authorizer)

![Cognito](/images/5-Workshop/5.2-Prerequisite/Cognito.png)

---

## 🔷 Amazon S3

Dùng cho:

- Lưu ảnh đại diện (avatar)
- Tài liệu lớp học
- Build artifacts frontend
- Deploy website

![S3](/images/5-Workshop/5.2-Prerequisite/S3.png)

---

## 🔷 Amazon API Gateway (REST API)

### AUTHENTICATION
| Method | Endpoint               | Description                |
|--------|------------------------|----------------------------|
| POST   | /auth/login            | Login bằng tài khoản thường |
| POST   | /auth/google           | Login bằng Google          |
| POST   | /auth/logout           | Logout                     |
| POST   | /auth/change-password  | Đổi mật khẩu               |
| POST   | /auth/forgot-password  | Quên mật khẩu              |

### USER PROFILE
| Method | Endpoint   | Description   |
|--------|------------|---------------|
| GET    | /profile   | Xem profile   |
| PATCH  | /profile   | Chỉnh sửa profile |

### NOTIFICATIONS
| Method | Endpoint                    | Description |
|--------|------------------------------|-------------|
| GET    | /notifications               | Thông báo chung (dashboard) |
| POST   | /lecturer/notifications/email | GV gửi thông báo email |
| GET    | /student/notifications       | Thông báo riêng của Student |

### ADMIN DASHBOARD
| Method | Endpoint                | Description                  |
|--------|--------------------------|------------------------------|
| GET    | /admin/all-students     | Tổng số sinh viên           |
| GET    | /admin/active-classes   | Tổng lớp đang hoạt động     |

### SEARCH & FILTER
| Method | Endpoint   | Description |
|--------|------------|-------------|
| GET    | /search    | Search đa loại dữ liệu theo role |

### EXPORT DATA
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /export  | Xuất dữ liệu |

### ADMIN – USER MANAGEMENT
| Method | Endpoint              | Description |
|--------|------------------------|-------------|
| POST   | /admin/register        | Tạo user mới |
| GET    | /admin/users           | Liệt kê user |
| PATCH  | /admin/users/{id}      | Deactivate user (status=0) |

### ADMIN – SUBJECT MANAGEMENT
| Method | Endpoint                     | Description |
|--------|-------------------------------|-------------|
| POST   | /admin/subjects               | Tạo môn học |
| GET    | /admin/subjects               | Liệt kê môn học |
| PATCH  | /admin/subjects/{id}          | Chỉnh sửa môn học |
| PATCH  | /admin/subjects/ban/{id}      | Deactivate subject (status=0) |

### ADMIN – CLASS MANAGEMENT
| Method | Endpoint               | Description |
|--------|-------------------------|-------------|
| POST   | /admin/classes          | Tạo lớp học |
| GET    | /admin/classes          | Liệt kê lớp |
| PUT    | /admin/classes/{id}     | Chỉnh sửa lớp |
| PATCH  | /admin/classes/{id}     | Deactivate class |

### ENROLL MANAGEMENT
| Method | Endpoint        | Description       |
|--------|------------------|-------------------|
| POST   | /admin/enroll    | Enroll sinh viên |

### AUDIT LOGS
| Method | Endpoint           | Description     |
|--------|----------------------|-----------------|
| GET    | /admin/audit-logs    | Xem audit logs |

### RANKING & ANALYTICS
| Method | Endpoint           | Description |
|--------|----------------------|-------------|
| GET    | /admin/ranking       | Ranking toàn hệ thống |

### LECTURER – CLASSES
| Method | Endpoint                   | Description |
|--------|------------------------------|-------------|
| GET    | /lecturer/classes            | Liệt kê lớp |
| PUT    | /lecturer/classes/{id}       | Chỉnh sửa lớp |
| DELETE | /lecturer/classes/{id}       | Deactivate class |

### LECTURER – STUDENTS
| Method | Endpoint                         | Description |
|--------|-----------------------------------|-------------|
| GET    | /lecturer/students/{class_id}     | Danh sách HS trong lớp |

### LECTURER – ASSIGNMENTS
| Method | Endpoint                                         | Description |
|--------|---------------------------------------------------|-------------|
| POST   | /lecturer/assignments                             | Tạo assignment |
| GET    | /lecturer/classes/{class_id}/assignments          | Liệt kê assignments |
| PUT    | /lecturer/assignments/{id}                        | Chỉnh sửa assignment |
| DELETE | /lecturer/assignments/{id}                        | Xoá assignment |
| POST   | /lecturer/assignments/{assignment_id}/update-grades | Tạo/Sửa điểm |
| GET    | /lecturer/assignments/get-submissions             | Xem submissions |

### LECTURER – RANKING
| Method | Endpoint                       | Description |
|--------|----------------------------------|-------------|
| GET    | /lecturer/ranking/{class_id}     | Ranking trong lớp |

### STUDENT – CLASSES
| Method | Endpoint                               | Description |
|--------|-----------------------------------------|-------------|
| POST   | /student/enroll                         | Enroll / Unenroll |
| GET    | /student/classes/class-enrolled         | Danh sách lớp đang học |
| GET    | /student/search                         | Tìm lớp / giảng viên |

### STUDENT – ASSIGNMENTS
| Method | Endpoint                                    | Description |
|--------|----------------------------------------------|-------------|
| GET    | /student/assignments                        | Danh sách assignment |
| POST   | /student/submit                             | Nộp bài |
| GET    | /student/assignments/get-submissions        | Xem submissions cá nhân |

### STUDENT – RANKING
| Method | Endpoint                          | Description |
|--------|------------------------------------|-------------|
| GET    | /student/ranking/{class_id}        | Ranking lớp |

# 🎯 Tổng kết

Trang **“Triển khai Backend”** mô tả toàn bộ các dịch vụ AWS cốt lõi bạn cần thiết lập trước khi bắt đầu phát triển chức năng:

- DynamoDB (Single-table design)
- Cognito (Authentication)
- S3 (Storage & Frontend deploy)
- API Gateway + Lambda (Backend serverless)

Các thành phần này tạo nền tảng để xây dựng hệ thống **Serverless – Realtime – Event-Driven** của **Student Management System**.

---
