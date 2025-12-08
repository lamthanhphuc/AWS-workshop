---
title: "Workshop"
date: "2025-12-07"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Serverless Student Management System on AWS

#### Overview

**Serverless Student Management Platform** is a cloud-based student management solution, cost-effective and flexible scalability thanks to the use of **AWS Serverless** services (Lambda, DynamoDB, API gateway...). The platform supports up to 50-100 students initially, with the ability to flexibly expand to 500–1000 students without major infrastructure changes.

Core services: API Gateway (REST), Lambda handles business logic, DynamoDB stores data, Amplify (frontend React), Cognito (user authentication), CloudWatch (monitoring), combined with modern DevOps CI/CD processes.

#### Contents

1. [System Overview and Architecture](5.1-Overview/)
2. [Environment Preparation & AWS Account Setup](5.2-Prerequiste/)
3. [Backend Deployment: DynamoDB, Lambda, API Gateway, Cognito](5.3-Backend/)
4. [Frontend Development: Amplify, AppSync, Realtime Chat](5.4-Frontend/)
5. [CI/CD_Pipeline](5.5-CI/CDPIPELINE/)
6. [CloudWatch](5.6-CLOUDWATCH/)

#### Experience Objectives

- Understand and implement multi-tier serverless architecture on AWS, operate with key services.
- Master authorization and user authentication with Cognito.
- Practice building CRUD functions.
- Experience modern DevOps process: automate build, test, deploy through CI/CD pipeline.
