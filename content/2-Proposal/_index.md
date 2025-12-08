---
title: "Proposal"
date: 2025-11-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
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

The solution creates a foundation for IT students to develop AWS serverless skills while providing efficient management tools for teachers in instruction and assessment. The platform reduces manual reporting for each class through centralized systems, simplifies management and maintenance, and improves data reliability. Monthly costs estimated at $7-20 USD based on the detailed budget estimate, totaling $21-60 USD for 3 months. Maximize AWS Free Tier (first year) and AWS Educate credits to reduce costs to ~$5-10/month during development phase. No additional development or licensing costs. ROI period of 3-6 months through significant savings in manual operations time (estimated 5-10 hours/week) and improved teaching efficiency.

  
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
| **GitLab**             | Source code management and trigger CI/CD pipeline                            | Version control (GitLab.com or self-hosted), merge requests, issue tracking, webhook integration with CodePipeline for automated deploy.                                              |
| **AWS CodePipeline**   | Orchestrate CI/CD pipeline from source to deploy                             | Automate entire process from GitLab to production, integrate with CodeBuild/CodeDeploy, support approvals and notifications.                     |
| **AWS CodeBuild**      | Build and test code from GitLab repository                                   | Auto compile, package artifacts, run unit tests in pipeline, support multi-environment builds.                                                                       |
| **AWS CodeDeploy**     | Deploy applications to AWS services                                          | Support blue/green deployment, automatic rollback, zero-downtime deployments for Lambda/Amplify.                                                            |

#### Component Design

| **Layer**            | **Core Components**                             | **Functions**                                |
| -------------------- | ----------------------------------------------- | -------------------------------------------- |
| **Edge Layer**       | Route53, CloudFront, WAF                        | DNS, CDN, layer security                     |
| **Frontend Layer**   | Amplify, AppSync, Cognito                       | Web interface, realtime chat, authentication |
| **Backend Layer**    | API Gateway, Lambda, DynamoDB                   | Logic processing, CRUD, data storage         |
| **Event & ML Layer** | EventBridge, SES, Personalize                   | Send notifications, process ML ranking       |
| **Monitoring Layer** | CloudWatch                                      | Logs, metrics, alerts                        |
| **CI/CD Layer**      | GitLab, CodePipeline, CodeBuild, CodeDeploy, S3 | Build & deploy automation                    |

#### Main Flow Annotations

| **Flow**  | **Description**                                                                        |
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
The project is implemented in **5 weeks** with 5 corresponding main phases:

1. **Week 1 - Foundation Setup**
   - **Day 1-2**: Set up AWS account, configure IAM roles/policies, set up billing alerts
   - **Day 3-4**: Create DynamoDB tables (Students, Courses, Grades, ChatMessages, Assignments, Attendance, Events) with GSI, configure Cognito User Pools with groups (Admin/Teacher/Student)
   - **Day 5-7**: Initialize IaC templates (AWS CDK/CloudFormation), research serverless patterns, design detailed architecture with sequence diagrams, setup Git repo structure for parallel development
   - **Estimated Cost**: $0-2/week (mostly free tier)

2. **Week 2 - Backend & Frontend Parallel (Parallel Development Phase 1)**
   - **Backend Team (Day 1-7)**:
     - Build 50+ Lambda functions for: Students (CRUD, search, bulk import/export), Courses (CRUD, enrollment), Grades (CRUD, analytics, statistics), Assignments (CRUD, submissions), Attendance (CRUD, reports), Chat (send, receive, history), Auth (login, register, refresh token), Notifications (preferences, history)
     - Set up API Gateway with 50+ REST endpoints: `/students/*` (10 endpoints), `/courses/*` (8 endpoints), `/grades/*` (12 endpoints), `/assignments/*` (8 endpoints), `/attendance/*` (6 endpoints), `/chat/*` (6 endpoints)
     - Unit testing with 80%+ coverage
   - **Frontend Team (Day 1-7)**:
     - Deploy Amplify hosting with React/Next.js, setup routing (React Router)
     - Build UI components: Layout (Header, Sidebar, Footer), Authentication (Login, Register), Dashboard (Overview, Stats cards)
     - Setup API client (Axios/Fetch), state management (Redux/Zustand), form validation (React Hook Form)
   - **Estimated Cost**: $1-3/week

3. **Week 3 - Backend & Frontend Parallel (Parallel Development Phase 2)**
   - **Backend Team (Day 1-7)**:
     - Build AppSync GraphQL schema with 50+ operations: queries (getStudent, listStudents, searchCourses, getGrades, etc.), mutations (createStudent, updateGrade, submitAssignment, etc.), subscriptions (onMessageSent, onGradeUpdated, onNotification)
     - Integrate EventBridge rules and Lambda handlers for automated workflows
     - Integration testing with Postman/Newman collections
   - **Frontend Team (Day 1-7)**:
     - Build 15+ pages: Student Management (List, Create, Edit, Detail, Import), Course Management (List, Create, Edit, Enrollment), Grade Management (List, Input, Analytics), Assignment (List, Submit, Review), Attendance (Tracker, Reports), Chat (Room, History), Profile Settings
     - Integrate AppSync subscriptions for realtime chat, notifications, live grade updates
     - Responsive design (mobile + tablet + desktop)
   - **Integration (Day 6-7)**:
     - Connect CloudFront CDN with Route53, configure WAF rules (rate limiting, geo-blocking), SSL/TLS certificates
     - End-to-end testing between Frontend and Backend
   - **Estimated Cost**: $2-5/week

