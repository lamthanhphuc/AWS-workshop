---
title : "Frontend Development: Amplify, AppSync, Realtime Chat"
date: "2025-12-07"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

In this section, you will build a web interface for the **Serverless Student Management System** using **AWS Amplify**, **CLOUDFRONT**, **WAF**, and **ROUTE53**.

Frontend provides the following functions:

Overall architecture:

![Amplify Architecture](/images/5-Workshop/5.4-Frontend/architecture.png)

---

## 1. AMPLIFY (Frontend)

### 1.1 Method 1: Amplify Console (Recommended)

1. Go to **AWS Console → Amplify → Create new app**
2. Select **Host web app**
3. Connect repository:
- GitHub/GitLab/Bitbucket
- ​​Authorize and select repo

4. Build configuration:

**amplify.yml** (already in the project):
```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm ci
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

5. **Environment Variables:** 
- `VITE_API_BASE_URL`: API Gateway URL 
- `VITE_COGNITO_USER_POOL_ID`: User Pool ID 
- `VITE_COGNITO_CLIENT_ID`: Client ID 
- `VITE_COGNITO_REGION`: ap-southeast-1

6. Deploy and get Amplify URL

### 1.2 Method 2: Amplify CLI

```bash
# Setting
npm install -g @aws-amplify/cli

# Configure AWS credentials
amplifier configure

# Initialize in project
cd serverless-student-management-system-front-end
amplify init

# Add hosting
amplify add hosting
# Choose: Hosting with Amplify Console
# Select: Manual deployment.deployment

# Deploy
amplify publish
```

---

## 2. CLOUDFRONT + WAF

### 2.1 Create WAF Web ACL

1. Go to **AWS Console → WAF & Shield → Create web ACL**
2. Configuration: 
- Name: `student-management-waf`
   - Resource type: CloudFront distributions
   - Region: Global (CloudFront)

3. Add rules:
   - **AWS Managed Rules:**
     - `AWSManagedRulesCommonRuleSet` (Core rule set)
     - `AWSManagedRulesKnownBadInputsRuleSet`
     - `AWSManagedRulesSQLiRuleSet` (SQL injection)
   
   - **Rate limiting:**
     - Name: `RateLimitRule`
     - Rate limit: 2000 requests per 5 minutes per IP

- **Rate limiting:** 
- Name: `RateLimitRule` 
- Rate limit: 2000 requests per 5 minutes per IP

### 2.2 Create CloudFront Distribution

1. Go to **AWS Console → CloudFront → Create distribution**

2. **Origin Settings:**
   - Origin domain: Amplify app URL (xxx.amplifyapp.com)
   - Protocol: HTTPS only

3. **Default Cache Behavior:**
   - Viewer protocol policy: Redirect HTTP to HTTPS
   - Allowed HTTP methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
   - Cache policy: CachingOptimized
   - Origin request policy: AllViewer

4. **Settings:**
   - Price class: Use all edge locations
   - WAF web ACL: Chọn ACL đã tạo
   - SSL certificate: Default CloudFront certificate (hoặc custom)

5. **Thêm Origin cho API Gateway:**
   - Add origin: API Gateway URL
   - Create behavior: `/api/*` → API Gateway origin

---

## 3. ROUTE53 (Custom Domain)

### 3.1 Create Hosted Zone

1. Go to **AWS Console → Route53 → Create hosted zone**
2. Domain name: `yourdomain.com`
3. Type: Public hosted zone

### 3.2 Configure DNS Records

**A Record for CloudFront:**
```
Record name: (empty or www)
Record type: A
Alias: Yes
Route traffic to: CloudFront distribution
```

**CNAME for Amplify (if not using CloudFront):**
```
Record name: app
Record type: CNAME
Value: xxx.amplifyapp.com
```

### 3.3 SSL Certificate (ACM)

1. Go to **AWS Console → Certificate Manager**
2. Request certificate (must be in region us-east-1 for CloudFront)
3. Domain: `yourdomain.com`, `*.yourdomain.com`
4. Validation: DNS validation
5. Add CNAME records to Route53

---

# 4. Summary

Page "Building a Frontend: Amplify, ROUTE53, CloudFront, WAF" full guide:
- Create a React project with Amplify
- Use Cognito for frontend authentication
- Hosting CI/CD via Amplify

Frontend connects directly to the serverless backend system, creating a smooth and secure real-time experience.