---
title: "Proposal"
date: 2025-11-02
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

# Serverless Student Management System
## Cloud-based serverless student management system for students and small businesses

### 1. Executive Summary

Serverless Student Management Platform is a cloud-based student management platform designed for educational institutions and small businesses to enhance the management, analysis, and interaction of student data. The platform supports up to 50-100 students initially, with the ability to flexibly scale up to 500-1000 students without major infrastructure changes, using AWS Serverless services such as Lambda, DynamoDB, AppSync, and EventBridge.

Users can access it via a friendly web interface (supporting real-time chat for assignments and comments) combined with APIs for real-time data transmission, ensuring high interactivity. The platform leverages AWS to provide centralized management (secure data storage), predictive analytics (via Personalize for score rankings and recommendations), reminder notifications (via SES), and system monitoring (via CloudWatch), while optimizing costs by charging only for actual usage.


### 2. Problem Statement

**Current Issues**

Current student management systems require manual data entry, making it difficult to manage multiple classes. There is no centralized system for data or real-time analytics, and third-party platforms are often expensive and overly complex. Lack of real-time communication features leads to delayed communication, absence of incentive mechanisms like diligence ranking, and manual exam event processing.

**Solution**

The platform uses Amazon API Gateway to handle REST requests, AWS Lambda for business logic processing, Amazon DynamoDB for storing student data and scores, and AWS AppSync with GraphQL for real-time communication. AWS Amplify with React/Next.js provides the web interface, and Amazon Cognito ensures secure access. Similar to traditional LMS systems but with lower costs, users can register new students and manage information, but this platform operates at a smaller scale and serves educational purposes. Key features include real-time management dashboard, learning trend analysis, and low operational costs.

**Benefits and Return on Investment (ROI)**

The solution creates a foundation for IT students to develop AWS serverless skills while providing efficient management tools for teachers in instruction and assessment. The platform reduces manual reporting for each class through centralized systems, simplifies management and maintenance, and improves data reliability. Monthly costs estimated at $7-20 USD (according to AWS Pricing Calculator), totaling $21-60 USD for 3 months. All AWS resources are used within free tier and low-cost options, with no additional development costs. ROI period of 3-6 months through significant savings in manual operations time and improved teaching efficiency.

  
### 3. Solution Architecture

The system is designed following the AWS Well-Architected Framework with interconnected layers, ensuring data management capabilities, authentication, monitoring, real-time communication, ML ranking, notifications, and continuous event processing. The serverless architecture helps optimize costs and ensures automatic scalability.


![Platform Architecture Diagram](/images/2-Proposal/solution.drawio.png)

#### AWS Core Services Used

| **Service**            | **Primary Function**                                                         | **Key Benefits**                                                                                                                  |
| ---------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon Route 53**    | DNS management and routing traffic to CloudFront                             | Custom domain setup, health checks, geo-routing, SSL certificates integration                                                     |
| **Amazon CloudFront**  | CDN distribution for static website and frontend resources                   | Reduce global latency, Edge caching, HTTPS, support HTTP/2 and HTTP/3, integrate WAF/Shield, custom domain (ACM), OAC protects S3 |
| **AWS WAF**            | Web Application Firewall protection against attacks                          | Block malicious requests, rate limiting, IP filtering, CloudFront integration                                                     |
| **AWS Amplify**        | Deploy and host frontend dashboard with CI/CD                                | Easy build/deploy web apps, seamless integration with Cognito and AppSync                                                         |
| **AWS AppSync**        | Support GraphQL API with subscriptions for realtime chat                     | Real-time data updates via WebSockets, easy integration with DynamoDB and Lambda                                                  |
| **Amazon API Gateway** | Handle API requests and connect frontend-backend                             | Support RESTful APIs, throttling and caching to improve performance                                                               |
| **Amazon Cognito**     | Manage user authentication and authorization                                 | Support MFA, JWT tokens, easy integration with frontend                                                                           |
| **AWS Lambda**         | Execute CRUD logic without servers                                           | Auto-scale, pay-per-use, reduce operational costs                                                                                 |
| **Amazon DynamoDB**    | Store NoSQL data for student information and chat messages                   | Fast queries, auto-scale, support GSI for complex searches                                                                        |
| **Amazon CloudWatch**  | Monitor system logs and metrics                                              | Real-time monitoring, alarms to detect issues early                                                                               |
| **Amazon S3**          | Store static files for frontend dashboard                                    | Cheap static website hosting, high durability                                                                                     |
| **Amazon EventBridge** | Route events from management web to process exams (update scores, reminders) | Event-driven architecture, integrate with Lambda to automate workflows                                                            |
| **AWS Personalize**    | Build ML models to rank students based on activity data                      | Personalized ranking, auto-learn from data, support real-time inference                                                           |
| **AWS SES**            | Send ranking notification emails and system updates                          | Easy integration with Lambda, support millions of emails/month, low cost                                                          |
| **AWS CodeBuild**      | Build and test code from GitLab repository                                   | Auto compile, package artifacts, run unit tests in pipeline                                                                       |
| **AWS CodeDeploy**     | Deploy applications to AWS services                                          | Blue/green deployment, rollback capabilities, zero-downtime deployment                                                            |
| **AWS CodePipeline**   | Orchestrate CI/CD pipeline from source to deploy                             | Automate the entire process, integrate with GitLab/CodeBuild/CodeDeploy, support approvals and notifications.                     |
| **GitLab**             | Source control and trigger CI/CD pipeline                                    | Version control, merge requests, issue tracking, webhook integration                                                              |

