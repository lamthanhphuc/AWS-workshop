---
title: "Amazon CloudFornt & WAF"
date: "2025-12-07"
weight: 2
chapter: false
pre: "<b> 5.4.2 </b>"
---

## CLOUDFRONT + WAF

### 1. Tạo WAF Web ACL

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

![ACL](/images/5-Workshop/5.4-Frontend/5.4.2-CloudFront&WAF/ACL.png)

### 2. Tạo CloudFront Distribution

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

![CloudFront](/images/5-Workshop/5.4-Frontend/5.4.2-CloudFront&WAF/CloudFront.png)

---