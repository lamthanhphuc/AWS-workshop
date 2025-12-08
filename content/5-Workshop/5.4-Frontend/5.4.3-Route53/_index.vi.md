---
title: "Amazon Route53"
date: "2025-12-07"
weight: 3
chapter: false
pre: "<b> 5.4.3 </b>"
---

## ROUTE53 (Custom Domain)

### 1. Tạo Hosted Zone

1. Vào **AWS Console → Route53 → Create hosted zone**
2. Domain name: `yourdomain.com`
3. Type: Public hosted zone

![hostedzone](/images/5-Workshop/5.4-Frontend/5.4.3-Route53/hostedzone.png)

### 2. Cấu hình DNS Records

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

### 3. SSL Certificate (ACM)

1. Vào **AWS Console → Certificate Manager**
2. Request certificate (phải ở region us-east-1 cho CloudFront)
3. Domain: `yourdomain.com`, `*.yourdomain.com`
4. Validation: DNS validation
5. Add CNAME records to Route53

---

## Tổng kết

Trang "Xây dựng Frontend: Amplify, ROUTE53, CloudFront, WAF hướng dẫn đầy đủ:
- Tạo project React với Amplify
- Dùng Cognito để xác thực frontend
- Hosting CI/CD qua Amplify

Frontend kết nối trực tiếp với hệ thống serverless backend, tạo trải nghiệm realtime mượt mà và bảo mật.