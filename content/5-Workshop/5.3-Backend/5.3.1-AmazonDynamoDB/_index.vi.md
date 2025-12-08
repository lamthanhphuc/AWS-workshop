---
title: "Amazon DynamoDB"
date: "2025-12-07"
weight: 1
chapter: false
pre: "<b> 5.3.1 </b>"
---

## 1. Amazon DynamoDB

DynamoDB là cơ sở dữ liệu NoSQL serverless, lưu trữ tất cả dữ liệu của hệ thống:

- Thông tin sinh viên
- Lớp học, môn học
- Giảng viên
- Điểm số

Hệ thống backend Spring Boot tương tác DynamoDB qua:

- AWS SDK for Java 17
- Spring Data DynamoDB hoặc repository do team tự viết
- Presigned URL (upload tài liệu, hồ sơ)

### 1.1 Thiết kế bảng DynamoDB (Single-Table Design)

![DynamoDB](/images/5-Workshop/5.3-Backend/5.3.1-AmazonDynamoDB/DynamoDB.png)

#### 1.1.1 Tạo Tables

**Table 1: Users**

```
Table name: student-management-users
Partition key: id (String)
Sort key: email (String)
```

**Table 2: Classes**

```
Table name: student-management-classes
Partition key: id (String)
```

**Table 3: Subjects**

```
Table name: student-management-subjects
Partition key: id (String)
```

**Table 4: Notifications**

```
Table name: student-management-notifications
Partition key: id (Number)
Sort key: sent_at (String)
```

#### 1.1.2 Cấu hình Global Secondary Index (GSI)

Cho table Users, thêm GSI:

---
- Index name: `role-index`
- Partition key: `role` (String)
---

![GSI](/images/5-Workshop/5.3-Backend/5.3.1-AmazonDynamoDB/GSI.png)