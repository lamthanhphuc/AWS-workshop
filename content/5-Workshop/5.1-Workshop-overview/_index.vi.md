---
title : "Tổng quan và kiến trúc hệ thống"
date: "2006-01-02" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về Serverless Student Management System

**Serverless Student Management System** (SMS) là nền tảng quản lý sinh viên hiện đại, xây dựng hoàn toàn trên các dịch vụ serverless của AWS. Hệ thống này giúp các trường học hoặc doanh nghiệp nhỏ dễ dàng quản lý, phân tích, và tương tác với dữ liệu sinh viên ở quy mô từ nhỏ đến lớn mà không cần lo lắng về hạ tầng phần cứng.  
Các thành phần serverless như Lambda, DynamoDB, AppSync, EventBridge giúp hệ thống tự động mở rộng, tối ưu chi phí, duy trì độ tin cậy và bảo mật cao.

+ Người dùng thao tác qua giao diện web (React/Amplify), tương tác realtime với dashboard, chat bài tập, bảng điểm & ranking học tập.
+ API backend (REST & GraphQL) đảm bảo truy cập dữ liệu sinh viên, quyền truy cập cá nhân hóa, gửi thông báo qua email và theo dõi sự kiện học tập.
+ Không cần quản lý server truyền thống, toàn bộ quy trình triển khai vận hành – từ lưu trữ, xử lý, đến CI/CD – đều tích hợp và tự động hóa qua các dịch vụ AWS.


#### Tổng quan về workshop

Trong workshop này, bạn sẽ:

+ Khởi tạo và cấu hình các dịch vụ serverless core của AWS: DynamoDB, Lambda, API Gateway, Amplify, AppSync, Cognito, SES, EventBridge, Personalize.
+ Xây dựng và kiểm thử hệ thống quản lý sinh viên với các tính năng: dashboard CRUD, chat realtime, ranking ML, thông báo email, giám sát chất lượng dịch vụ.
+ Trải nghiệm quy trình DevOps CI/CD tự động hóa từ GitLab lên AWS Pipeline, kiểm thử và demo live hệ thống.
+ Vận dụng kiến thức thực hành vào bài học thực tế hoặc các dự án tương tự, dễ dàng mở rộng sang môi trường mobile hoặc tích hợp AI nâng cao.

![overview](/images/5-Workshop/5.1-Workshop-overview/solution.drawio.png)


