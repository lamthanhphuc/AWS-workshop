---
title : "Xây dựng Frontend: Amplify, AppSync, Realtime Chat"
date: "2025-12-07" 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ xây dựng giao diện web cho hệ thống **Serverless Student Management System** bằng **AWS Amplify**, **AppSync (GraphQL)** và **Realtime Subscription**.  
Frontend cung cấp các chức năng:

- Đăng nhập bằng Amazon Cognito  
- Dashboard realtime  
- Chat thời gian thực  
- CRUD Student/Classes  
- Gửi tương tác sự kiện để backend xử lý  

Kiến trúc tổng quát:

![Amplify Architecture](/images/5-Workshop/5.4-Frontend/architecture.png)

---

# 1. Khởi tạo dự án Frontend với AWS Amplify

### **1.1 Cài đặt Amplify CLI**
```bash
npm install -g @aws-amplify/cli
amplify configure
```
### **1.2 Khởi tạo project React**
```bash
npx create-react-app student-portal
cd student-portal
amplify init
```
Lựa chọn:

- Hosting: Amplify Hosting
- Auth: Cognito User Pool
- API: AppSync GraphQL

# 2. Tạo GraphQL API với AppSync
Chạy lệnh:
```bash
amplify add api
```
Chọn:
- GraphQL API
- Authorization: Cognito User Pool
- Conflict detection: Auto merge
- Realtime subscription: Enable

### **2.1 Định nghĩa schema GraphQL**

File: amplify/backend/api/studentapi/schema.graphql
```graphql
type Student @model @auth(rules: [{ allow: owner }]) {
  id: ID!
  name: String!
  email: String!
  classId: ID
}

type Message
  @model
  @auth(rules: [{ allow: owner }, { allow: private }])
  @aws_subscribe(mutations: ["sendMessage"]) {
  id: ID!
  sender: String!
  content: String!
  createdAt: AWSDateTime!
}

input SendMessageInput {
  sender: String!
  content: String!
}

type Mutation {
  sendMessage(input: SendMessageInput!): Message
    @function(name: "chatHandler-${env}")
}
```
### **2.2 Deploy API**
```bash
amplify push
```
# 3. Tích hợp Cognito Authentication

Amplify tự tạo cấu hình Auth:
```bash
amplify add auth
```
Chọn:
- Email sign-in
- No MFA (hoặc optional)
- Import vào React:
```javascript
import { Auth } from 'aws-amplify';
const user = await Auth.signIn(email, password);
console.log("Logged in:", user);
```

# 4. Realtime Subscription (Chat)
## **4.1 Gửi tin nhắn**
```javascript
import { API, graphqlOperation } from 'aws-amplify';
import { sendMessage } from './graphql/mutations';

await API.graphql(
  graphqlOperation(sendMessage, {
    input: { sender, content }
  })
);
```
## **4.2 Nhận tin nhắn realtime**
```javascript
import { onCreateMessage } from './graphql/subscriptions';

useEffect(() => {
  const sub = API.graphql(
    graphqlOperation(onCreateMessage)
  ).subscribe({
    next: ({ value }) => {
      const msg = value.data.onCreateMessage;
      setMessages(prev => [...prev, msg]);
    }
  });

  return () => sub.unsubscribe();
}, []);
```

# 5. Hosting Frontend trên Amplify Hosting
## **5.1 Kết nối repo GitHub**
Trong AWS Amplify Console:
- Chọn New App → Host Web App
- Kết nối GitHub Repository
- Amplify tự build + deploy qua CI/CD

## **5.2 Cấu hình build**

File amplify.yml:
```yml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: build
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```
# 6. Kết nối Realtime Dashboard

Ví dụ sử dụng GraphQL subscription để cập nhật số lượng sinh viên online:
```graphql
type OnlineEvent @model @aws_subscribe(mutations: ["updateOnline"]) {
  id: ID!
  userId: String!
  time: AWSDateTime!
}
```

React subscription:
```javascript
API.graphql(graphqlOperation(onUpdateOnline)).subscribe({
  next: ({ value }) => {
    const e = value.data.onUpdateOnline;
    setOnlineList(prev => [...prev, e]);
  }
});
```
# 7. Tổng kết

Trang "Xây dựng Frontend: Amplify, AppSync, Realtime Chat" hướng dẫn đầy đủ:
- Tạo project React với Amplify
- Dùng Cognito để xác thực frontend
- AppSync GraphQL API
- Realtime subscription cho chat
- Hosting CI/CD qua Amplify

Frontend kết nối trực tiếp với hệ thống serverless backend, tạo trải nghiệm realtime mượt mà và bảo mật.