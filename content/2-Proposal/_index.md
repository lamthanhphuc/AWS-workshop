---
title: "Proposal"
date: 2025-11-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Student Management System

Download [Proposal Template](/document/ProposalTemplate.docx) for detailed cost information.

### 1. Executive Summary

Serverless Student Management Platform is a cloud-based student management platform designed for educational institutions and small businesses to enhance the ability to manage, analyze, and interact with student data. The platform supports up to 50-100 students initially, with the ability to flexibly scale up to 500–1000 students without major infrastructure changes, using AWS Serverless services such as Lambda, DynamoDB, API Gateway.

### 2. Problem Statement

**Current Problem**

Current student management systems require manual data entry, which is difficult to manage when there are many classes. There is no centralized system for real-time data or analytics, and third-party platforms are often expensive and too complex.

**Solution**

The platform uses Amazon API Gateway to receive REST requests, AWS Lambda to handle business logic, Amazon DynamoDB to store student data and scores. AWS Amplify with React/TypeScript provides the web interface, and Amazon Cognito ensures secure access. Similar to traditional LMS systems but at a lower cost, users can register new students and manage information, but the platform operates at a smaller scale and serves learning purposes.

**Benefits and ROI**

The solution creates a basic foundation for IT students to develop serverless AWS skills, while providing effective management tools for teachers to support teaching and assessment. The platform reduces manual reporting for each class through a centralized system, simplifies management and maintenance, and improves data reliability. The estimated monthly cost is $7-20 USD according to the detailed budget estimate, totaling $40-60 USD for 3 months.

### 3. Solution Architecture

The system is designed according to the AWS Well-Architected Framework architecture with interconnected layers, ensuring data management, authentication, and monitoring capabilities. The serverless architecture helps optimize costs and ensure automatic scalability.

<!-- ![Cloud Security & Monitoring System Architecture](/images/2-Proposal/edge_architecture.jpeg) -->

![Platform Architecture Diagram](/images/2-Proposal/Solution.drawio.png)

#### AWS Core Services Used

| **Services** | **Key Features** | **Key Benefits** |
| ---------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **Amazon Route 53** | Manage DNS and route traffic to CloudFront | Set up custom domains, health checks, geo-routing, SSL integration to ensure secure and fast access. |
| **Amazon CloudFront** | CDN delivery for static content and frontend resources | Global latency reduction, caching at edge locations, HTTPS support, WAF integration for protection and OAC for S3. |
| **AWS WAF** | Firewall to protect web applications from attacks | Block malicious requests, rate limiting, IP filtering, seamless integration with CloudFront to secure traffic. |
| **AWS Amplify** | Frontend hosting and deployment with CI/CD | Build and deploy web applications quickly, integrate with Cognito/AppSync for user-friendly interface. |
| **Amazon API Gateway** | Handle and route API requests from frontend to backend | Support REST/HTTP APIs, throttling, caching, and authorizer for increased performance and security. |
| **Amazon Cognito** | Manage user authentication and authorization | Support MFA, JWT tokens, groups for permissions (teacher/admin vs. student), easy integration with AppSync/API Gateway. |
| **AWS Lambda** | Execute backend logic and event handling | Serverless, auto-scaling, pay-per-use, CRUD processing. |
| **Amazon DynamoDB** | NoSQL data storage for student information and assignments | Fast query, auto-scaling, Global Secondary Indexes (GSI) support for complex searches and low cost. |
| **Amazon CloudWatch** | Monitor logs, metrics, and system alarms | Real-time monitoring, set alarms to detect problems early, integrate with Lambda/DynamoDB for optimization. |
| **Amazon S3** | Store artifacts and builds from CI/CD | Cheap, highly sustainable static file hosting, integrated with CodePipeline to store deployed artifacts. |
| **GitLab** | Manage source code and trigger CI/CD pipeline | Version control (GitLab.com or self-hosted), merge requests, issue tracking, webhooks integrated with CodePipeline to automate deployment. |
| **AWS CodePipeline** | Orchestrate CI/CD pipeline from source to deploy | Automate the entire process from GitLab to production, integrated with CodeBuild/CodeDeploy, support approvals and notifications. |
| **AWS CodeBuild** | Build and test code from GitLab repository | Automatically compile, package artifacts, run unit tests in pipeline, support multi-environment builds. |
| **AWS CodeDeploy** | Deploy applications to AWS services | Support blue/green deployment, automatic rollback, zero-downtime deployments for Lambda/Amplify. |

