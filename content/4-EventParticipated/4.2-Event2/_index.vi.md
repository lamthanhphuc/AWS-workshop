---
title: "Event 2"
date: "2006-01-02"
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch "AWS Cloud Mastery Series #2 - DevOps on AWS"

### Mục Đích Của Sự Kiện

- Chia sẻ văn hóa và nguyên tắc DevOps trên AWS
- Hướng dẫn xây dựng CI/CD pipeline hoàn chỉnh
- Giới thiệu Infrastructure as Code với CloudFormation và CDK
- Thực hành triển khai container services và monitoring
- Áp dụng DevOps best practices vào thực tế doanh nghiệp


### Nội Dung Nổi Bật

#### Phiên Sáng (8:30 AM – 12:00 PM)

**8:30 – 9:00 AM | Welcome & DevOps Mindset**
- Văn hóa và nguyên tắc DevOps
- Lợi ích và các chỉ số quan trọng (DORA, MTTR, deployment frequency)
- Tầm quan trọng của collaboration giữa Dev và Ops

**9:00 – 10:30 AM | AWS DevOps Services – CI/CD Pipeline**
- **Source Control**: AWS CodeCommit, Git strategies (GitFlow, Trunk-based)
- **Build & Test**: CodeBuild configuration, testing pipelines
- **Deployment**: CodeDeploy với Blue/Green, Canary, Rolling updates
- **Orchestration**: CodePipeline automation
- **Demo**: Full CI/CD pipeline walkthrough

**10:45 AM – 12:00 PM | Infrastructure as Code (IaC)**
- **AWS CloudFormation**: Templates, stacks, drift detection
- **AWS CDK**: Constructs, reusable patterns, language support
- **Demo**: Deploying với CloudFormation và CDK
- **Discussion**: Choosing between IaC tools

#### Phiên Chiều (1:00 – 5:00 PM)

**1:00 – 2:30 PM | Container Services on AWS**
- **Docker Fundamentals**: Microservices và containerization
- **Amazon ECR**: Image storage, scanning, lifecycle policies
- **Amazon ECS & EKS**: Deployment strategies, scaling, orchestration
- **AWS App Runner**: Simplified container deployment
- **Demo & Case Study**: Microservices deployment comparison

**2:45 – 4:00 PM | Monitoring & Observability**
- **CloudWatch**: Metrics, logs, alarms, dashboards
- **AWS X-Ray**: Distributed tracing và performance insights
- **Demo**: Full-stack observability setup
- **Best Practices**: Alerting, dashboards, on-call processes

**4:00 – 4:45 PM | DevOps Best Practices & Case Studies**
- **Deployment strategies**: Feature flags, A/B testing
- **Automated testing** và CI/CD integration
- **Incident management** và postmortems
- **Case Studies**: Startups và enterprise DevOps transformations

### Những Gì Học Được

#### DevOps Culture & Practices

- **Collaboration mindset**: Phá vỡ rào cản giữa Dev và Ops teams
- **Automation first**: Tự động hóa mọi khâu có thể từ build, test đến deployment
- **Metrics-driven**: DORA metrics, MTTR, deployment frequency để đo lường hiệu quả
- **Continuous improvement**: Feedback loops và iterative optimization

#### CI/CD Pipeline Architecture

- **Source control strategies**: GitFlow vs Trunk-based development
- **Build automation**: CodeBuild configuration, testing frameworks integration
- **Deployment patterns**: Blue/Green, Canary, Rolling updates - khi nào dùng pattern nào
- **Pipeline orchestration**: CodePipeline automation với multi-stage approvals
- **Security integration**: Security scanning trong CI/CD pipeline

#### Infrastructure as Code

- **CloudFormation fundamentals**: Templates, stacks, change sets, drift detection
- **AWS CDK advantages**: Type-safe, reusable constructs, multiple language support
- **IaC best practices**: Version control, testing, modularization
- **State management**: Comparing CloudFormation vs Terraform approaches

#### Container Orchestration

- **Containerization benefits**: Portability, consistency, resource efficiency
- **ECR features**: Image scanning, lifecycle policies, replication
- **ECS vs EKS**: Trade-offs và use cases cho mỗi service
- **App Runner simplicity**: Khi nào chọn App Runner thay vì ECS/EKS

#### Observability & Monitoring

