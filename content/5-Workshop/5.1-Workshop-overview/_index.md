---
title : "System Overview and Architecture"
date: "2025-12-07" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

**Serverless Student Management System** (SMS) is a modern student management platform built entirely on AWS serverless services. This system helps schools or small businesses easily manage, analyze, and interact with student data at any scale without worrying about hardware infrastructure.  
Serverless components like Lambda, DynamoDB, AppSync, and EventBridge enable the system to auto-scale, optimize costs, and maintain high reliability and security.

+ Users interact via a web interface (React/Amplify), with real-time dashboard, assignment chat, gradebook & learning ranking.
+ Backend APIs (REST & GraphQL) ensure access to student data, personalized permissions, email notifications, and tracking learning events.
+ No need for traditional server management; the entire deployment and operation process—from storage, processing, to CI/CD—is integrated and automated through AWS services.

#### Workshop Overview

In this workshop, you will:

+ Initialize and configure AWS core serverless services: DynamoDB, Lambda, API Gateway, Amplify, AppSync, Cognito, SES, EventBridge, Personalize.
+ Build and test the student management system with features: CRUD dashboard, real-time chat, ML ranking, email notifications, service quality monitoring.
+ Experience automated DevOps CI/CD workflow from GitLab to AWS Pipeline, test and live demo the system.
+ Apply practical knowledge to real lessons or similar projects, easily extend to mobile environments or integrate advanced AI.

![overview](/images/5-Workshop/5.1-Workshop-overview/solution.drawio.png)