#### Thiết kế thành phần

| **Layer**            | **Thành phần chính**                            | **Chức năng**                                |
| -------------------- | ----------------------------------------------- | -------------------------------------------- |
| **Edge Layer**       | Route53, CloudFront, WAF                        | DNS, CDN, layer security                     |
| **Frontend Layer**   | Amplify, AppSync, Cognito                       | Web interface, realtime chat, authentication |
| **Backend Layer**    | API Gateway, Lambda, DynamoDB                   | Logic processing, CRUD, data storage         |
| **Event & ML Layer** | EventBridge, SES, Personalize                   | Send notifications, process ML ranking       |
| **Monitoring Layer** | CloudWatch                                      | Logs, metrics, alerts                        |
| **CI/CD Layer**      | GitLab, CodePipeline, CodeBuild, CodeDeploy, S3 | Build & deploy automation                    |

#### Chú thích luồng chính

| **Luồng** | **Mô tả**                                                                              |
| --------- | -------------------------------------------------------------------------------------- |
| **(1)**   | User access via Route53 → CloudFront → Amplify (frontend).                             |
| **(2)**   | Amplify communicates with API Gateway or AppSync to send/receive data.                 |
| **(3.1)** | API Gateway → Lambda to handle logic (CRUD, events, etc.).                             |
| **(3.2)** | Cognito authenticates and returns tokens.                                              |
| **(4)**   | Lambda emits events via EventBridge.                                                   |
| **(4.1)** | EventBridge → SES sends reminder emails.                                               |
| **(4.2)** | EventBridge → Lambda → Personalize for ML ranking.                                     |
| **(5)**   | Dev pushes code to GitLab → CodePipeline pull → CodeBuild/CodeDeploy → Amplify/Lambda. |
| **(6)**   | CloudWatch collects logs and metrics.                                                  |
| **(7)**   | AppSync sends realtime data (chat/notifications) to the frontend.                      |


### 4. Technical Implementation

#### Implementation Phases
The project consists of 2 parts — developing a serverless student management system and building an intelligent analytics platform — each going through 4 phases:

1. **Research and Architecture Design**: Research AWS serverless patterns with DynamoDB and design architecture integrating realtime chat/ranking (1 month before internship).
2. **Cost Calculation and Feasibility Check**: Use AWS Pricing Calculator for estimation and adjust scope accordingly (Month 1).
3. **Architecture Adjustment for Cost/Solution Optimization**: Fine-tune (e.g., optimize Lambda with React SSR) to ensure efficiency (Month 2).
4. **Development, Testing, Deployment**: Program React frontend, AWS services with CDK/SDK and AppSync subscriptions, then test and deploy to production (Month 2–3).

#### Technical Requirements

**Student Management System**: Web dashboard (CRUD students, scores, courses), real-time chat, ML ranking. Frontend React/Next.js running on Amplify Hosting, using AppSync subscriptions to receive real-time chat messages and ranking updates. Cognito authenticates and authorizes all users, including 5-10 admins/teachers (with high privileges like CRUD data) and students (with limited privileges like viewing personal scores, joining chat).

**Intelligent Analytics Platform**: Practical knowledge of AWS Amplify (hosting React), Lambda (minimized due to SSR handling), AWS Personalize (ML ranking), DynamoDB (main NoSQL), AppSync (GraphQL + subscriptions), and EventBridge (event routing). Use AWS CDK/SDK for programming (e.g., EventBridge rules to Lambda for notifications). React SSR helps reduce Lambda load for fullstack web applications.
 