- **CloudWatch deep dive**: Custom metrics, log insights, composite alarms
- **Distributed tracing**: X-Ray for microservices debugging
- **Dashboards design**: Creating actionable dashboards
- **Alert management**: Reducing alert fatigue, on-call best practices

#### Amazon Q Developer

- **SDLC automation**: Từ planning đến maintenance
- **Code transformation**: Java upgrade, .NET modernization
- **AWS Transform agents**: VMware, Mainframe, .NET migration

### Những Gì Học Được

#### Tư Duy Thiết Kế

- **Business-first approach**: Luôn bắt đầu từ business domain, không phải technology
- **Ubiquitous language**: Importance của common vocabulary giữa business và tech teams
- **Bounded contexts**: Cách identify và manage complexity trong large systems

#### Kiến Trúc Kỹ Thuật

- **Event storming technique**: Phương pháp thực tế để mô hình hóa quy trình kinh doanh
- Sử dụng **Event-driven communication** thay vì synchronous calls
- **Integration patterns**: Hiểu khi nào dùng sync, async, pub/sub, streaming
- **Compute spectrum**: Criteria chọn từ VM → containers → serverless

#### Chiến Lược Hiện Đại Hóa

- **Phased approach**: Không rush, phải có roadmap rõ ràng
- **7Rs framework**: Nhiều con đường khác nhau tùy thuộc vào đặc điểm của mỗi ứng dụng
- **ROI measurement**: Cost reduction + business agility

### Ứng Dụng Vào Công Việc

- **Áp dụng DDD** cho project hiện tại: Event storming sessions với business team
- **Refactor microservices**: Sử dụng bounded contexts để identify service boundaries
- **Implement event-driven patterns**: Thay thế một số sync calls bằng async messaging
- **Serverless adoption**: Pilot AWS Lambda cho một số use cases phù hợp
- **Try Amazon Q Developer**: Integrate vào development workflow để boost productivity

### Trải nghiệm trong event

Tham gia workshop **“GenAI-powered App-DB Modernization”** là một trải nghiệm rất bổ ích, giúp tôi có cái nhìn toàn diện về cách hiện đại hóa ứng dụng và cơ sở dữ liệu bằng các phương pháp và công cụ hiện đại. Một số trải nghiệm nổi bật:

#### Học hỏi từ các diễn giả có chuyên môn cao
- Các diễn giả đến từ AWS và các tổ chức công nghệ lớn đã chia sẻ **best practices** trong thiết kế ứng dụng hiện đại.
- Qua các case study thực tế, tôi hiểu rõ hơn cách áp dụng **Domain-Driven Design (DDD)** và **Event-Driven Architecture** vào các project lớn.

#### Trải nghiệm kỹ thuật thực tế
- Tham gia các phiên trình bày về **event storming** giúp tôi hình dung cách **mô hình hóa quy trình kinh doanh** thành các domain events.
- Học cách **phân tách microservices** và xác định **bounded contexts** để quản lý sự phức tạp của hệ thống lớn.
- Hiểu rõ trade-offs giữa **synchronous và asynchronous communication** cũng như các pattern tích hợp như **pub/sub, point-to-point, streaming**.

#### Ứng dụng công cụ hiện đại
- Trực tiếp tìm hiểu về **Amazon Q Developer**, công cụ AI hỗ trợ SDLC từ lập kế hoạch đến maintenance.
- Học cách **tự động hóa code transformation** và pilot serverless với **AWS Lambda**, từ đó nâng cao năng suất phát triển.

#### Kết nối và trao đổi
- Workshop tạo cơ hội trao đổi trực tiếp với các chuyên gia, đồng nghiệp và team business, giúp **nâng cao ngôn ngữ chung (ubiquitous language)** giữa business và tech.
- Qua các ví dụ thực tế, tôi nhận ra tầm quan trọng của **business-first approach**, luôn bắt đầu từ nhu cầu kinh doanh thay vì chỉ tập trung vào công nghệ.

#### Bài học rút ra
- Việc áp dụng DDD và event-driven patterns giúp giảm **coupling**, tăng **scalability** và **resilience** cho hệ thống.
- Chiến lược hiện đại hóa cần **phased approach** và đo lường **ROI**, không nên vội vàng chuyển đổi toàn bộ hệ thống.
- Các công cụ AI như Amazon Q Developer có thể **boost productivity** nếu được tích hợp vào workflow phát triển hiện tại.