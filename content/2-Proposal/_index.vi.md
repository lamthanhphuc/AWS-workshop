---
title: "Bản đề xuất"
date: 2025-11-02
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Student Management System

### 1. Tóm tắt điều hành

Serverless Student Management Platform là nền tảng quản lý sinh viên dựa trên đám mây, được thiết kế dành cho các tổ chức giáo dục và doanh nghiệp nhỏ nhằm nâng cao khả năng quản lý, phân tích và tương tác với dữ liệu sinh viên. Nền tảng hỗ trợ tối đa 50-100 sinh viên ban đầu, với khả năng mở rộng linh hoạt lên đến 500–1000 sinh viên mà không cần thay đổi hạ tầng lớn, nhờ sử dụng các dịch vụ AWS Serverless như Lambda, DynamoDB, AppSync và EventBridge.

Người dùng có thể truy cập qua giao diện web thân thiện (hỗ trợ chat real-time cho bài tập và bình luận) kết hợp với API để truyền dữ liệu thời gian thực, đảm bảo tính tương tác cao. Nền tảng tận dụng AWS để cung cấp quản lý tập trung (lưu trữ dữ liệu an toàn), phân tích dự đoán (qua Personalize cho ranking điểm số và recommendation), gửi thông báo nhắc nhở (qua SES), và giám sát hệ thống (qua CloudWatch), đồng thời tối ưu hóa chi phí bằng cách chỉ tính phí theo sử dụng thực tế.


### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

Các hệ thống quản lý sinh viên hiện tại yêu cầu nhập dữ liệu thủ công, khó quản lý khi có nhiều lớp học. Không có hệ thống tập trung cho dữ liệu hoặc phân tích thời gian thực, và các nền tảng bên thứ ba thường tốn kém và quá phức tạp. Thiếu tính năng trao đổi realtime dẫn đến giao tiếp chậm trễ, thiếu cơ chế khuyến khích như xếp hạng chăm chỉ, và xử lý sự kiện kiểm tra thủ công.

**Giải pháp**

Nền tảng sử dụng Amazon API Gateway để tiếp nhận yêu cầu REST, AWS Lambda xử lý logic nghiệp vụ, Amazon DynamoDB để lưu trữ dữ liệu sinh viên và điểm số, và AWS AppSync cùng GraphQL để cung cấp giao tiếp realtime. AWS Amplify với React/Next.js cung cấp giao diện web, và Amazon Cognito đảm bảo quyền truy cập an toàn. Tương tự như các hệ thống LMS truyền thống nhưng với chi phí thấp hơn, người dùng có thể đăng ký sinh viên mới và quản lý thông tin, nhưng nền tảng này hoạt động ở quy mô nhỏ hơn và phục vụ mục đích học tập. Các tính năng chính bao gồm dashboard quản lý thời gian thực, phân tích xu hướng học tập và chi phí vận hành thấp.

**Lợi ích và hoàn vốn đầu tư (ROI)**

Giải pháp tạo nền tảng cơ bản để các sinh viên IT phát triển kỹ năng AWS serverless, đồng thời cung cấp công cụ quản lý hiệu quả cho giáo viên phục vụ giảng dạy và đánh giá. Nền tảng giảm bớt báo cáo thủ công cho từng lớp học thông qua hệ thống tập trung, đơn giản hóa quản lý và bảo trì, đồng thời cải thiện độ tin cậy dữ liệu. Chi phí hàng tháng ước tính $7-20 USD (theo AWS Pricing Calculator), tổng cộng $21-60 USD cho 3 tháng. Tất cả tài nguyên AWS được sử dụng trong free tier và giá rẻ, không phát sinh chi phí phát triển thêm. Thời gian hoàn vốn 3-6 tháng nhờ tiết kiệm đáng kể thời gian thao tác thủ công và nâng cao hiệu quả giảng dạy.

  
### 3. Kiến trúc giải pháp

