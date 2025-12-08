---
title: "Amazon Route53"
date: "2025-12-07"
weight: 3
chapter: false
pre: "<b> 5.4.3 </b>"
---

## ROUTE53 (Custom Domain)

### 1. Create Hosted Zone

1. Go to **AWS Console → Route53 → Create hosted zone**
2. Domain name: `yourdomain.com`
3. Type: Public hosted zone

![hostedzone](/images/5-Workshop/5.4-Frontend/5.4.3-Route53/hostedzone.png)

### 2. Configure DNS Records

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

### 3. SSL Certificate (ACM)

1. Go to **AWS Console → Certificate Manager**
2. Request certificate (must be in region us-east-1 for CloudFront)
3. Domain: `yourdomain.com`, `*.yourdomain.com`
4. Validation: DNS validation
5. Add CNAME records to Route53

---

## Summary

Page "Building a Frontend: Amplify, ROUTE53, CloudFront, WAF" full guide:
- Create a React project with Amplify
- Use Cognito for frontend authentication
- Hosting CI/CD via Amplify

Frontend connects directly to the serverless backend system, creating a smooth and secure real-time experience.