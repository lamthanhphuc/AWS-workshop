---
title: "Dọn dẹp tài nguyên"
date: 2025-12-08
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

Chúc mừng bạn đã hoàn thành việc deploy toàn bộ hệ thống Student Management Serverless!
Để tránh phát sinh chi phí không cần thiết trên tài khoản AWS của bạn, hãy thực hiện các bước **Clean up** dưới đây theo thứ tự từ trên xuống dưới (ngược lại thứ tự tạo).

### 1. Xóa CodePipeline & CodeBuild Projects

- Vào **CodePipeline** → Chọn pipeline `student-management-pipeline` → **Release change** (nếu đang chạy) → **Delete**
- Vào **CodeBuild** → Xóa 2 project:
  - `student-management-frontend-build`
  - `student-management-backend-build`

### 2. Xóa CloudFront Distribution

- Vào **CloudFront** → Chọn distribution của bạn → **Disable** → Chờ ~10–15 phút → **Delete**
- (Không xóa được ngay nếu vẫn đang In Progress → phải Disable trước)

### 3. Xóa Amplify App (nếu dùng Amplify Console)

- Vào **AWS Amplify** → Chọn app → **Actions** → **Delete app** → Gõ `delete` để xác nhận

### 4. Xóa API Gateway

- Vào **API Gateway** → Chọn `student-management-api` (HTTP API) → **Delete**

### 5. Xóa Lambda Function

- Vào **Lambda** → Chọn function `student-management-api` → **Actions** → **Delete**

### 6. Xóa AppSync API (nếu đã tạo phần real-time chat)

- Vào **AppSync** → Chọn `student-management-chat` → **Delete**

### 7. Xóa Cognito User Pool

- Vào **Cognito** → **User Pools** → Chọn pool của bạn → **Delete user pool**
  > Lưu ý: Nếu còn user nào đang sign-in, bạn cần force delete hoặc xóa user trước.

### 8. Xóa DynamoDB Tables

- Vào **DynamoDB** → **Tables** → Xóa lần lượt 4 table:
  - `student-management-users`
  - `student-management-classes`
  - `student-management-subjects`
  - `student-management-notifications`
  - (Nếu có thêm table Messages cho AppSync → xóa luôn)

### 9. Xóa WAF Web ACL

- Vào **WAF & Shield** → **Web ACLs** → Region **Global (CloudFront)** → Chọn `student-management-waf` → **Delete**

### 10. Xóa Route53 Hosted Zone (nếu bạn tạo mới)

- Vào **Route53** → **Hosted zones** → Chọn domain `yourdomain.com` → **Delete hosted zone**
  > Chỉ xóa nếu bạn tạo hosted zone mới trong workshop, không xóa nếu là domain thật đang dùng!

### 11. Xóa ACM Certificate (nếu tạo trong us-east-1)

- Vào **Certificate Manager** → Region **us-east-1** → Chọn certificate → **Delete**

### 12. Xóa S3 Buckets (Artifacts & Amplify nếu còn)

- Xóa bucket:
  - `student-management-artifacts-{account-id}`
  - Các bucket Amplify tự tạo (thường có dạng `amplify-...`)
    > Trước khi xóa: **Empty bucket** trước → rồi mới Delete

### 13. Xóa CloudWatch Log Groups (tùy chọn, để gọn tài khoản)

- Vào **CloudWatch** → **Log groups** → Xóa:
  - `/aws/lambda/student-management-api`
  - `/aws/apigateway/...`
  - Các log group của CodeBuild/CodePipeline nếu muốn

### 14. Xóa IAM Roles/Policies (tùy chọn nhưng nên dọn)

- Tìm và xóa các role tự tạo:
  - Role của Lambda
  - Role của CodePipeline/CodeBuild
  - Role service của Amplify (nếu có)

Sau khi hoàn thành các bước trên, tài khoản AWS của bạn sẽ không còn phát sinh chi phí nào từ workshop này nữa (ngoại trừ một vài cent rất nhỏ có thể còn trong billing cycle hiện tại).