#### Component Design

| **Layer** | **Main Components** | **Functionality** |
| -------------------- | -------------------------------------------------- | -------------------------------------- |
| **Edge Layer** | Route53, CloudFront, WAF | DNS, CDN, security layer |
| **Frontend Layer** | Amplify, Cognito | Web interface, authentication |
| **Backend Layer** | API Gateway, Lambda, DynamoDB | Logic processing, CRUD, data storage |
| **Monitoring Layer** | CloudWatch | Logs, metrics, alerts |
| **CI/CD Layer** | GitLab, CodePipeline, CodeBuild, CodeDeploy, S3 | Build & deploy automation |

#### Main Flow Annotation

| **Flow** | **Description** |
| --------- | -------------------------------------------------------------------------------------------------- |
| **(1)** | User accesses via Route53 → CloudFront → Amplify (frontend). |
| **(2)** | Amplify communicates with API Gateway to send/receive data. |
| **(3.1)** | API Gateway → Lambda to handle logic (CRUD, etc.). |
| **(3.2)** | Cognito authenticates and returns tokens. |
| **(4)** | Dev pushes code to GitLab → CodePipeline pull → CodeBuild/CodeDeploy → Amplify/Lambda. |
| **(5)** | CloudWatch collects logs and metrics. |

### 4. Technical Implementation

#### Implementation Phases
The project was implemented in **5 weeks** with 5 main phases:

1. **Week 1 - Foundation Setup**
- **Days 1-2**: Set up AWS account, configure IAM roles/policies, set up billing alerts
- **Days 3-4**: Create DynamoDB tables (Students, Courses, Grades, Assignments) with GSI, configure Cognito User Pools with groups (Admin/Teacher/Student)
- **Days 5-7**: Initialize IaC templates (AWS CDK/CloudFormation), research serverless patterns, design detailed architecture with sequence diagrams, setup Git repo structure for parallel development

2. **Week 2 - Backend & Frontend Parallel (Parallel Development Phase 1)**
- **Backend Team (Day 1-7)**: 
- Build 50+ Lambda functions for: Students (CRUD, search, bulk import/export), Courses (CRUD, enrollment), Grades (CRUD, analytics, statistics), Assignments (CRUD, submissions), Auth (login, registration, refresh token) 
- Set up API Gateway with 50+ REST endpoints: `/students/*` (10 endpoints), `/courses/*` (8 endpoints), `/grades/*` (12 endpoints), `/assignments/*` (8 endpoints)
- Unit testing with 80%+ coverage 
- **Frontend Team (July 1)**: 
- Deploy Amplify hosting with React/TypeScript, setup routing (React Router) 
- Build UI components: Layout (Header, Sidebar, Footer), Authentication (Login, Register), Dashboard (Overview, Stats cards) 
- Setup API client (Axios/Fetch), state management (Redux/Zustand), form validation (React Hook Form)


3. **Week 3 - Backend & Frontend Parallel (Parallel Development Phase 2)** 
- **Backend Team (Day 1-7)**: 
- Integrate Lambda handlers for automated workflows 
- Integration testing with Postman/SWAGGER 
- **Frontend Team (July 1)**: 
- Build 15+ pages: Student Management (List, Create, Edit, Detail, Import), Course Management (List, Create, Edit, Enrollment), Grade Management (List, Input, Analytics), Assignment (List, Submit, Review)
- **Integration (Days 6-7)**: 
- Connect CloudFront CDN with Route53, configure WAF rules (rate limiting, geo-blocking), SSL/TLS certificates 
- End-to-end testing between Frontend and Backend

4. **Week 4 - CI/CD with CodeBuild, CodeDeploy, CodePipeline**
- **Day 1-2**: Learn and configure AWS CodeBuild for frontend and backend (buildspec, build environment, save artifacts to S3)
- **Day 3-4**: Set up AWS CodePipeline to automate the build and deploy process from GitLab to AWS environment (connect stages: source, build, deploy)
- **Day 5-6**: Configure AWS CodeDeploy to deploy backend (Lambda or EC2) and frontend (Amplify or S3), test the automated deployment process
- **Day 7**: Test the entire pipeline, ensure the build, automated deployment works stably, record results and optimize the pipeline

