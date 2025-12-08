---
title: "Bản đề xuất"
date: 2025-11-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Student Management System

### 1. Tóm tắt điều hành

Serverless Student Management Platform là nền tảng quản lý sinh viên dựa trên đám mây, được thiết kế dành cho các tổ chức giáo dục và doanh nghiệp nhỏ nhằm nâng cao khả năng quản lý, phân tích và tương tác với dữ liệu sinh viên. Nền tảng hỗ trợ tối đa 50-100 sinh viên ban đầu, với khả năng mở rộng linh hoạt lên đến 500–1000 sinh viên mà không cần thay đổi hạ tầng lớn, nhờ sử dụng các dịch vụ AWS Serverless như Lambda, DynamoDB, API Gateway.

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

Các hệ thống quản lý sinh viên hiện tại yêu cầu nhập dữ liệu thủ công, khó quản lý khi có nhiều lớp học. Không có hệ thống tập trung cho dữ liệu hoặc phân tích thời gian thực, và các nền tảng bên thứ ba thường tốn kém và quá phức tạp.

**Giải pháp**

Nền tảng sử dụng Amazon API Gateway để tiếp nhận yêu cầu REST, AWS Lambda xử lý logic nghiệp vụ, Amazon DynamoDB để lưu trữ dữ liệu sinh viên và điểm số. AWS Amplify với React/Next.js cung cấp giao diện web, và Amazon Cognito đảm bảo quyền truy cập an toàn. Tương tự như các hệ thống LMS truyền thống nhưng với chi phí thấp hơn, người dùng có thể đăng ký sinh viên mới và quản lý thông tin, nhưng nền tảng này hoạt động ở quy mô nhỏ hơn và phục vụ mục đích học tập.

**Lợi ích và hoàn vốn đầu tư (ROI)**

Giải pháp tạo nền tảng cơ bản để các sinh viên IT phát triển kỹ năng AWS serverless, đồng thời cung cấp công cụ quản lý hiệu quả cho giáo viên phục vụ giảng dạy và đánh giá. Nền tảng giảm bớt báo cáo thủ công cho từng lớp học thông qua hệ thống tập trung, đơn giản hóa quản lý và bảo trì, đồng thời cải thiện độ tin cậy dữ liệu. Chi phí hàng tháng ước tính $7-20 USD theo bảng ước tính ngân sách chi tiết, tổng cộng $40-60 USD cho 3 tháng.

  
### 3. Kiến trúc giải pháp

Hệ thống được thiết kế theo kiến trúc AWS Well-Architected Framework với các tầng liên kết, đảm bảo khả năng quản lý dữ liệu, xác thực, giám sát. Kiến trúc serverless giúp tối ưu chi phí và đảm bảo khả năng mở rộng tự động.

<!-- ![Cloud Security & Monitoring System Architecture](/images/2-Proposal/edge_architecture.jpeg) -->

![Platform Architecture Diagram](/images/2-Proposal/Solution.drawio.png)

#### Dịch vụ AWS Core được sử dụng

