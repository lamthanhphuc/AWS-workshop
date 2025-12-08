---
title: "Triển khai Backend"
date: "2025-12-07"
weight: 3
chapter: false
pre: "<b> 5.3. </b>"
---

# Xây dựng Backend Serverless với AWS Lambda, API Gateway, DynamoDB và Cognito

## Tổng quan

Phần này hướng dẫn triển khai backend cho hệ thống quản lý sinh viên serverless trên AWS. Bạn sẽ sử dụng các dịch vụ chủ lực như DynamoDB để lưu trữ dữ liệu, Lambda để xử lý logic nghiệp vụ, API Gateway để kết nối giữa frontend và backend, và Cognito để xác thực người dùng. Quy trình thực hiện bao gồm thiết kế bảng dữ liệu, cấu hình xác thực, xây dựng ứng dụng backend với Java Spring Boot, đóng gói và triển khai lên Lambda, cũng như cấu hình API Gateway để phục vụ các nghiệp vụ quản lý sinh viên một cách bảo mật, tự động hóa và dễ mở rộng.

![Amplify Architecture](/images/5-Workshop/5.3-Backend/architecture.png)

## Nội dung

1. [Amazon DynamoDB](5.3.1-AmazonDynamoDB/)
2. [Amazon Cognito](5.3.2-AmazonCognito/)
3. [Amazon S3](5.3.3-AmazonS3/)
4. [LAMBDA + API GATEWAY](5.3.4-LAMBDA&APIGATEWAY/)
