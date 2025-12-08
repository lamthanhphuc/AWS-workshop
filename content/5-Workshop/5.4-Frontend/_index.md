---
title : "Frontend Development: Amplify, AppSync, Realtime Chat"
date: "2025-12-07"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

In this section, you will build a web interface for a **Serverless Student Management System** using **AWS Amplify**, **AppSync (GraphQL)**, and **Realtime Subscription**.

Frontend provides the following functions:

- Login with Amazon Cognito
- Realtime Dashboard
- Realtime Chat
- CRUD Student/Classes
- Send event interactions to the backend

Overall Architecture:

![Amplify Architecture](/images/5-Workshop/5.4-Frontend/architecture.png)

---

# 1. Initialize the Frontend project with AWS Amplify

### **1.1 Install Amplify CLI**
```bash
npm install -g @aws-amplify/cli
amplify configure
```
### **1.2 Initialize the React project**
```bash
npx create-react-app student-portal
cd student-portal
amplify init
```
Options:

- Hosting: Amplify Hosting
- Auth: Cognito User Pool
- API: AppSync GraphQL

# 2. Create GraphQL API with AppSync
Run command:
```bash
amplify add api
```
Select:
- GraphQL API
- Authorization: Cognito User Pool
- Conflict detection: Auto merge
- Realtime subscription: Enable

### **2.1 GraphQL schema definition**

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
amplifier push
```
# 3. Integrate Cognito Authentication

Amplify creates its own Auth configuration:
```bash
amplify add auth
```
Select:
- Email sign-in
- No MFA (or optional)
- Import into React:

```javascript
import { Auth } from 'aws-amplify';
const user = await Auth.signIn(email, password);
console.log("Logged in:", user);
```

# 4. Realtime Subscription (Chat)
## **4.1 Sending a message**
```javascript
import { API, graphqlOperation } from 'aws-amplify';
import { sendMessage } from './graphql/mutations';

await API.graphql(
  graphqlOperation(sendMessage, {
    input: { sender, content }
  })
);
```
## **4.2 Receive realtime messages**
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

# 5. Hosting Frontend on Amplify Hosting
## **5.1 Connect GitHub repository**
In AWS Amplify Console:
- Select New App → Host Web App
- Connect GitHub Repository
- Amplify self-build + deploy via CI/CD

## **5.2 Build configuration**

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
# 6. Connect Realtime Dashboard

Example using GraphQL subscription to update the number of online students:
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
# 7. Summary

The "Building a Frontend: Amplify, AppSync, Realtime Chat" page has a complete guide:
- Create a React project with Amplify
- Use Cognito for frontend authentication
- AppSync GraphQL API
- Realtime subscription for chat
- Hosting CI/CD via Amplify

Frontend connects directly to the serverless backend system, creating a smooth and secure realtime experience.