| **Dịch vụ**            | **Chức năng chính**                                                  | **Lợi ích nổi bật**                                                                                                             |
| ---------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon Route 53**    | Quản lý DNS và routing traffic đến CloudFront                        | Thiết lập domain tùy chỉnh, health checks, geo-routing, tích hợp SSL để đảm bảo truy cập an toàn và nhanh chóng.                |
| **Amazon CloudFront**  | Phân phối CDN cho nội dung tĩnh và tài nguyên frontend               | Giảm độ trễ toàn cầu, caching tại edge locations, hỗ trợ HTTPS, tích hợp WAF để bảo vệ và OAC cho S3.                           |
| **AWS WAF**            | Firewall bảo vệ ứng dụng web khỏi các tấn công                       | Chặn request độc hại, rate limiting, lọc IP, tích hợp liền mạch với CloudFront để bảo mật traffic.                              |
| **AWS Amplify**        | Hosting và triển khai frontend với CI/CD                             | Xây dựng và deploy ứng dụng web nhanh chóng, tích hợp với Cognito/AppSync cho giao diện thân thiện và chat real-time.           |
| **Amazon API Gateway** | Xử lý và routing yêu cầu API từ frontend đến backend                 |  Hỗ trợ REST/HTTP APIs, throttling, caching, và authorizer để tăng hiệu suất và bảo mật.                                        |
| **Amazon Cognito**     | Quản lý xác thực và ủy quyền người dùng                              | Hỗ trợ MFA, JWT tokens, groups cho phân quyền (giáo viên/admin vs. học sinh), tích hợp dễ với AppSync/API Gateway.              |
| **AWS Lambda**         | Thực thi logic backend và xử lý sự kiện                              | Serverless, tự động scale, pay-per-use, xử lý CRUD, events từ EventBridge và ML inference từ Personalize.                       |
| **Amazon DynamoDB**    | Lưu trữ dữ liệu NoSQL cho thông tin sinh viên, chat và bài tập       | Query nhanh, tự động scale, hỗ trợ Global Secondary Indexes (GSI) cho tìm kiếm phức tạp và chi phí thấp.                        |
| **Amazon CloudWatch**  | Giám sát logs, metrics và alarms hệ thống                            | Theo dõi real-time, thiết lập alarms để phát hiện vấn đề sớm, tích hợp với Lambda/DynamoDB để tối ưu hóa.                       |
| **Amazon S3**          | Lưu trữ artifact và build từ CI/CD                                   | Hosting static files rẻ tiền, bền vững cao, tích hợp với CodePipeline để lưu artifact deploy.                                   |
| **GitLab**             | Quản lý source code và trigger CI/CD pipeline                        | Version control (GitLab.com hoặc self-hosted), merge requests, issue tracking, webhook tích hợp với CodePipeline để automate deploy.                          |
| **AWS CodePipeline**   | Orchestrate CI/CD pipeline từ source đến deploy                      | Tự động hóa toàn bộ quy trình từ GitLab đến production, tích hợp với CodeBuild/CodeDeploy, hỗ trợ approvals và notifications.                     |
| **AWS CodeBuild**      | Build và test code từ GitLab repository                              | Tự động compile, package artifacts, chạy unit tests trong pipeline, hỗ trợ multi-environment builds.                                   |
| **AWS CodeDeploy**     | Triển khai ứng dụng lên các dịch vụ AWS                              | Hỗ trợ blue/green deployment, rollback tự động, zero-downtime deployments cho Lambda/Amplify.                                               |

#### Thiết kế thành phần

| **Layer**            | **Thành phần chính**                            | **Chức năng**                          |
| -------------------- | ----------------------------------------------- | -------------------------------------- |
| **Edge Layer**       | Route53, CloudFront, WAF                        | DNS, CDN, bảo mật lớp ngoài            |
| **Frontend Layer**   | Amplify, Cognito                       | Giao diện web, realtime chat, xác thực |
| **Backend Layer**    | API Gateway, Lambda, DynamoDB                   | Xử lý logic, CRUD, lưu trữ dữ liệu     |
| **Monitoring Layer** | CloudWatch                                      | Logs, metrics, alerts                  |
| **CI/CD Layer**      | GitLab, CodePipeline, CodeBuild, CodeDeploy, S3 | Tự động hóa build & deploy             |

#### Chú thích luồng chính

| **Luồng** | **Mô tả**                                                                             |
| --------- | ------------------------------------------------------------------------------------- |
| **(1)**   | User truy cập qua Route53 → CloudFront → Amplify (frontend).                          |
| **(2)**   | Amplify giao tiếp với API Gateway để gửi/nhận dữ liệu.                   |
| **(3.1)** | API Gateway → Lambda để xử lý logic (CRUD, v.v.).                              |
| **(3.2)** | Cognito xác thực và trả token.                                                        |
| **(4)**   | Dev push code lên GitLab → CodePipeline pull → CodeBuild/CodeDeploy → Amplify/Lambda. |
| **(5)**   | CloudWatch thu thập logs và metrics.                                                  |

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai
Dự án được triển khai trong **5 tuần** với 5 giai đoạn chính tương ứng:

1. **Tuần 1 - Thiết lập nền tảng (Foundation Setup)**
   - **Ngày 1-2**: Thiết lập tài khoản AWS, cấu hình IAM roles/policies, thiết lập billing alerts
   - **Ngày 3-4**: Tạo DynamoDB tables (Students, Courses, Grades, ChatMessages, Assignments, Attendance, Events) với GSI, cấu hình Cognito User Pools với groups (Admin/Teacher/Student)
   - **Ngày 5-7**: Khởi tạo IaC templates (AWS CDK/CloudFormation), nghiên cứu serverless patterns, thiết kế kiến trúc chi tiết với sequence diagrams, setup Git repo structure cho parallel development


