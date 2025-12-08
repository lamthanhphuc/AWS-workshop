---
title: "Amazon Amplify"
date: "2025-12-07"
weight: 1
chapter: false
pre: "<b> 5.4.1 </b>"
---

## AMPLIFY 

### 1. Method 1: Amplify Console (Recommended)

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

### 2. Method 2: Amplify CLI

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

![Amplify](/images/5-Workshop/5.4-Frontend/5.4.1-AmazonAmplify/Amplify.png)
