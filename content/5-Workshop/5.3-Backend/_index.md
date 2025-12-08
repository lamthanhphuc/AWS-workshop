---
title: "Backend Deployment"
date: "2025-12-07"
weight: 3
chapter: false
pre: "<b> 5.3. </b>"
---

# Building a Serverless Backend with AWS Lambda, API Gateway, DynamoDB, and Cognito

## Overview

This section guides you through the backend implementation of a serverless student management system on AWS. You will use core services such as DynamoDB for data storage, Lambda for business logic, API Gateway to connect the frontend and backend, and Cognito for user authentication. The implementation process includes designing the data table, configuring authentication, building the backend application with Java Spring Boot, packaging and deploying to Lambda, as well as configuring API Gateway to serve student management tasks in a secure, automated, and scalable way.

## ![Amplify Architecture](/images/5-Workshop/5.3-Backend/architecture.png)

## Content

1. [Amazon DynamoDB](5.3.1-AmazonDynamoDB/)
2. [Amazon Cognito](5.3.2-AmazonCognito/)
3. [Amazon S3](5.3.3-AmazonS3/)
4. [LAMBDA + API GATEWAY](5.3.4-LAMBDA&APIGATEWAY/)