Hệ thống được thiết kế theo kiến trúc AWS Well-Architected Framework với các tầng liên kết, đảm bảo khả năng quản lý dữ liệu, xác thực, giám sát, giao tiếp realtime, xếp hạng ML, thông báo và xử lý sự kiện liên tục. Kiến trúc serverless giúp tối ưu chi phí và đảm bảo khả năng mở rộng tự động.

<!-- ![Cloud Security & Monitoring System Architecture](/images/2-Proposal/edge_architecture.jpeg) -->

![Platform Architecture Diagram](/images/2-Proposal/solution.drawio.png)

#### Dịch vụ AWS Core được sử dụng

| **Dịch vụ**            | **Chức năng chính**                                                  | **Lợi ích nổi bật**                                                                                                             |
| ---------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon Route 53**    | Quản lý DNS và routing traffic đến CloudFront                        | Thiết lập domain tùy chỉnh, health checks, geo-routing, tích hợp SSL để đảm bảo truy cập an toàn và nhanh chóng.                |
| **Amazon CloudFront**  | Phân phối CDN cho nội dung tĩnh và tài nguyên frontend               | Giảm độ trễ toàn cầu, caching tại edge locations, hỗ trợ HTTPS, tích hợp WAF để bảo vệ và OAC cho S3.                           |
| **AWS WAF**            | Firewall bảo vệ ứng dụng web khỏi các tấn công                       | Chặn request độc hại, rate limiting, lọc IP, tích hợp liền mạch với CloudFront để bảo mật traffic.                              |
| **AWS Amplify**        | Hosting và triển khai frontend với CI/CD                             | Xây dựng và deploy ứng dụng web nhanh chóng, tích hợp với Cognito/AppSync cho giao diện thân thiện và chat real-time.           |
| **AWS AppSync**        | Hỗ trợ GraphQL API với subscriptions cho chat real-time              | Cập nhật dữ liệu thời gian thực qua WebSockets, tích hợp với DynamoDB/Lambda để xử lý bình luận bài tập và tương tác sinh viên. |
| **Amazon API Gateway** | Xử lý và routing yêu cầu API từ frontend đến backend                 |  Hỗ trợ REST/HTTP APIs, throttling, caching, và authorizer để tăng hiệu suất và bảo mật.                                        |
| **Amazon Cognito**     | Quản lý xác thực và ủy quyền người dùng                              | Hỗ trợ MFA, JWT tokens, groups cho phân quyền (giáo viên/admin vs. học sinh), tích hợp dễ với AppSync/API Gateway.              |
| **AWS Lambda**         | Thực thi logic backend và xử lý sự kiện                              | Serverless, tự động scale, pay-per-use, xử lý CRUD, events từ EventBridge và ML inference từ Personalize.                       |
| **Amazon DynamoDB**    | Lưu trữ dữ liệu NoSQL cho thông tin sinh viên, chat và bài tập       | Query nhanh, tự động scale, hỗ trợ Global Secondary Indexes (GSI) cho tìm kiếm phức tạp và chi phí thấp.                        |
| **Amazon CloudWatch**  | Giám sát logs, metrics và alarms hệ thống                            | Theo dõi real-time, thiết lập alarms để phát hiện vấn đề sớm, tích hợp với Lambda/DynamoDB để tối ưu hóa.                       |
| **Amazon S3**          | Lưu trữ artifact và build từ CI/CD                                   | Hosting static files rẻ tiền, bền vững cao, tích hợp với CodePipeline để lưu artifact deploy.                                   |
| **Amazon EventBridge** | Routing sự kiện để xử lý kiểm tra, thông báo và ranking              | Event-driven, tích hợp với Lambda/SES để tự động hóa quy trình như cập nhật điểm và nhắc nhở.                                   |
| **AWS Personalize**    | Xây dựng mô hình ML để xếp hạng sinh viên dựa trên dữ liệu hoạt động | Cá nhân hóa thứ hạng dựa trên dữ liệu hoạt động, hỗ trợ real-time inference, tự động học từ dữ liệu DynamoDB.                   |
| **AWS SES**            | Gửi email thông báo qua web xếp hạng và cập nhật hệ thống            | Gửi hàng loạt email đáng tin cậy, tích hợp với EventBridge/Lambda, chi phí thấp cho quy mô nhỏ.                                 |
| **AWS CodeBuild**      | Build và test code từ GitLab repository                              | Tự động compile, package artifacts, chạy unit tests trong pipeline, hỗ trợ multi-environment.                                   |
| **AWS CodeDeploy**     | Triển khai ứng dụng lên các dịch vụ AWS                              | Hỗ trợ blue/green deployment, rollback tự động, zero-downtime cho Lambda/Amplify.                                               |
| **AWS CodePipeline**   | Orchestrate CI/CD pipeline từ source đến deploy                      | Tự động hóa toàn bộ quy trình, tích hợp với GitLab/CodeBuild/CodeDeploy, hỗ trợ approvals và notifications.                     |
| **GitLab**             | Quản lý source code và trigger CI/CD                                 | Version control, merge requests, issue tracking, webhook tích hợp với CodePipeline để automate deploy.                          |

