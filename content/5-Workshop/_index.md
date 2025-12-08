---
title: "Workshop"
date: "2025-12-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Serverless Student Management System on AWS

#### Overview

**Serverless Student Management Platform** is a cloud-based student management solution, optimized for cost and scalable flexibility by using **AWS Serverless** services (Lambda, DynamoDB, AppSync, EventBridge, etc.). The solution provides a real-time dashboard, assignment chat, machine learning-based ranking analysis, automated notifications, and operates entirely on event-driven architecture.

Core services: API Gateway (REST), Lambda for business logic, DynamoDB for data storage, AppSync (GraphQL) & Amplify (React frontend), Cognito (user authentication), SES (email sending), Personalize (ML ranking), EventBridge (event orchestration), CloudWatch (monitoring), combined with modern DevOps CI/CD workflows.

#### Contents

1. [System Overview and Architecture](5.1-Overview/)
2. [Environment Preparation & AWS Account Setup](5.2-Prerequiste/)
3. [Backend Deployment: DynamoDB, Lambda, API Gateway, Cognito](5.3-Backend/)
4. [Frontend Development: Amplify, AppSync, Realtime Chat](5.4-Frontend/)
5. [Event-driven, Email Notification & ML Ranking](5.5-Event-ML/)
6. [Resource Cleanup](5.6-Cleanup/)

#### Experience Objectives

+ Understand multi-layer serverless architecture operating on AWS.
+ Learn permission models with Cognito, real-time processing with AppSync.
+ Directly build CRUD, chat, student ranking, send email notifications via events.
+ Practice automated DevOps pipeline from code to deployment.