2. **Tuần 2 - Backend & Frontend Song Song (Parallel Development Phase 1)**
   - **Backend Team (Ngày 1-7)**:
     - Xây dựng 50+ Lambda functions cho: Students (CRUD, search, bulk import/export), Courses (CRUD, enrollment), Grades (CRUD, analytics, statistics), Assignments (CRUD, submissions), Attendance (CRUD, reports), Auth (login, register, refresh token)
     - Thiết lập API Gateway với 50+ REST endpoints: `/students/*` (10 endpoints), `/courses/*` (8 endpoints), `/grades/*` (12 endpoints), `/assignments/*` (8 endpoints), `/attendance/*` (6 endpoints)
     - Unit testing với 80%+ coverage
   - **Frontend Team (Ngày 1-7)**:
     - Deploy Amplify hosting với React/Next.js, setup routing (React Router)
     - Xây dựng UI components: Layout (Header, Sidebar, Footer), Authentication (Login, Register), Dashboard (Overview, Stats cards)
     - Setup API client (Axios/Fetch), state management (Redux/Zustand), form validation (React Hook Form)


3. **Tuần 3 - Backend & Frontend Song Song (Parallel Development Phase 2)**
   - **Backend Team (Ngày 1-7)**:
     - Tích hợp Lambda handlers cho automated workflows
     - Integration testing với Postman/ SWAGGER
   - **Frontend Team (Ngày 1-7)**:
     - Xây dựng 15+ pages: Student Management (List, Create, Edit, Detail, Import), Course Management (List, Create, Edit, Enrollment), Grade Management (List, Input, Analytics), Assignment (List, Submit, Review), Attendance (Tracker, Reports)
   - **Integration (Ngày 6-7)**:
     - Kết nối CloudFront CDN với Route53, cấu hình WAF rules (rate limiting, geo-blocking), SSL/TLS certificates
     - End-to-end testing giữa Frontend và Backend


4. **Tuần 4 - CI/CD với CodeBuild, CodeDeploy, CodePipeline**
  - **Ngày 1-2**: Tìm hiểu và cấu hình AWS CodeBuild cho frontend và backend (buildspec, môi trường build, lưu artifacts vào S3)
  - **Ngày 3-4**: Thiết lập AWS CodePipeline để tự động hóa quy trình build và deploy từ GitLab lên môi trường AWS (kết nối các stage: source, build, deploy)
  - **Ngày 5-6**: Cấu hình AWS CodeDeploy để triển khai backend (Lambda hoặc EC2) và frontend (Amplify hoặc S3), kiểm thử quy trình deploy tự động
  - **Ngày 7**: Kiểm thử toàn bộ pipeline, đảm bảo build, deploy tự động hoạt động ổn định, ghi nhận kết quả và tối ưu hóa pipeline


5. **Tuần 5 - Kiểm thử & Demo (Testing & Launch)**
   - **Ngày 1-2**: End-to-end testing (user flows, edge cases), load testing với 100+ concurrent users
   - **Ngày 3-4**: Performance optimization (Lambda memory tuning, DynamoDB capacity adjustment, CloudFront caching)
   - **Ngày 5-6**: Hoàn thiện documentation (architecture diagrams, API docs, deployment guide, user manual)
   - **Ngày 7**: Demo preparation, presentation slides, video recording


#### Yêu cầu kỹ thuật

**Hệ thống quản lý sinh viên**: Dashboard web đầy đủ với 5 modules chính (Students, Courses, Grades, Assignments, Attendance). Frontend React/Next.js chạy trên Amplify Hosting với 15+ pages và 50+ components. Cognito xác thực và phân quyền cho tất cả người dùng, bao gồm 5-10 admin/giáo viên (với quyền cao như CRUD dữ liệu) và học sinh (với quyền giới hạn như xem điểm số,lớp học).

**Kiến trúc API toàn diện**: 50+ REST API endpoints qua API Gateway (CRUD operations, search, bulk actions). Backend được xây dựng song song với Frontend để tối ưu thời gian phát triển.

**Nền tảng phân tích thông minh**: Kiến thức thực tế về AWS Amplify (hosting React), Lambda (50+ functions xử lý nghiệp vụ), DynamoDB (5 tables với GSI). Sử dụng AWS CDK/SDK để lập trình IaC và automation. Parallel development methodology với Git branching strategy cho Backend/Frontend teams.


### 5. Lộ trình & Mốc quan trọng

Để phù hợp với thời gian thực tập, dự án được triển khai trong **5 tuần** với 5 giai đoạn chính:

