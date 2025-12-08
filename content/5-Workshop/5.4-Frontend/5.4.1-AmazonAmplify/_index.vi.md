---
title: "Amazon Amplify"
date: "2025-12-07"
weight: 1
chapter: false
pre: "<b> 5.4.1 </b>"
---

## AMPLIFY 

### 1. Cách 1: Amplify Console (Recommended)

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

### 2. Cách 2: Amplify CLI

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

![Amplify](/images/5-Workshop/5.4-Frontend/5.4.1-AmazonAmplify/Amplify.png)


