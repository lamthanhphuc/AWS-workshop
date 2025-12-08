---
title : "Xây dựng Frontend: Amplify, ROUTE53, CloudFront, WAF"
date: "2025-12-07" 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ xây dựng giao diện web cho hệ thống **Serverless Student Management System** bằng **AWS Amplify**, **CLOUDFRONT**, **WAF** và **ROUTE53**.  
Frontend cung cấp các chức năng:

Kiến trúc tổng quát:

![Amplify Architecture](/images/5-Workshop/5.4-Frontend/architecture.png)

---

## 1. AMPLIFY (Frontend)

### 1.1 Cách 1: Amplify Console (Recommended)

1. Vào **AWS Console → Amplify → Create new app**
2. Chọn **Host web app**
3. Connect repository:
   - GitHub/GitLab/Bitbucket
   - Authorize và chọn repo

4. Cấu hình build:

**amplify.yml** (đã có trong project):
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
   - `VITE_API_BASE_URL`: URL của API Gateway
   - `VITE_COGNITO_USER_POOL_ID`: User Pool ID
   - `VITE_COGNITO_CLIENT_ID`: Client ID
   - `VITE_COGNITO_REGION`: ap-southeast-1

6. Deploy và lấy Amplify URL

### 1.2 Cách 2: Amplify CLI

```bash
# Cài đặt
npm install -g @aws-amplify/cli

# Cấu hình AWS credentials
amplify configure

# Khởi tạo trong project
cd serverless-student-management-system-front-end
amplify init

# Thêm hosting
amplify add hosting
# Chọn: Hosting with Amplify Console
# Chọn: Manual deployment

# Deploy
amplify publish
```

---

## 2. CLOUDFRONT + WAF

### 2.1 Tạo WAF Web ACL

1. Vào **AWS Console → WAF & Shield → Create web ACL**
2. Cấu hình:
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

### 2.2 Tạo CloudFront Distribution

1. Vào **AWS Console → CloudFront → Create distribution**

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

### 3.1 Tạo Hosted Zone

1. Vào **AWS Console → Route53 → Create hosted zone**
2. Domain name: `yourdomain.com`
3. Type: Public hosted zone

### 3.2 Cấu hình DNS Records

**A Record cho CloudFront:**
```
Record name: (empty hoặc www)
Record type: A
Alias: Yes
Route traffic to: CloudFront distribution
```

**CNAME cho Amplify (nếu không dùng CloudFront):**
```
Record name: app
Record type: CNAME
Value: xxx.amplifyapp.com
```

### 3.3 SSL Certificate (ACM)

1. Vào **AWS Console → Certificate Manager**
2. Request certificate (phải ở region us-east-1 cho CloudFront)
3. Domain: `yourdomain.com`, `*.yourdomain.com`
4. Validation: DNS validation
5. Add CNAME records to Route53

---

# 4. Tổng kết

Trang "Xây dựng Frontend: Amplify, ROUTE53, CloudFront, WAF hướng dẫn đầy đủ:
- Tạo project React với Amplify
- Dùng Cognito để xác thực frontend
- Hosting CI/CD qua Amplify

Frontend kết nối trực tiếp với hệ thống serverless backend, tạo trải nghiệm realtime mượt mà và bảo mật.