### 5. Roadmap & Key Milestones

The project is implemented over 14 weeks (from September to December 2025), divided into 6 main phases following the Agile model.

| **Phase**                              | **Timeline** | **Main Objective**         | **Deliverables**                                                          | **Success Criteria**                       |
| -------------------------------------- | ------------ | -------------------------- | ------------------------------------------------------------------------- | ------------------------------------------ |
| **Phase 1: Foundation Setup**          | Weeks 1–2    | Set up AWS environment     | • AWS account setup<br>• DynamoDB & Cognito config<br>• IaC templates<br> | • AWS infrastructure working stably<br>    |
| **Phase 2: Backend Deployment**        | Weeks 3–5    | Build API and logic        | • Lambda functions<br>• API Gateway & AppSync endpoints                   | • CRUD & chat mutations working            |
| **Phase 3: Frontend Integration**      | Weeks 6–7    | Deploy dashboard & chat    | • S3 + CloudFront hosting<br>• Realtime subscriptions integration         | • Dashboard & chat accessible in real-time |
| **Phase 4: ML & Ranking**              | Weeks 8-9    | Integrate Personalize      | • Datasets import<br>• Ranking API                                        | • Student ranking working                  |
| **Phase 5: Notification & Monitoring** | Weeks 10–11  | Integrate SES & monitoring | • Email notifications<br>• CloudWatch alarms<br>                          | • Notifications sent accurately            |
| **Phase 6: Testing & Review**          | Weeks 12–14  | Testing and finalization   | • End-to-end tests<br>• Documentation & Demo                              | • Stable system, complete demo             |


### 6. Budget Estimate

You can view the cost on [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=security-monitoring-2025) or download the [budget estimate file](./budget-estimate.xlsx).

#### Infrastructure Costs

**AWS Services:**


| **Service**            | **Usage Description**                                         | **Estimated Cost / month (USD)** | **Notes**                                                                           |
| ---------------------- | ------------------------------------------------------------- | -------------------------------- | ----------------------------------------------------------------------------------- |
| **Amazon API Gateway** | Process ~1M API calls/month                                   | ~$1.00 – $3.50                   | HTTP APIs: $1.00/million calls; REST: $3.50/million; free tier 1M calls.            |
| **AWS Lambda**         | ~1M requests, 400k GB-seconds                                 | ~$0.20 – $0.50                   | Requests: $0.20/million; Duration: $0.0000166667/GB-second; free tier sufficient.   |
| **Amazon DynamoDB**    | ~25 GB storage, 2.5M reads/writes                             | ~$0.25 – $1.25                   | Reads: $0.25/million; Writes: $1.25/million; Storage: $0.25/GB; free tier 25 GB.    |
| **Amazon Cognito**     | ~10k monthly active users                                     | ~$0.0055 – $0.015                | Essentials: $0.015/MAU; free tier 10k MAUs.                                         |
| **Amazon S3**          | ~5 GB storage, low requests                                   | ~$0.023 – $0.12                  | Storage: $0.023/GB; Requests: $0.0004/1k GET; free tier credits.                    |
| **Amazon CloudWatch**  | ~5 GB logs, 10 metrics/alarms                                 | ~$0.03 – $0.50                   | Logs: $0.50/GB ingestion; Storage: $0.03/GB; Metrics: $0.30/metric; free tier 5 GB. |
| **AWS AppSync**        | ~1M queries/subscriptions, realtime chat                      | ~$0.50 – $2.00                   | Requests: $4.00/million; Data transfer: $0.09/GB; free tier 250k requests.          |
| **AWS Personalize**    | Train model weekly, ~10k interactions                         | ~$0.50 – $5.00                   | Training: $0.25/hour; Inference: $0.00005/request; Storage: $0.05/GB.               |
| **AWS SES**            | ~10k emails/month                                             | ~$0.10 – $0.30                   | $0.10/1k emails; free tier 62k emails/first month.                                  |
| **Amazon EventBridge** | ~10k events/month, rules for exams                            | ~$0.10 – $0.50                   | $1.00/million events; free tier 100k events/month.                                  |
| **AWS Amplify**        | Hosting dashboard, few builds/month                           | ~$0.00 – $1.00                   | Build minutes: $0.01/minute; Hosting: $0.15/GB served; free tier available.         |
| **Amazon Route 53**    | DNS queries, hosted zones                                     | ~$0.50 – $1.00                   | Hosted zone: $0.50/zone/month; DNS queries: $0.40/million queries.                  |
| **AWS WAF**            | Web ACL evaluation, rules                                     | ~$1.00 – $3.00                   | Web ACL: $1.00/month; Rules: $0.60/million requests; managed rules additional.      |
| **AWS CodeBuild**      | ~10 builds/month, 100 minutes                                 | ~$0.00 – $0.50                   | $0.005/build minute; free tier 100 minutes/month.                                   |
| **AWS CodeDeploy**     | Application deployments                                       | ~$0.00 – $0.20                   | EC2/on-premise: $0.02/deployment; serverless free.                                  |
| **Amazon CloudFront**  | CDN delivers static content (Route53 & Amplify/S3 connection) | ~$0.10 – $1.00                   | First 1 TB/month free in Free Tier; $0.085/GB thereafter.                           |
| **AWS CodePipeline**   | CI/CD automation (GitLab connection → CodeBuild → CodeDeploy) | ~$1.00                           | $1.00/month for each active pipeline.                                               |