#### Thiết kế thành phần

| **Layer**            | **Thành phần chính**                            | **Chức năng**                          |
| -------------------- | ----------------------------------------------- | -------------------------------------- |
| **Edge Layer**       | Route53, CloudFront, WAF                        | DNS, CDN, bảo mật lớp ngoài            |
| **Frontend Layer**   | Amplify, AppSync, Cognito                       | Giao diện web, realtime chat, xác thực |
| **Backend Layer**    | API Gateway, Lambda, DynamoDB                   | Xử lý logic, CRUD, lưu trữ dữ liệu     |
| **Event & ML Layer** | EventBridge, SES, Personalize                   | Gửi thông báo, xử lý ML ranking        |
| **Monitoring Layer** | CloudWatch                                      | Logs, metrics, alerts                  |
| **CI/CD Layer**      | GitLab, CodePipeline, CodeBuild, CodeDeploy, S3 | Tự động hóa build & deploy             |

#### Chú thích luồng chính

| **Luồng** | **Mô tả**                                                                             |
| --------- | ------------------------------------------------------------------------------------- |
| **(1)**   | User truy cập qua Route53 → CloudFront → Amplify (frontend).                          |
| **(2)**   | Amplify giao tiếp với API Gateway hoặc AppSync để gửi/nhận dữ liệu.                   |
| **(3.1)** | API Gateway → Lambda để xử lý logic (CRUD, event, v.v.).                              |
| **(3.2)** | Cognito xác thực và trả token.                                                        |
| **(4)**   | Lambda phát sự kiện qua EventBridge.                                                  |
| **(4.1)** | EventBridge → SES gửi email nhắc nhở.                                                 |
| **(4.2)** | EventBridge → Lambda → Personalize cho ML ranking.                                    |
| **(5)**   | Dev push code lên GitLab → CodePipeline pull → CodeBuild/CodeDeploy → Amplify/Lambda. |
| **(6)**   | CloudWatch thu thập logs và metrics.                                                  |
| **(7)**   | AppSync gửi dữ liệu realtime (chat/notifications) đến frontend.                       |


### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai
Dự án gồm 2 phần — phát triển hệ thống quản lý sinh viên serverless và xây dựng nền tảng phân tích thông minh — mỗi phần trải qua 4 giai đoạn:

1. **Nghiên cứu và vẽ kiến trúc**: Nghiên cứu AWS serverless patterns với DynamoDB và thiết kế kiến trúc tích hợp realtime chat/ranking (1 tháng trước kỳ thực tập).
2. **Tính toán chi phí và kiểm tra tính khả thi**: Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh scope phù hợp (Tháng 1).
3. **Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp**: Tinh chỉnh (ví dụ tối ưu Lambda với React SSR) để đảm bảo hiệu quả (Tháng 2).
4. **Phát triển, kiểm thử, triển khai**: Lập trình React frontend, AWS services với CDK/SDK và AppSync subscriptions, sau đó kiểm thử và đưa vào vận hành (Tháng 2–3).

