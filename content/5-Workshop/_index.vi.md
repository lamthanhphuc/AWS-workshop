---
title: "Workshop"
date: "2025-12-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Serverless Student Management System trên AWS

## Tổng quan

**Serverless Student Management Platform** là giải pháp quản lý sinh viên dựa trên đám mây, tối ưu chi phí và mở rộng linh hoạt nhờ sử dụng các dịch vụ **AWS Serverless** (Lambda, DynamoDB, API gateway...). Nền tảng hỗ trợ tối đa 50-100 sinh viên ban đầu, với khả năng mở rộng linh hoạt lên đến 500–1000 sinh viên mà không cần thay đổi hạ tầng lớn.

Các dịch vụ core: API Gateway (REST), Lambda xử lý logic nghiệp vụ, DynamoDB lưu dữ liệu, Amplify (frontend React), Cognito (xác thực người dùng), CloudWatch (giám sát), kết hợp quy trình DevOps CI/CD hiện đại.

## Nội dung

1. [Tổng quan & kiến trúc hệ thống](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường & tài khoản AWS](5.2-Prerequiste/)
3. [Triển khai Backend: DynamoDB, Lambda, API Gateway, Cognito](5.3-Backend/)
4. [Xây dựng Frontend: Amplify, Route 53, CloudFront, WAF](5.4-Frontend/)
5. [CI/CD_Pipeline](5.5-CI-CD/)
6. [CloudWatch](5.6-Cloud-Watch/)
7. [Dọn dẹp tài nguyên](5.7-Clean-up/)

## Mục tiêu trải nghiệm

- Hiểu và triển khai kiến trúc serverless đa tầng trên AWS, vận hành thực tế với các dịch vụ chủ chốt.
- Nắm vững cách phân quyền, xác thực người dùng với Cognito.
- Thực hành xây dựng các chức năng CRUD.
- Trải nghiệm quy trình DevOps hiện đại: tự động hóa build, test, deploy qua CI/CD pipeline.