5. **Week 5 - Testing & Demo (Testing & Launch)**
- **Day 1-2**: End-to-end testing (user flows, edge cases), load testing with 100+ concurrent users 
- **Day 3-4**: Performance optimization (Lambda memory tuning, DynamoDB capacity adjustment, CloudFront caching) 
- **Day 5-6**: Complete documentation (architecture diagrams, API docs, deployment guide, user manual) 
- **Day 7**: Demo preparation, presentation slides, video recording

#### Technical Requirements

**Student Management System**: Full web dashboard with 5 main modules (Students, Courses, Grades, Assignments). Frontend React/TypeScript running on Amplify Hosting with 15+ pages and 50+ components. Cognito authenticates and authorizes all users, including 5-10 admins/teachers (with high permissions like CRUD data) and students (with limited permissions like viewing scores, classes).

**Comprehensive API architecture**: 50+ REST API endpoints via API Gateway (CRUD operations, search, bulk actions). Backend built in parallel with Frontend to optimize development time.

**Smart Analytics Platform**: Practical knowledge of AWS Amplify (hosting React), Lambda (50+ business processing functions), DynamoDB (5 tables with GSI). Use AWS CDK/SDK for IaC programming and automation. Parallel development methodology with Git branching strategy for Backend/Frontend teams.

### 5. Roadmap & Milestones

To fit the internship time, the project is implemented in **5 weeks** with 5 main phases:

| **Phase** | **Time** | **Main Objective** | **Deliverables** | **Success Criteria** |
| --------------------------- | ------------- | --------------------------- | --------------------------------------------------------------------------- | ------------------------------------------ |
| **1: Setting up the foundation** | Week 1 | Setting up the AWS environment | • AWS account with IAM setup<br>• 7 DynamoDB tables with GSI<br>• Cognito User Pool<br>• IaC templates (CDK/CloudFormation)<br>• Git repo structure | • Infrastructure as Code complete<br>• Security baseline up to standard<br>• Parallel dev environment ready |
| **2: Backend & Frontend Parallel (Phase 1)** | Week 2 | Build core Backend + Frontend foundation | • 50+ Lambda functions<br>• API Gateway with 50+ REST endpoints<br>• React/TypeScript app foundation<br>• 10+ UI components<br>• Unit tests (>80% coverage) | • 50+ active API endpoints<br>• API response time <500ms<br>• Frontend routing setup<br>• All backend tests passed |
| **3: Backend & Frontend Parallel (Phase 2)** | Week 3 | Integrating Lambda workflows & integration testing | • Integrating Lambda handlers for automated workflows<br>• Integration testing with Postman/SWAGGER<br>• 15+ complete pages<br>• CloudFront + Route53 + WAF<br>• Responsive design | • Automated workflows work well<br>• Integration tested with Postman/SWAGGER<br>• All pages integrated with backend<br>• SSL/HTTPS enabled<br>• Mobile responsive |
| **4: CI/CD with CodeBuild, CodeDeploy, CodePipeline** | Week 4 | Set up and test automated CI/CD process | • Configure CodeBuild for frontend/backend<br>• Set up CodePipeline connecting GitLab, CodeBuild, CodeDeploy<br>• Test automated deployment for backend/frontend | • Build and deploy automation work well<br>• Artifacts stored correctly<br>• Pipeline optimized |
| **5: Test & Demo** | Week 5 | Test and finalize | • Load test reports (50+ users)<br>• Performance optimization<br>• Complete documentation<br>• Demo video + slides | • System uptime ≥99%<br>• All features stable<br>• Documentation complete<br>• Demo ready |

### 6. Budget Estimate

Download [budget estimate file](/images/2-Proposal/MyEstimate.pdf) for detailed cost information.

#### Infrastructure Costs


**AWS Services:**