#### Yêu cầu kỹ thuật

**Hệ thống quản lý sinh viên**: Dashboard web (CRUD sinh viên, điểm số, khóa học), chat realtime, xếp hạng ML. Frontend React/Next.js chạy trên Amplify Hosting, sử dụng AppSync subscriptions để nhận tin nhắn chat thời gian thực và cập nhật ranking. Cognito xác thực và phân quyền cho tất cả người dùng, bao gồm 5-10 admin/giáo viên (với quyền cao như CRUD dữ liệu) và học sinh (với quyền giới hạn như xem điểm số cá nhân, tham gia chat).

**Nền tảng phân tích thông minh**: Kiến thức thực tế về AWS Amplify (hosting React), Lambda (giảm thiểu do SSR xử lý), AWS Personalize (ML ranking), DynamoDB (NoSQL chính), AppSync (GraphQL + subscriptions), và EventBridge (event routing). Sử dụng AWS CDK/SDK để lập trình (ví dụ EventBridge rules tới Lambda cho thông báo). React SSR giúp giảm tải Lambda cho ứng dụng web fullstack.


### 5. Lộ trình & Mốc quan trọng

Dự án được triển khai trong 14 tuần (từ tháng 9 đến tháng 12/2025), chia thành 6 giai đoạn chính theo mô hình Agile.

| **Giai đoạn**               | **Thời gian** | **Mục tiêu chính**          | **Sản phẩm đầu ra (Deliverables)**                                          | **Tiêu chí thành công (Success Criteria)** |
| --------------------------- | ------------- | --------------------------- | --------------------------------------------------------------------------- | ------------------------------------------ |
| **1: Thiết lập nền tảng**   | Tuần 1–2      | Thiết lập môi trường AWS    | • Thiết lập tài khoản AWS<br>• Cấu hình DynamoDB & Cognito<br>• Mẫu IaC<br> | • Hạ tầng AWS hoạt động ổn định<br>        |
| **2: Triển khai Backend**   | Tuần 3–5      | Xây dựng API và logic       | • Các hàm Lambda<br>• Các endpoint API Gateway & AppSync                    | • CRUD & chat mutations hoạt động          |
| **3: Tích hợp Frontend**    | Tuần 6–7      | Triển khai dashboard & chat | • Hosting S3 + CloudFront<br>• Tích hợp subscriptions thời gian thực        | • Dashboard & chat truy cập realtime       |
| **4: ML & Xếp hạng**        | Tuần 8-9      | Tích hợp Personalize        | • Nhập datasets<br>• API xếp hạng                                           | • Xếp hạng sinh viên hoạt động             |
| **5: Thông báo & Giám sát** | Tuần 10–11    | Tích hợp SES & giám sát     | • Thông báo qua email<br>• Cảnh báo CloudWatch<br>                          | • Thông báo gửi chính xác                  |
| **6: Kiểm thử & Đánh giá**  | Tuần 12–14    | Kiểm thử và hoàn thiện      | • Kiểm thử đầu cuối<br>• Tài liệu & Demo                                    | • Hệ thống ổn định, demo hoàn thiện        |


### 6. Ước tính ngân sách

Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=security-monitoring-2025) hoặc tải [tệp ước tính ngân sách](./budget-estimate.xlsx).

#### Chi phí hạ tầng

**Dịch vụ AWS:**


