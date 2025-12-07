---
title : "Access S3 from on-premises"
date: "2006-01-02"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

+ In this section, you will create an Interface endpoint to access Amazon S3 from a simulated on-premises environment. The Interface endpoint will allow you to route to Amazon S3 over a VPN connection from your simulated on-premises environment.

+ Why using **Interface endpoint**: 
    + Gateway endpoints only work with resources running in the VPC where they are created. Interface endpoints work with resources running in VPC, and also resources running in on-premises environments. Connectivty from your on-premises environment to the cloud can be provided by AWS Site-to-Site VPN or AWS Direct Connect.
    + Interface endpoints allow you to connect to services powered by AWS PrivateLink. These services include some AWS services, services hosted by other AWS customers and partners in their own VPCs (referred to as PrivateLink Endpoint Services), and supported AWS Marketplace Partner services. For this workshop, we will focus on connecting to Amazon S3.

![Interface endpoin---
title : "Dọn dẹp tài nguyên"
date: "2025-12-07"  
weight : 5
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ xây dựng cơ chế **event-driven** cho hệ thống Serverless Student Management System bằng cách sử dụng **Amazon EventBridge**, **AWS Lambda**, **Amazon SES**, và **Amazon Personalize**.  

Hệ thống có khả năng:

- Phát sinh sự kiện khi sinh viên đăng ký lớp, nộp bài, hoặc nhận thông báo  
- Gửi email tự động qua Amazon SES  
- Huấn luyện mô hình ML bằng Amazon Personalize  
- Tạo ranking sinh viên hoặc gợi ý khóa học cá nhân hóa  
- Xử lý luồng sự kiện phi đồng bộ và mở rộng vô hạn  

Sơ đồ tổng quan:

![Event-driven Architecture](/images/5-Workshop/5.5-EventDriven/architecture.png)

---

# 1. Event-driven với Amazon EventBridge

### **1.1 Tạo Event Bus**

Trong EventBridge Console:

- Create → Event Bus  
Tên: `student-event-bus`

### **1.2 Các loại sự kiện hệ thống**

| Event Type | Source | Detail Example |
|------------|--------|----------------|
| `student.enrolled` | api.student | `{ "studentId": "1", "classId": "A1" }` |
| `student.submitAssignment` | api.assignment | `{ "studentId": "1", "fileUrl": "..." }` |
| `teacher.sendAnnouncement` | api.teacher | `{ "teacherId": "T1", "content": "..." }` |
| `ranking.requestUpdate` | api.ranking | `{ "trigger": "nightly" }` |

### **1.3 Publish sự kiện từ Lambda**

```javascript
import { EventBridgeClient, PutEventsCommand } from "@aws-sdk/client-eventbridge";

const client = new EventBridgeClient();

export const handler = async (event) => {
  await client.send(
    new PutEventsCommand({
      Entries: [
        {
          EventBusName: "student-event-bus",
          Source: "api.student",
          DetailType: "student.enrolled",
          Detail: JSON.stringify({
            studentId: event.studentId,
            classId: event.classId
          })
        }
      ]
    })
  );

  return { status: "OK" };
};
```
# 2. Email Notification bằng Amazon SES
### **2.1 Cấu hình SES**
- Verify domain
- Verify email sender
- Request production access (hoặc dùng sandbox với chế độ test)

## **2.2 Lambda gửi email**
Lambda được trigger bởi EventBridge rule:

```javascript

import { SESClient, SendEmailCommand } from "@aws-sdk/client-ses";

const ses = new SESClient({});

export const handler = async (event) => {
  const detail = event.detail;

  const params = {
    Destination: {
      ToAddresses: [detail.email]
    },
    Message: {
      Body: {
        Text: { Data: `Bạn vừa đăng ký lớp ${detail.classId}.` }
      },
      Subject: { Data: "Xác nhận đăng ký lớp" }
    },
    Source: "noreply@student-system.com"
  };

  await ses.send(new SendEmailCommand(params));

  return { status: "EMAIL_SENT" };
};
```
## **2.3 Rule: gửi email khi đăng ký lớp**
```css
Event Pattern:
{
  "detail-type": ["student.enrolled"]
}
```
# 3. ML Ranking với Amazon Personalize
## **3.1 Mục tiêu**
Sử dụng Personalize để:
- Gợi ý môn học phù hợp
- Ranking sinh viên theo độ tương tác
- Gợi ý tài liệu, quiz
## **3.2 Chuẩn bị dữ liệu DynamoDB**
Bảng cần có:
- Interactions
- Students
- ClassActivity

Sử dụng Glue crawler → S3 để tạo dataset import.

## **3.3 Tạo dataset trong Personalize**
Dataset Group: student-ranking

Dataset Type: Interactions, Items, Users

Schema JSON theo định dạng Personalize

## **3.4 Training mô hình**
Chọn recipe:
- aws-user-personalization (gợi ý cá nhân hóa)
- Hoặc aws-personalized-ranking (ranking theo activity)
## **3.5 Tạo Campaign**
Output:
- ARN của campaign
- API endpoint để request recommendation
# 4. Tích hợp Personalize vào Backend
## **4.1 Lambda gọi Personalize**
```javascript
import {
  PersonalizeRuntimeClient,
  GetRecommendationsCommand
} from "@aws-sdk/client-personalize-runtime";

const client = new PersonalizeRuntimeClient();

export const handler = async (event) => {
  const { userId } = event;

  const response = await client.send(
    new GetRecommendationsCommand({
      campaignArn: process.env.CAMPAIGN_ARN,
      userId
    })
  );

  return { recommendations: response.itemList };
};
```
## **4.2 Expose qua API Gateway**
Method	Endpoint	Lambda
GET	/ranking/{userId}	ranking.getRecommendation

# 5. Tích hợp Event-driven với ML (Nightly Auto Update)
EventBridge Scheduler:
- Thời gian: 0 2 * * * (2 giờ sáng hàng ngày)
- Trigger: Lambda ranking.generateTrainingData

Flow:

1. Lambda export dữ liệu mới từ DynamoDB → S3

2. Update dataset

3. Re-train model hoặc incremental import

4. Update campaign

# 6. Tổng kết
Trang “Event-driven, Email Notification & ML Ranking” cung cấp:

- Cách tạo hệ thống event-driven hoàn chỉnh bằng EventBridge

- Gửi email tự động với SES

- Tích hợp Machine Learning để gợi ý và ranking sinh viên

- Tự động hóa training bằng event scheduler

Kết hợp các service này giúp hệ thống hoạt động mượt, realtime và thông minh hơn — mang lại trải nghiệm học tập cá nhân hóa cho người dùng.t architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)





