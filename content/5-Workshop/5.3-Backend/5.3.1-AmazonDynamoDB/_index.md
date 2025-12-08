---
title: "Amazon DynamoDB"
date: "2025-12-07"
weight: 1
chapter: false
pre: "<b> 5.3.1 </b>"
---

## 1. Amazon DynamoDB

DynamoDB is a serverless NoSQL database that stores all system data:

- Student information
- Classes, subjects
- Lecturers
- Scores

Spring Boot backend system interacts with DynamoDB via:

- AWS SDK for Java 17
- Spring Data DynamoDB or repository written by the team
- Presigned URL (upload documents, profiles)

### 1.1 DynamoDB table design (Single-Table Design)

![DynamoDB](/images/5-Workshop/5.3-Backend/5.3.1-AmazonDynamoDB/DynamoDB.png)

#### 1.1.1 Create Tables

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

#### 1.1.2 Global Secondary Index (GSI) Configuration

For the Users table, add GSI:

---
- Index name: `role-index`
- Partition key: `role` (String)
---

![GSI](/images/5-Workshop/5.3-Backend/5.3.1-AmazonDynamoDB/GSI.png)