4. **Week 4 - ML & Notifications (Advanced Features)**
   - **Day 1-2**: Prepare dataset for Personalize (interaction data, user-item interactions), create solution version
   - **Day 3-4**: Integrate EventBridge rules (grade update → email notification, weekly ranking trigger), Lambda handlers
   - **Day 5-6**: Configure SES (verify domain, email templates), set up CI/CD pipeline (GitLab → CodePipeline → CodeBuild → CodeDeploy)
   - **Day 7**: Testing ML ranking accuracy, notification delivery, pipeline automation
   - **Estimated Cost**: $3-8/week (Personalize training cost)

5. **Week 5 - Testing & Demo (Testing & Launch)**
   - **Day 1-2**: End-to-end testing (user flows, edge cases), load testing with 100+ concurrent users
   - **Day 3-4**: Performance optimization (Lambda memory tuning, DynamoDB capacity adjustment, CloudFront caching)
   - **Day 5-6**: Finalize documentation (architecture diagrams, API docs, deployment guide, user manual)
   - **Day 7**: Demo preparation, presentation slides, video recording
   - **Estimated Cost**: $1-2/week

#### Technical Requirements

**Student Management System**: Full-featured web dashboard with 8 main modules (Students, Courses, Grades, Assignments, Attendance, Chat, Notifications, Analytics), real-time chat, ML ranking. Frontend React/Next.js running on Amplify Hosting with 15+ pages and 50+ components, using AppSync subscriptions to receive real-time chat messages and ranking updates. Cognito authenticates and authorizes all users, including 5-10 admins/teachers (with high privileges like CRUD data) and students (with limited privileges like viewing personal scores, joining chat).

**Comprehensive API Architecture**: 50+ REST API endpoints via API Gateway (CRUD operations, search, bulk actions) and 50+ GraphQL operations via AppSync (queries, mutations, subscriptions) for a total of 100+ API operations. Backend developed in parallel with Frontend to optimize development time.

**Intelligent Analytics Platform**: Practical knowledge of AWS Amplify (hosting React), Lambda (50+ functions processing business logic), AWS Personalize (ML ranking), DynamoDB (7 tables with GSI), AppSync (GraphQL + subscriptions), and EventBridge (event routing). Use AWS CDK/SDK for IaC programming and automation. Parallel development methodology with Git branching strategy for Backend/Frontend teams.
 

### 5. Roadmap & Key Milestones

To align with the internship timeline, the project is implemented in **5 weeks** with 5 main phases:

| **Phase**                       | **Timeline** | **Main Objective**          | **Deliverables**                                                     | **Success Criteria** | **Cost** |
| ------------------------------- | ------------ | --------------------------- | -------------------------------------------------------------------- | ------------------------------------- | -------- |
| **1: Foundation Setup**         | Week 1       | Set up AWS environment      | • AWS account with IAM setup<br>• 7 DynamoDB tables with GSI<br>• Cognito User Pool<br>• IaC templates (CDK/CloudFormation)<br>• Git repo structure | • Infrastructure as Code complete<br>• Security baseline met<br>• Parallel dev environment ready | $0-2 |
| **2: Backend & Frontend Parallel (Phase 1)** | Week 2 | Build core Backend + Frontend foundation | • 50+ Lambda functions<br>• API Gateway with 50+ REST endpoints<br>• React/Next.js app foundation<br>• 10+ UI components<br>• Unit tests (>80% coverage) | • 50+ API endpoints working<br>• API response time <500ms<br>• Frontend routing setup<br>• All backend tests passed | $1-3 |
| **3: Backend & Frontend Parallel (Phase 2)** | Week 3 | GraphQL + Full UI integration | • AppSync with 50+ GraphQL operations<br>• 15+ complete pages<br>• Realtime subscriptions<br>• CloudFront + Route53 + WAF<br>• Responsive design | • GraphQL API fully functional<br>• All pages integrated with backend<br>• Chat latency <2s<br>• SSL/HTTPS enabled<br>• Mobile responsive | $2-5 |
| **4: ML & Notifications**       | Week 4       | Integrate Personalize & SES | • Personalize dataset + solution<br>• EventBridge rules (5+ events)<br>• SES email templates<br>• CI/CD pipeline (GitLab → AWS) | • ML ranking accuracy >70%<br>• Email delivery rate >95%<br>• Automated deployment working | $3-8 |
| **5: Testing & Demo**           | Week 5       | Testing and finalization    | • Load test reports (100+ users)<br>• Performance optimization<br>• Complete documentation<br>• Demo video + slides | • System uptime ≥99%<br>• All features stable<br>• Documentation complete<br>• Demo ready | $1-2 |


### 6. Budget Estimate

You can download the [budget estimate file](/images/2-Proposal/budget-estimate.pdf) to view detailed costs.