| **Services**           | **Usage Description**                        | **Estimated Cost / Month (USD)** | **Notes** |
|------------------------|----------------------------------------------|-------------------------------|-------------------------------|
| **Amazon API Gateway** | Handles ~100k-500k API calls/month           | $3.8                          | HTTP APIs, REST APIs, first 1M calls free |
| **AWS Lambda**         | ~200k-500k requests, 100k-200k GB-sec        | $0.5                          | Requests, GB-sec, 1M requests + 400k GB-sec free |
| **Amazon DynamoDB**    | ~5-10 GB storage, 250k-500k reads/writes     | $1                            | Reads, Writes, Storage, 25 GB + 25 RCU/WCU free |
| **Amazon Cognito**     | ~100-200 monthly active users                | $0                            | First 10k users free |
| **Amazon S3**          | ~5 GB storage, low requirements              | $0.13                         | Storage, requests, free credits available |
| **Amazon CloudWatch**  | ~5 GB logs, 10 metrics/alarms                | $4                            | Logs, metrics, alarms, 5 GB free |
| **AWS Amplify**        | Dashboard hosting, few builds/month          | $1                            | Build minutes, storage, free credits available |
| **Amazon Route 53**    | DNS queries, storage zones                   | $1                            | Storage zone, DNS queries |
| **AWS WAF**            | Web ACL Assessment, Rules                    | $7                            | Web ACL, rules, rule management costs extra |
| **AWS CodeBuild**      | ~10 builds/month, 100 minutes                | $0.9                          | $0.005/min build, 100 minutes/month free |
| **AWS CodeDeploy**     | Application Deployment                       | $0.8                          | EC2/on-premises, serverless free |
| **Amazon CloudFront**  | Static Content Delivery CDN                  | $1                            | First 1 TB/month free, $0.085/GB thereafter |
| **AWS CodePipeline**   | CI/CD automation (GitLab → CodeBuild → CodeDeploy) | $1                       | $1/month for each active pipeline |

#### Estimated total
| **Total cost / month (estimated)** | **3-month total (estimated)** | **With AWS Free Tier** | **Notes** |
| ------------------------------------ | ---------------------------- | ------------------------------------------------------------------------------------------------------------------ |--------------|
| **~$7 – $20 / month** | **~$40 – $60 / 3 months** | **~$15 – $30 / 3 months** | Depends on actual usage. Take advantage of Free Tier (first year) + AWS Educate credits to reduce costs by 50%. |

#### Note
- All core services are included in the first year free plan.
- Additional discounts may apply if using AWS Educate / Student Credits.

### 7. Risk Assessment

Based on the NIST Risk Management Framework, the project team identified key risks and mitigation measures.

| **Risk Code** | **Description** | **Level** | **Mitigation** |
| --------------------------------- | ---------------------------------------- | -------------- | -------------------------------------------------------------- |
| **R1 – Data Leakage** | Data exposure due to incorrect configuration | **High** | Apply Cognito auth, IAM least privilege, DynamoDB encryption |
| **R2 – API Overload** | Too many requests causing slowness | **Medium** | Throttling API Gateway/AppSync, CloudWatch alarms |
| **R3 – Lambda Cold Start** | Delay when invoking | **Medium** | Optimize code, Provisioned Concurrency if needed |
| **R4 – Cost Overrun** | Unusual usage increase (high emails) | **Medium** | AWS Budgets alerts, monitor Cost Explorer |
| **R5 – Service Downtime** | AWS Outage | **Low** | Multi-AZ config, backups DynamoDB |

**Contingency Plan (Summary):**
- Recovery: Use CloudFormation to quickly recreate infrastructure in the event of a failure or deployment error.

- Monitoring & Alerting: Set up CloudWatch alarms to detect performance, security, and cost issues early, send email notifications to the operations team.

- Continuous Improvement: Periodically evaluate CI/CD processes, automate testing, optimize pipelines, and update documentation to ensure the system is always stable and easy to maintain.

### 8. Expected Results

**Technical Results:**
- Complete a serverless student management system with an automated CI/CD process, ensuring fast and stable build, test, and deployment.
- API fully supports student management functions, integrates Lambda workflows, integration testing with Postman/SWAGGER.

- Modern frontend with React/TypeScript, 15+ pages, 50+ UI components, realtime connection with backend.

- Integrates key AWS services: API Gateway, Lambda, DynamoDB, Cognito, S3, Amplify, CloudWatch, Route53, CloudFront, WAF, CodePipeline, CodeBuild, CodeDeploy.

- Performance: API response <500ms, uptime ≥99%, CI/CD process optimizes cost and deployment time.

- Actual cost: $7-20/month, total ~$21-60 for 3 months (can be reduced to $15-30 with AWS Free Tier and Educate credits).

**Long-term value:**
- Easily expandable to mobile apps or advanced AI/analytics integration.
- A practice and training platform for serverless, CI/CD, and DevOps for students and small businesses.