| **Dịch vụ**            | **Mô tả sử dụng**                                           | **Chi phí ước tính / tháng (USD)** | **Ghi chú**                                                                           |
| ---------------------- | ----------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------- |
| **Amazon API Gateway** | Xử lý ~1M API calls/tháng                                   | ~$1.00 – $3.50                     | HTTP APIs: $1.00/triệu cuộc gọi; REST: $3.50/triệu; miễn phí 1M cuộc gọi.             |
| **AWS Lambda**         | ~1M yêu cầu, 400k GB-giây                                   | ~$0.20 – $0.50                     | Yêu cầu: $0.20/triệu; Thời gian: $0.0000166667/GB-giây; miễn phí đủ dùng.             |
| **Amazon DynamoDB**    | ~25 GB lưu trữ, 2.5M đọc/ghi                                | ~$0.25 – $1.25                     | Đọc: $0.25/triệu; Ghi: $1.25/triệu; Lưu trữ: $0.25/GB; miễn phí 25 GB.                |
| **Amazon Cognito**     | ~10k người dùng hoạt động hàng tháng                        | ~$0.0055 – $0.015                  | Cơ bản: $0.015/người dùng; miễn phí 10k người dùng.                                   |
| **Amazon S3**          | ~5 GB lưu trữ, yêu cầu thấp                                 | ~$0.023 – $0.12                    | Lưu trữ: $0.023/GB; Yêu cầu: $0.0004/1k GET; có tín dụng miễn phí.                    |
| **Amazon CloudWatch**  | ~5 GB nhật ký, 10 số liệu/cảnh báo                          | ~$0.03 – $0.50                     | Nhật ký: $0.50/GB thu thập; Lưu trữ: $0.03/GB; Số liệu: $0.30/số liệu; miễn phí 5 GB. |
| **AWS AppSync**        | ~1M truy vấn/đăng ký, chat thời gian thực                   | ~$0.50 – $2.00                     | Yêu cầu: $4.00/triệu; Truyền dữ liệu: $0.09/GB; miễn phí 250k yêu cầu.                |
| **AWS Personalize**    | Huấn luyện mô hình hàng tuần, ~10k tương tác                | ~$0.50 – $5.00                     | Huấn luyện: $0.25/giờ; Suy luận: $0.00005/yêu cầu; Lưu trữ: $0.05/GB.                 |
| **AWS SES**            | ~10k email/tháng                                            | ~$0.10 – $0.30                     | $0.10/1k email; miễn phí 62k email/tháng đầu tiên.                                    |
| **Amazon EventBridge** | ~10k sự kiện/tháng, quy tắc cho kiểm tra                    | ~$0.10 – $0.50                     | $1.00/triệu sự kiện; miễn phí 100k sự kiện/tháng.                                     |
| **AWS Amplify**        | Lưu trữ dashboard, vài lần build/tháng                      | ~$0.00 – $1.00                     | Phút build: $0.01/phút; Lưu trữ: $0.15/GB phục vụ; có miễn phí.                       |
| **Amazon Route 53**    | Truy vấn DNS, vùng lưu trữ                                  | ~$0.50 – $1.00                     | Vùng lưu trữ: $0.50/vùng/tháng; Truy vấn DNS: $0.40/triệu truy vấn.                   |
| **AWS WAF**            | Đánh giá Web ACL, quy tắc                                   | ~$1.00 – $3.00                     | Web ACL: $1.00/tháng; Quy tắc: $0.60/triệu yêu cầu; quy tắc quản lý thêm phí.         |
| **AWS CodeBuild**      | ~10 lần build/tháng, 100 phút                               | ~$0.00 – $0.50                     | $0.005/phút build; miễn phí 100 phút/tháng.                                           |
| **AWS CodeDeploy**     | Triển khai ứng dụng                                         | ~$0.00 – $0.20                     | EC2/tại chỗ: $0.02/triển khai; serverless miễn phí.                                   |
| **Amazon CloudFront**  | CDN phân phối nội dung tĩnh (kết nối Route53 & Amplify/S3)  | ~$0.10 – $1.00                     | Miễn phí 1 TB/tháng đầu tiên trong Free Tier; $0.085/GB sau đó.                       |
| **AWS CodePipeline**   | Tự động hóa CI/CD (kết nối GitLab → CodeBuild → CodeDeploy) | ~$1.00                             | $1.00/tháng cho mỗi pipeline đang hoạt động.                                          |