#### Infrastructure Costs

**AWS Services:**


| **Service**            | **Usage Description**                                         | **Estimated Cost / month (USD)** | **Notes**                                                                           |
| ---------------------- | ------------------------------------------------------------- | -------------------------------- | ----------------------------------------------------------------------------------- |
| **Amazon API Gateway** | Process ~100k-500k API calls/month                            | ~$0.00 – $1.75                   | HTTP APIs: $1.00/million calls; REST: $3.50/million; free tier 1M calls first.            |
| **AWS Lambda**         | ~200k-500k requests, 100k-200k GB-seconds                     | ~$0.00 – $0.20                   | Requests: $0.20/million; Duration: $0.0000166667/GB-second; free tier 1M requests + 400k GB-seconds.   |
| **Amazon DynamoDB**    | ~5-10 GB storage, 250k-500k reads/writes                      | ~$0.00 – $0.75                   | Reads: $0.25/million; Writes: $1.25/million; Storage: $0.25/GB; free tier 25 GB + 25 RCU/WCU.    |
| **Amazon Cognito**     | ~100-200 monthly active users                                 | ~$0.00                            | Essentials: $0.015/MAU; free tier 10k MAUs first (enough for 50-100 students + admins).                                         |
| **Amazon S3**          | ~5 GB storage, low requests                                   | ~$0.023 – $0.12                  | Storage: $0.023/GB; Requests: $0.0004/1k GET; free tier credits.                    |
| **Amazon CloudWatch**  | ~5 GB logs, 10 metrics/alarms                                 | ~$0.03 – $0.50                   | Logs: $0.50/GB ingestion; Storage: $0.03/GB; Metrics: $0.30/metric; free tier 5 GB. |
| **AWS AppSync**        | ~1M queries/subscriptions, realtime chat                      | ~$0.50 – $2.00                   | Requests: $4.00/million; Data transfer: $0.09/GB; free tier 250k requests.          |
| **AWS Personalize**    | Train model weekly, ~10k interactions                         | ~$0.50 – $5.00                   | Training: $0.25/hour; Inference: $0.00005/request; Storage: $0.05/GB.               |
| **AWS SES**            | ~10k emails/month                                             | ~$0.10 – $0.30                   | $0.10/1k emails; free tier 62k emails/first month.                                  |
| **Amazon EventBridge** | ~10k events/month, rules for exams                            | ~$0.00                            | $1.00/million events; free tier 100k events/month first (10k events completely free).                                  |
| **AWS Amplify**        | Hosting dashboard, few builds/month                           | ~$0.00 – $1.00                   | Build minutes: $0.01/minute; Hosting: $0.15/GB served; free tier available.         |
| **Amazon Route 53**    | DNS queries, hosted zones                                     | ~$0.50 – $1.00                   | Hosted zone: $0.50/zone/month; DNS queries: $0.40/million queries.                  |
| **AWS WAF**            | Web ACL evaluation, rules                                     | ~$1.00 – $3.00                   | Web ACL: $1.00/month; Rules: $0.60/million requests; managed rules additional.      |
| **AWS CodeBuild**      | ~10 builds/month, 100 minutes                                 | ~$0.00 – $0.50                   | $0.005/build minute; free tier 100 minutes/month.                                   |
| **AWS CodeDeploy**     | Application deployments                                       | ~$0.00 – $0.20                   | EC2/on-premise: $0.02/deployment; serverless free.                                  |
| **Amazon CloudFront**  | CDN delivers static content (Route53 & Amplify/S3 connection) | ~$0.10 – $1.00                   | First 1 TB/month free in Free Tier; $0.085/GB thereafter.                           |
| **AWS CodePipeline**   | CI/CD automation (GitLab connection → CodeBuild → CodeDeploy) | ~$1.00                           | $1.00/month for each active pipeline.                                               |

#### Total Estimate
| **Total cost / month (estimate)** | **Total 3 months (estimate)** | **With AWS Free Tier** | **Notes**                                                                                                          |
| --------------------------------- | ----------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **~$7 – $20 / month**             | **~$21 – $60 / 3 months**     | **~$15 – $30 / 3 months** | Depends on actual usage (chat, ranking, emails, events). Leverage Free Tier (first year) + AWS Educate credits can reduce 50% cost. Highest cost in week 4 due to Personalize training. |

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
- **API Architecture**: 50+ REST endpoints (API Gateway) + 50+ GraphQL operations (AppSync) for total 100+ API operations, 50+ Lambda functions processing business logic.
- **Frontend**: 15+ responsive pages, 50+ UI components, realtime subscriptions for chat and notifications.
- Integration of 17 AWS services: API Gateway, Lambda, DynamoDB, Cognito, S3, Amplify, CloudWatch, AppSync, Personalize, SES, EventBridge, Route53, CloudFront, WAF, CodePipeline, CodeBuild, CodeDeploy.
- **Performance targets**: API response <500ms, chat latency <2s, event processing <5s, system uptime ≥99%.
- **Actual cost**: $7-20/month, total ~$21-60 for 3 months (can reduce to $15-30 with AWS Free Tier and Educate credits).

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