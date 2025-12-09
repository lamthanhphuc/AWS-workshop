---
title: "Event 2"
date: "2006-01-02"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #2 - DevOps on AWS"

### Event Objectives

- Share DevOps culture and principles on AWS
- Guide on building complete CI/CD pipelines
- Introduce Infrastructure as Code with CloudFormation and CDK
- Practice deploying container services and monitoring
- Apply DevOps best practices to enterprise environments

### Key Highlights

#### Morning Session (8:30 AM – 12:00 PM)

**8:30 – 9:00 AM | Welcome & DevOps Mindset**
- DevOps culture and principles
- Benefits and key metrics (DORA, MTTR, deployment frequency)
- Importance of collaboration between Dev and Ops

**9:00 – 10:30 AM | AWS DevOps Services – CI/CD Pipeline**
- **Source Control**: AWS CodeCommit, Git strategies (GitFlow, Trunk-based)
- **Build & Test**: CodeBuild configuration, testing pipelines
- **Deployment**: CodeDeploy with Blue/Green, Canary, Rolling updates
- **Orchestration**: CodePipeline automation
- **Demo**: Full CI/CD pipeline walkthrough

**10:45 AM – 12:00 PM | Infrastructure as Code (IaC)**
- **AWS CloudFormation**: Templates, stacks, drift detection
- **AWS CDK**: Constructs, reusable patterns, language support
- **Demo**: Deploying with CloudFormation and CDK
- **Discussion**: Choosing between IaC tools

#### Afternoon Session (1:00 – 5:00 PM)

**1:00 – 2:30 PM | Container Services on AWS**
- **Docker Fundamentals**: Microservices and containerization
- **Amazon ECR**: Image storage, scanning, lifecycle policies
- **Amazon ECS & EKS**: Deployment strategies, scaling, orchestration
- **AWS App Runner**: Simplified container deployment
- **Demo & Case Study**: Microservices deployment comparison

**2:45 – 4:00 PM | Monitoring & Observability**
- **CloudWatch**: Metrics, logs, alarms, dashboards
- **AWS X-Ray**: Distributed tracing and performance insights
- **Demo**: Full-stack observability setup
- **Best Practices**: Alerting, dashboards, on-call processes

**4:00 – 4:45 PM | DevOps Best Practices & Case Studies**
- **Deployment strategies**: Feature flags, A/B testing
- **Automated testing** and CI/CD integration
- **Incident management** and postmortems
- **Case Studies**: Startups and enterprise DevOps transformations

- **3 integration patterns**: Publish/Subscribe, Point-to-point, Streaming  
- **Benefits**: Loose coupling, scalability, resilience  
- **Sync vs async comparison**: Understanding the trade-offs  

#### Compute Evolution

- **Shared Responsibility Model**: EC2 → ECS → Fargate → Lambda  
- **Serverless benefits**: No server management, auto-scaling, pay-for-value  
- **Functions vs Containers**: Criteria for appropriate choice  

#### Amazon Q Developer

- **SDLC automation**: From planning to maintenance  
- **Code transformation**: Java upgrade, .NET modernization  
- **AWS Transform agents**: VMware, Mainframe, .NET migration  

### Key Takeaways

#### Design Mindset

- **Business-first approach**: Always start from the business domain, not the technology  
- **Ubiquitous language**: Importance of a shared vocabulary between business and tech teams  
- **Bounded contexts**: Identifying and managing complexity in large systems  

#### Technical Architecture

- **Event storming technique**: Practical method for modeling business processes  
- Use **event-driven communication** instead of synchronous calls  
- **Integration patterns**: When to use sync, async, pub/sub, streaming  
- **Compute spectrum**: Criteria for choosing between VM, containers, and serverless  

#### Modernization Strategy

- **Phased approach**: No rushing — follow a clear roadmap  
- **7Rs framework**: Multiple modernization paths depending on the application  
- **ROI measurement**: Cost reduction + business agility  

### Applying to Work

- **Apply DDD** to current projects: Event storming sessions with business teams  
- **Refactor microservices**: Use bounded contexts to define service boundaries  
- **Implement event-driven patterns**: Replace some sync calls with async messaging  
- **Adopt serverless**: Pilot AWS Lambda for suitable use cases  
- **Try Amazon Q Developer**: Integrate into the dev workflow to boost productivity  

### Event Experience

Attending the **“GenAI-powered App-DB Modernization”** workshop was extremely valuable, giving me a comprehensive view of modernizing applications and databases using advanced methods and tools. Key experiences included:

#### Learning from highly skilled speakers
- Experts from AWS and major tech organizations shared **best practices** in modern application design.  
- Through real-world case studies, I gained a deeper understanding of applying **DDD** and **Event-Driven Architecture** to large projects.  

#### Hands-on technical exposure
- Participating in **event storming** sessions helped me visualize how to **model business processes** into domain events.  
- Learned how to **split microservices** and define **bounded contexts** to manage large-system complexity.  
- Understood trade-offs between **synchronous and asynchronous communication** and integration patterns like **pub/sub, point-to-point, streaming**.  

#### Leveraging modern tools
- Explored **Amazon Q Developer**, an AI tool for SDLC support from planning to maintenance.  
- Learned to **automate code transformation** and pilot serverless with **AWS Lambda** to improve productivity.  

#### Networking and discussions
- The workshop offered opportunities to exchange ideas with experts, peers, and business teams, enhancing the **ubiquitous language** between business and tech.  
- Real-world examples reinforced the importance of the **business-first approach** rather than focusing solely on technology.  

#### Lessons learned
- Applying DDD and event-driven patterns reduces **coupling** while improving **scalability** and **resilience**.  
- Modernization requires a **phased approach** with **ROI measurement**; rushing the process can be risky.  
- AI tools like Amazon Q Developer can significantly **boost productivity** when integrated into the current workflow.  