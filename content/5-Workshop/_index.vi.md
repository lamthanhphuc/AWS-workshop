---
title: "Workshop"
date: "2025-12-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Serverless Student Management System trên AWS

#### Tổng quan

**Serverless Student Management Platform** là giải pháp quản lý sinh viên dựa trên đám mây, tối ưu chi phí và mở rộng linh hoạt nhờ sử dụng các dịch vụ **AWS Serverless** (Lambda, DynamoDB, AppSync, EventBridge...). Giải pháp cung cấp dashboard realtime, chat bài tập, phân tích ranking bằng machine learning, gửi thông báo tự động, và vận hành hoàn toàn bằng kiến trúc sự kiện.

Các dịch vụ core: API Gateway (REST), Lambda xử lý logic nghiệp vụ, DynamoDB lưu dữ liệu, AppSync (GraphQL) & Amplify (frontend React), Cognito (xác thực người dùng), SES (gửi email), Personalize (ML ranking), EventBridge (điều phối sự kiện), CloudWatch (giám sát), kết hợp quy trình DevOps CI/CD hiện đại.


#### Nội dung

1. [Tổng quan và kiến trúc hệ thống](5.1-Overview/)
2. [Chuẩn bị môi trường & tài khoản AWS](5.2-Prerequiste/)
3. [Triển khai Backend: DynamoDB, Lambda, API Gateway, Cognito](5.3-Backend/)
4. [Xây dựng Frontend: Amplify, AppSync, Realtime Chat](5.4-Frontend/)
5. [Event-driven, Email Notification & ML Ranking](5.5-Event-ML/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)

#### Mục tiêu trải nghiệm

+ Nắm kiến trúc serverless đa lớp vận hành thực tế trên AWS.
+ Hiểu mô hình phân quyền với Cognito, xử lý realtime với AppSync.
+ Trực tiếp build CRUD, chat, ranking sinh viên, gửi thông báo email qua sự kiện.
+ Thực hành pipeline DevOps tự động hóa từ code đến deploy.

