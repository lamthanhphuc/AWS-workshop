---
title : "Dọn dẹp tài nguyên"
date: "2025-12-07"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng quan

Sau khi hoàn thành workshop **Serverless Student Management System**, bước cuối cùng là **dọn dẹp toàn bộ tài nguyên AWS** để tránh phát sinh chi phí không mong muốn.

Trang này hướng dẫn bạn xóa lần lượt các dịch vụ đã tạo:  
**DynamoDB, Lambda, API Gateway, Cognito, S3, EventBridge, SES, AppSync, Amplify, CloudWatch Logs**, và các tài nguyên khác.

---

# 1. Xóa bảng DynamoDB

Truy cập **DynamoDB Console → Tables**:

- Chọn bảng:  
  - `Student-Management-Database`
- Actions → **Delete Table**
- Confirm

> ⚠️ Lưu ý: Xóa bảng là không thể khôi phục.

---

# 2. Xóa Lambda Functions

Vào **AWS Lambda Console → Functions** và xóa:

- Các hàm API Backend (auth, student, lecturer, admin)
- Các hàm Event-driven
- Hàm gửi Email (SES Handler)
- Hàm ML (Personalize Integration)
- Hàm AppSync resolver (nếu có)

> Nếu dùng Lambda Layers → xóa cả Layer.

---

# 3. Xóa API Gateway

### Đối với REST API

Vào **API Gateway Console → APIs → REST APIs**:

- Chọn API backend chính
- Actions → **Delete**

### Đối với WebSocket / Realtime (nếu dùng AppSync Chat)

- Xóa WebSocket APIs (nếu triển khai)

---

# 4. Xóa Cognito User Pool

Vào **Cognito Console → User Pools**:

- Chọn User Pool (ví dụ: `StudentUserPool`)
- Delete

Đừng quên xóa cả:

- App Client
- Domain Cognito (nếu có custom domain)

---

# 5. Xóa S3 Buckets

Bao gồm các bucket sau:

- Bucket chứa file frontend build (Amplify/S3 website)
- Bucket lưu hình ảnh sinh viên
- Bucket lưu assignment submissions
- Bucket training dataset của Personalize (nếu có)

Thực hiện:

1. Empty Bucket  
2. Delete Bucket

> S3 không cho xóa bucket nếu còn file.

---

# 6. Xóa Amplify Project (Frontend)

Vào **Amplify Console → Apps**:

- Chọn App Frontend
- Actions → **Delete App**

Tự động xóa:

- Backend Environment  
- Branch configs  
- Build logs  
- Hosting resources  

---

# 7. Xóa AppSync (nếu dùng Realtime Chat)

Vào **AppSync Console → APIs**:

- Chọn `StudentChatAPI` (hoặc tên bạn đặt)
- Delete API

---

# 8. Xóa EventBridge

### Xóa Event Bus

EventBridge Console → Event Buses:

- Xóa `student-event-bus`

### Xóa Rules

- Xóa rule gửi email  
- Xóa rule training ML  
- Xóa scheduler nightly update  

---

# 9. Xóa SES cấu hình

Trong **SES Console**:

- Delete verified identity (email hoặc domain)
- Delete custom MAIL FROM domain (nếu có)
- Xóa configuration sets (nếu đã tạo)
- Kiểm tra hàng chờ gửi email

> SES trong sandbox không mất phí nhưng phải dọn để tránh lỗi sau này.

---

# 10. Xóa Personalize (ML Ranking)

Trong **Amazon Personalize Console**:

- Xóa:
  - Dataset Group
  - Datasets
  - Solutions
  - Campaigns  
  - Event Tracker  
  - Batch Inference Jobs  

> Các tài nguyên Personalize rất tốn phí, cần xóa sạch.

---

# 11. Xóa CloudWatch Logs

Vào **CloudWatch Logs → Log Groups**:

Xóa:

- `/aws/lambda/...`  
- `/aws/appsync/...`  
- `/aws/events/...`  
- `/aws/api-gateway/...`  

---

# 12. Xóa IAM Roles & Policies tự tạo

Xóa các role:

- Lambda Execution Roles  
- AppSync role  
- SES sending role  
- Personalize import job role  
- DynamoDB access roles  
- Amplify service role  

> ⚠️ Không xóa các AWS-managed roles.

---

# 15. Tổng kết

Trang **“Dọn dẹp tài nguyên”** giúp bạn:

- Xóa toàn bộ dịch vụ đã sử dụng trong workshop  
- Ngăn chặn phát sinh chi phí không mong muốn  
- Dọn môi trường sạch sẽ để chuẩn bị cho workshop tiếp theo  

Đây là bước cuối cùng của Workshop **Serverless Student Management System**.

---