| **Giai đoạn**               | **Thời gian** | **Mục tiêu chính**          | **Sản phẩm đầu ra (Deliverables)**                                          | **Tiêu chí thành công (Success Criteria)** | 
| --------------------------- | ------------- | --------------------------- | --------------------------------------------------------------------------- | ------------------------------------------ | 
| **1: Thiết lập nền tảng**   | Tuần 1        | Thiết lập môi trường AWS    | • Tài khoản AWS với IAM setup<br>• 7 DynamoDB tables với GSI<br>• Cognito User Pool<br>• IaC templates (CDK/CloudFormation)<br>• Git repo structure | • Infrastructure as Code hoàn chỉnh<br>• Security baseline đạt chuẩn<br>• Parallel dev environment ready | 
| **2: Backend & Frontend Parallel (Phase 1)** | Tuần 2 | Xây dựng core Backend + Frontend foundation | • 50+ Lambda functions<br>• API Gateway với 50+ REST endpoints<br>• React/Next.js app foundation<br>• 10+ UI components<br>• Unit tests (>80% coverage) | • 50+ API endpoints hoạt động<br>• API response time <500ms<br>• Frontend routing setup<br>• All backend tests passed | 
| **3: Backend & Frontend Parallel (Phase 2)** | Tuần 3 | Tích hợp Lambda workflows & kiểm thử tích hợp | • Tích hợp Lambda handlers cho automated workflows<br>• Integration testing với Postman/SWAGGER<br>• 15+ complete pages<br>• CloudFront + Route53 + WAF<br>• Responsive design | • Automated workflows hoạt động ổn định<br>• Đã kiểm thử tích hợp với Postman/SWAGGER<br>• All pages integrated with backend<br>• SSL/HTTPS enabled<br>• Mobile responsive |
  | **4: CI/CD với CodeBuild, CodeDeploy, CodePipeline** | Tuần 4 | Thiết lập và kiểm thử quy trình CI/CD tự động | • Cấu hình CodeBuild cho frontend/backend<br>• Thiết lập CodePipeline kết nối GitLab, CodeBuild, CodeDeploy<br>• Kiểm thử deploy tự động cho backend/frontend | • Build và deploy tự động hoạt động ổn định<br>• Artifacts lưu trữ đúng chuẩn<br>• Pipeline tối ưu hóa |
| **5: Kiểm thử & Demo**      | Tuần 5        | Kiểm thử và hoàn thiện      | • Load test reports (50+ users)<br>• Performance optimization<br>• Complete documentation<br>• Demo video + slides | • System uptime ≥99%<br>• All features stable<br>• Documentation complete<br>• Demo ready |


### 6. Ước tính ngân sách

Có thể tải [tệp ước tính ngân sách](/images/2-Proposal/MyEstimate.pdf) để xem chi tiết chi phí.

#### Chi phí hạ tầng

**Dịch vụ AWS:**


| **Dịch vụ**            | **Mô tả sử dụng**                                           | **Chi phí ước tính / tháng (USD)** | **Ghi chú**                                                                           |
| ---------------------- | ----------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------- |
| **Amazon API Gateway** | Xử lý ~100k-500k API calls/tháng                            | $3.8                              | HTTP APIs, REST APIs, miễn phí 1M cuộc gọi đầu tiên.                                   |
| **AWS Lambda**         | ~200k-500k yêu cầu, 100k-200k GB-giây                       | $0.5                              | Yêu cầu, GB-giây, miễn phí 1M yêu cầu + 400k GB-giây.                                 |
| **Amazon DynamoDB**    | ~5-10 GB lưu trữ, 250k-500k đọc/ghi                         | $1                                | Đọc, ghi, lưu trữ, miễn phí 25 GB + 25 RCU/WCU.                                       |
| **Amazon Cognito**     | ~100-200 người dùng hoạt động hàng tháng                    | $0                                | Miễn phí 10k người dùng đầu tiên.                                                     |
| **Amazon S3**          | ~5 GB lưu trữ, yêu cầu thấp                                 | $0.13                             | Lưu trữ, yêu cầu, có tín dụng miễn phí.                                               |
| **Amazon CloudWatch**  | ~5 GB nhật ký, 10 số liệu/cảnh báo                          | $4                                | Nhật ký, số liệu, cảnh báo, miễn phí 5 GB.                                            |
| **AWS Amplify**        | Lưu trữ dashboard, vài lần build/tháng                      | $1                                | Phút build, lưu trữ, có miễn phí.                                                     |
| **Amazon Route 53**    | Truy vấn DNS, vùng lưu trữ                                  | $1                                | Vùng lưu trữ, truy vấn DNS.                                                           |
| **AWS WAF**            | Đánh giá Web ACL, quy tắc                                   | $7                                | Web ACL, quy tắc, phí quản lý quy tắc.                                                |
| **AWS CodeBuild**      | ~10 lần build/tháng, 100 phút                               | $0.9                              | $0.005/phút build, miễn phí 100 phút/tháng.                                           |
| **AWS CodeDeploy**     | Triển khai ứng dụng                                         | $0.8                              | EC2/tại chỗ, serverless miễn phí.                                                     |
| **Amazon CloudFront**  | CDN phân phối nội dung tĩnh (kết nối Route53 & Amplify/S3)  | $1                                | Miễn phí 1 TB/tháng đầu tiên, $0.085/GB sau đó.                                       |
| **AWS CodePipeline**   | Tự động hóa CI/CD (kết nối GitLab → CodeBuild → CodeDeploy) | $1                                | $1/tháng cho mỗi pipeline đang hoạt động.                                             |

