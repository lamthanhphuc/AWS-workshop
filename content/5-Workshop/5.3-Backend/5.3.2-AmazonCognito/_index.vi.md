---
title: "Amazon Cognito"
date: "2025-12-07"
weight: 3
chapter: false
pre: "<b> 5.3. </b>"
---

## Amazon Cognito

### 1. Tạo User Pool

1. Vào **AWS Console → Cognito → Create user pool**
2. Cấu hình:

   - Sign-in: Email
   - Password policy: Minimum 8 characters
   - MFA: Optional
   - Email: Send email with Cognito

![UserPool](/images/5-Workshop/5.3-Backend/5.3.2-AmazonCognito/UserPool.png)

3. Tạo App Client:
   - App client name: `student-management-app`
   - Generate client secret: No
   - Auth flows: `ALLOW_USER_SRP_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`

![AppClient](/images/5-Workshop/5.3-Backend/5.3.2-AmazonCognito/AppClient.png)

### 2. Lưu thông tin

```env
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXX
VITE_COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
VITE_COGNITO_REGION=ap-southeast-1
```