#### Total Estimate
| **Total cost / month (estimate)** | **Total 3 months (estimate)** | **Notes**                                                                                                          |
| --------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **~$7 – $20 / month**             | **~$21 – $60 / 3 months**     | Depends on actual usage (chat, ranking, emails, events); leverage free tier. Additional Route53, WAF, CI/CD costs. |

#### Notes
- All main services are within first year Free Tier.
- Can reduce further if using AWS Educate / Credits for Students.
- Personalize & SES costs based on volume; optimize with batch processing.
- EventBridge costs based on number of events; optimize with filter rules.


### 7. Risk Assessment

Based on NIST Risk Management Framework, the project team identifies key risks and mitigation measures.

| **Risk Code**                     | **Description**                            | **Level**  | **Mitigation**                                               |
| --------------------------------- | ------------------------------------------ | ---------- | ------------------------------------------------------------ |
| **R1 – Data Leakage**             | Data exposure due to misconfiguration      | **High**   | Apply Cognito auth, IAM least privilege, DynamoDB encryption |
| **R2 – API Overload**             | Too many requests causing slowdown         | **Medium** | Throttling API Gateway/AppSync, CloudWatch alarms            |
| **R3 – Lambda Cold Start**        | Delay on invocation                        | **Medium** | Optimize code, Provisioned Concurrency if needed             |
| **R4 – Cost Overrun**             | Unusual usage increase (high chat, emails) | **Medium** | AWS Budgets alerts, monitor Cost Explorer                    |
| **R5 – Service Downtime**         | AWS interruption                           | **Low**    | Multi-AZ config, DynamoDB backups                            |
| **R6 – Realtime Latency**         | Chat delay due to subscriptions            | **Medium** | Use AppSync caching, test with high load                     |
| **R7 – ML Accuracy**              | Inaccurate ranking due to poor data        | **Medium** | Validate datasets, periodic retraining with Lambda           |
| **R8 – Event Processing Failure** | Error routing exam events                  | **Medium** | Dead-letter queues for EventBridge, test rules               |

**Contingency Plan (Summary):**
- Recovery: Use CloudFormation for fast rebuild.
- Communication: CloudWatch alarms send emails.
- Continuous Improvement: Weekly review, including chat performance, ranking, and event handling.


### 8. Expected Results

**Technical Results:**
- Complete serverless student management system with full CRUD, real-time chat, ML ranking, email notifications, and exam event processing.
- Integration of API Gateway, Lambda, DynamoDB, Cognito, S3, Amplify, CloudWatch, AppSync, Personalize, SES, EventBridge.
- Response time <1 second, chat latency <2 seconds, event processing <5 seconds, uptime ≥99%.
- Actual cost ≤ $20/3 months.

**Learning and Training Results:**
- Learners master serverless development, real-time apps, and event-driven architecture.
- Prepare for AWS Developer Associate certification.
- Develop IaC, NoSQL, Authentication, GraphQL, Personalization, Events skills.

**Professional & Presentation Results:**
- Report with dashboard, chat, ranking, email, and event demo.
- Demo CRUD, real-time chat, notifications, and exam event processing.
- Deployment guide documentation.

**Long-term Value:**
- Easy to expand to mobile app or advanced AI analytics.
- Platform for serverless, real-time, and event-driven training labs.


---