#### Tổng cộng ước tính
| **Tổng chi phí / tháng (ước lượng)** | **Tổng 3 tháng (ước lượng)** | **Với AWS Free Tier** | **Ghi chú**                                                                                                             |
| ------------------------------------ | ---------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **~$7 – $20 / tháng**                | **~$40 – $60 / 3 tháng**     | **~$15 – $30 / 3 tháng** | Phụ thuộc mức sử dụng thực tế. Tận dụng Free Tier (năm đầu) + AWS Educate credits có thể giảm 50% chi phí. |

#### Lưu ý
- Tất cả dịch vụ chính nằm trong gói miễn phí năm đầu.
- Có thể giảm thêm nếu sử dụng AWS Educate / Tín dụng cho sinh viên.


### 7. Đánh giá rủi ro

Dựa trên NIST Risk Management Framework, nhóm dự án xác định các rủi ro chính và biện pháp giảm thiểu.

| **Mã rủi ro**                     | **Mô tả**                                | **Mức độ**     | **Giảm thiểu (Mitigation)**                                    |
| --------------------------------- | ---------------------------------------- | -------------- | -------------------------------------------------------------- |
| **R1 – Data Leakage**             | Lộ dữ liệu do config sai                 | **Cao**        | Áp dụng Cognito auth, IAM least privilege, DynamoDB encryption |
| **R2 – API Overload**             | Quá nhiều requests gây chậm              | **Trung bình** | Throttling API Gateway/AppSync, alarms CloudWatch              |
| **R3 – Lambda Cold Start**        | Delay khi invoke                         | **Trung bình** | Tối ưu code, Provisioned Concurrency nếu cần                   |
| **R4 – Chi phí vượt**             | Usage tăng bất thường (chat, emails cao) | **Trung bình** | AWS Budgets alerts, monitor Cost Explorer                      |
| **R5 – Service Downtime**         | Gián đoạn AWS                            | **Thấp**       | Multi-AZ config, backups DynamoDB                              |


**Kế hoạch dự phòng (Tóm tắt):**
- Khôi phục: Sử dụng CloudFormation để tái tạo hạ tầng nhanh chóng khi có sự cố hoặc lỗi triển khai.
- Giám sát & cảnh báo: Thiết lập CloudWatch alarms để phát hiện sớm các vấn đề về hiệu năng, bảo mật và chi phí, gửi thông báo qua email cho nhóm vận hành.
- Cải tiến liên tục: Đánh giá định kỳ quy trình CI/CD, kiểm thử tự động, tối ưu hóa pipeline và cập nhật tài liệu hướng dẫn để đảm bảo hệ thống luôn ổn định và dễ bảo trì.



### 8. Kết quả kỳ vọng

**Kết quả kỹ thuật:**
- Hoàn thiện hệ thống quản lý sinh viên serverless với quy trình CI/CD tự động hóa, đảm bảo build, test, deploy nhanh chóng và ổn định.
- API đáp ứng đầy đủ các chức năng quản lý sinh viên, tích hợp Lambda workflows, kiểm thử tích hợp với Postman/SWAGGER.
- Frontend hiện đại với React/Next.js, 15+ trang, 50+ UI components, kết nối realtime với backend.
- Tích hợp các dịch vụ AWS trọng yếu: API Gateway, Lambda, DynamoDB, Cognito, S3, Amplify, CloudWatch, Route53, CloudFront, WAF, CodePipeline, CodeBuild, CodeDeploy.
- Hiệu năng: API response <500ms, uptime ≥99%, quy trình CI/CD tối ưu hóa chi phí và thời gian triển khai.
- Chi phí thực tế: $7-20/tháng, tổng ~$21-60 cho 3 tháng (có thể giảm xuống $15-30 với AWS Free Tier và Educate credits).

**Giá trị dài hạn:**
- Dễ dàng mở rộng sang các ứng dụng mobile hoặc tích hợp AI/analytics nâng cao.
- Là nền tảng thực hành và đào tạo về serverless, CI/CD, và DevOps cho sinh viên và doanh nghiệp nhỏ.