#### Tổng cộng ước tính
| **Tổng chi phí / tháng (ước lượng)** | **Tổng 3 tháng (ước lượng)** | **Ghi chú**                                                                                                             |
| ------------------------------------ | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **~$7 – $20 / tháng**                | **~$21 – $60 / 3 tháng**     | Phụ thuộc mức sử dụng thực tế (chat, ranking, email, sự kiện); tận dụng gói miễn phí. Thêm chi phí Route53, WAF, CI/CD. |

#### Lưu ý
- Tất cả dịch vụ chính nằm trong gói miễn phí năm đầu.
- Có thể giảm thêm nếu sử dụng AWS Educate / Tín dụng cho sinh viên.
- Personalize & SES chi phí dựa trên khối lượng; tối ưu bằng cách xử lý theo lô.
- EventBridge chi phí dựa trên số sự kiện; tối ưu bằng cách lọc quy tắc.


### 7. Đánh giá rủi ro

Dựa trên NIST Risk Management Framework, nhóm dự án xác định các rủi ro chính và biện pháp giảm thiểu.

| **Mã rủi ro**                     | **Mô tả**                                | **Mức độ**     | **Giảm thiểu (Mitigation)**                                    |
| --------------------------------- | ---------------------------------------- | -------------- | -------------------------------------------------------------- |
| **R1 – Data Leakage**             | Lộ dữ liệu do config sai                 | **Cao**        | Áp dụng Cognito auth, IAM least privilege, DynamoDB encryption |
| **R2 – API Overload**             | Quá nhiều requests gây chậm              | **Trung bình** | Throttling API Gateway/AppSync, alarms CloudWatch              |
| **R3 – Lambda Cold Start**        | Delay khi invoke                         | **Trung bình** | Tối ưu code, Provisioned Concurrency nếu cần                   |
| **R4 – Chi phí vượt**             | Usage tăng bất thường (chat, emails cao) | **Trung bình** | AWS Budgets alerts, monitor Cost Explorer                      |
| **R5 – Service Downtime**         | Gián đoạn AWS                            | **Thấp**       | Multi-AZ config, backups DynamoDB                              |
| **R6 – Realtime Latency**         | Delay trong chat do subscriptions        | **Trung bình** | Sử dụng AppSync caching, test với load cao                     |
| **R7 – ML Accuracy**              | Xếp hạng không chính xác do data kém     | **Trung bình** | Validate datasets, retrain định kỳ với Lambda                  |
| **R8 – Event Processing Failure** | Lỗi routing sự kiện kiểm tra             | **Trung bình** | Dead-letter queues cho EventBridge, test rules                 |

**Contingency Plan (Tóm tắt):**
- Recovery: Sử dụng CloudFormation rebuild nhanh.
- Communication: CloudWatch alarms gửi email.
- Continuous Improvement: Review hàng tuần, bao gồm performance chat, ranking và event handling.


### 8. Kết quả kỳ vọng

**Kết quả kỹ thuật:**
- Hoàn thiện hệ thống serverless quản lý sinh viên với CRUD đầy đủ, chat realtime, ranking ML, thông báo email và xử lý sự kiện kiểm tra.
- Tích hợp API Gateway, Lambda, DynamoDB, Cognito, S3, Amplify, CloudWatch, AppSync, Personalize, SES, EventBridge.
- Thời gian response <1 giây, latency chat <2 giây, event processing <5 giây, uptime ≥99%.
- Chi phí thực tế ≤ $20/3 tháng.

**Kết quả học tập và đào tạo:**
- Người học nắm vững phát triển serverless, realtime apps và event-driven architecture.
- Chuẩn bị cho chứng chỉ AWS Developer Associate.
- Phát triển kỹ năng IaC, NoSQL, Authentication, GraphQL, Personalization, Events.

**Kết quả chuyên môn & trình bày:**
- Báo cáo kèm dashboard, chat, ranking, email và event demo.
- Demo CRUD, chat thời gian thực, thông báo và xử lý sự kiện kiểm tra.
- Tài liệu hướng dẫn triển khai.

**Giá trị dài hạn:**
- Dễ mở rộng sang mobile app hoặc AI analytics nâng cao.
- Nền tảng cho lab đào tạo serverless, realtime và event-driven.