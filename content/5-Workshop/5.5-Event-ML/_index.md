---
title : "Event-driven, Email Notification & ML Ranking"
date: "2025-12-07"  
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Overview

In this section, you will build an **event-driven** mechanism for a Serverless Student Management System using **Amazon EventBridge**, **AWS Lambda**, **Amazon SES**, and **Amazon Personalize**.

The system is capable of:

- Emitting events when students register for classes, submit assignments, or receive notifications
- Sending automated emails via Amazon SES
- Training ML models using Amazon Personalize
- Generating student rankings or personalized course recommendations
- Processing asynchronous event streams and scaling infinitely

Overview diagram:

![Event-driven Architecture](/images/5-Workshop/5.5-EventDriven/architecture.png)

---

# 1. Event-driven with Amazon EventBridge

### **1.1 Creating an Event Bus**

In EventBridge Console:

- Create → Event Bus
Name: `student-event-bus`

### **1.2 Types of system events**

| Event Type | Source | Detail Example |
|------------|-------|----------------|
| `student.enrolled` | api.student | `{ "studentId": "1", "classId": "A1" }` |
| `student.submitAssignment` | api.assignment | `{ "studentId": "1", "fileUrl": "..." }` |
| `teacher.sendAnnouncement` | api.teacher | `{ "teacherId": "T1", "content": "..." }` |
| `ranking.requestUpdate` | api.ranking | `{ "trigger": "nightly" }` |

### **1.3 Publish events from Lambda**

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
# 2. Email Notification using Amazon SES
### **2.1 SES Configuration**
- Verify domain
- Verify email sender
- Request production access (or use sandbox with test mode)

## **2.2 Lambda sending email**
Lambda triggered by EventBridge rule:

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
## **2.3 Rule: send email when registering for class**
```css
Event Pattern:
{
  "detail-type": ["student.enrolled"]
}
```
# 3. ML Ranking with Amazon Personalize
## **3.1 Objectives**
Use Personalize to:
- Suggest suitable subjects
- Rank students by interaction
- Suggest documents, quiz
## **3.2 Prepare DynamoDB data**
Required tables:
- Interactions
- Students
- ClassActivity

Use Glue crawler → S3 to create dataset import.

## **3.3 Create dataset in Personalize**
Dataset Group: student-ranking

Dataset Type: Interactions, Items, Users

JSON Schema in Personalize format

## **3.4 Training model**
Choose recipe:
- aws-user-personalization (personalized recommendations)
- Or aws-personalized-ranking (ranking by activity)
## **3.5 Create Campaign**
Output:
- Campaign ARN
- API endpoint to request recommendation
# 4. Integrate Personalize into Backend
## **4.1 Lambda calls Personalize**
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
## **4.2 Expose via API Gateway**
Method Endpoint Lambda
GET /ranking/{userId} ranking.getRecommendation

# 5. Integrate Event-driven with ML (Nightly Auto Update)
EventBridge Scheduler:
- Time: 0 2 * * * (2 AM daily)
- Trigger: Lambda ranking.generateTrainingData

Flow:

1. Lambda exports new data from DynamoDB → S3

2. Update dataset

3. Re-train model or incremental import

4. Update campaign

# 6. Summary
The “Event-driven, Email Notification & ML Ranking” page provides:

- How to create a complete event-driven system using EventBridge

- Send automatic emails with SES

- Integrate Machine Learning to recommend and rank students

- Automate training with event scheduler

Combining these services helps the system operate Smooth, real-time, and smarter — delivering personalized learning experiences to users.