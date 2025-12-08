---
title : "CloudWatch"
date: "2025-12-07"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng quan

Phần này hướng dẫn bạn sử dụng Amazon CloudWatch để giám sát, cảnh báo và quản lý các dịch vụ trong hệ thống Serverless Student Management System trên AWS. Bạn sẽ học cách tạo log groups cho Lambda, thiết lập các cảnh báo (alarms) cho lỗi và hiệu năng của Lambda, API Gateway, cũng như xây dựng dashboard trực quan để theo dõi hoạt động của toàn bộ hệ thống. Việc giám sát chủ động giúp phát hiện sớm sự cố, tối ưu vận hành và kiểm soát chi phí AWS hiệu quả.

---

## PHẦN 8: CLOUDWATCH (Monitoring)

### 1. Tạo Log Groups

Lambda tự động tạo log group: `/aws/lambda/student-management-api`

### 2. Tạo Alarms

1. Vào **CloudWatch → Alarms → Create alarm**

**Lambda Error Alarm:**
```
Metric: AWS/Lambda → Errors
Function: student-management-api
Statistic: Sum
Period: 5 minutes
Threshold: > 5
```

**API Gateway 5xx Alarm:**
```
Metric: AWS/ApiGateway → 5XXError
Statistic: Sum
Period: 5 minutes
Threshold: > 10
```

### 3. Tạo Dashboard

1. Vào **CloudWatch → Dashboards → Create dashboard**
2. Add widgets:
   - Lambda invocations
   - Lambda errors
   - Lambda duration
   - API Gateway requests
   - DynamoDB read/write capacity

---

#### Tổng kết

Qua phần này, bạn đã biết cách sử dụng Amazon CloudWatch để giám sát, cảnh báo và quản lý hiệu quả các dịch vụ AWS trong hệ thống serverless. Việc thiết lập log groups, alarms và dashboard giúp bạn phát hiện sớm sự cố, tối ưu vận hành, kiểm soát chi phí và đảm bảo hệ thống luôn hoạt động ổn định, an toàn. Đây là bước quan trọng để duy trì chất lượng dịch vụ và hỗ trợ vận hành lâu dài trên nền tảng AWS.