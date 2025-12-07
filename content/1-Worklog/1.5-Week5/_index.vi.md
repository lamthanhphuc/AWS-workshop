---
title: "Worklog Tuần 5"
date: "2006-01-02"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Hiểu và triển khai AWS Transit Gateway để kết nối nhiều VPCs.
* Học lý thuyết về Amazon EC2.
* Nắm vững các khái niệm Instance Types, AMI, EBS, User Data.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - **Thực hành Lab20:** AWS Transit Gateway <br>&emsp; + Giới thiệu Transit Gateway <br>&emsp; + Preparation steps <br>&emsp; + Create Transit Gateway | 06/10/2025 | 06/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 3   | - **Thực hành Lab20 (tiếp):** <br>&emsp; + Create Transit Gateway Attachments <br>&emsp; + Create Transit Gateway Route Tables | 07/10/2025 | 07/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 4   | - **Thực hành Lab20 (tiếp):** <br>&emsp; + Add Transit Gateway Routes to VPC Route Tables <br>&emsp; + Test connectivity giữa các VPCs <br>&emsp; + Clean up resources | 08/10/2025 | 08/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 5   | - Học lý thuyết Module 03-01: Amazon EC2 <br>&emsp; + Compute VM on AWS <br>&emsp; + Instance Types (General, Compute, Memory, Storage, Accelerated) <br>&emsp; + AMI (Amazon Machine Image) <br>&emsp; + Backup và Key Pair | 09/10/2025 | 09/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 6   | - Học lý thuyết Module 03-01 (tiếp): <br>&emsp; + Elastic Block Store (EBS) <br>&emsp; + Instance Store <br>&emsp; + User Data <br>&emsp; + Metadata <br>&emsp; + EC2 Auto Scaling | 10/10/2025 | 10/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |

### Kết quả đạt được tuần 5:

* Thực hành thành công AWS Transit Gateway:
  * Tạo Transit Gateway để kết nối nhiều VPCs
  * Tạo Transit Gateway Attachments cho từng VPC
  * Cấu hình Transit Gateway Route Tables
  * Cập nhật VPC Route Tables để sử dụng Transit Gateway
  * Test connectivity giữa các VPCs thông qua Transit Gateway

* So sánh được Transit Gateway vs VPC Peering:
  * Transit Gateway: Hub-and-spoke model, scalable, centralized
  * VPC Peering: Point-to-point, không transitive

* Nắm vững lý thuyết Amazon EC2:
  * Các loại Instance Types và use cases
  * AMI là gì và cách tạo custom AMI
  * EBS volumes và các loại (gp2, gp3, io1, io2, st1, sc1)
  * Instance Store vs EBS
  * User Data để bootstrap EC2
  * Metadata service để lấy thông tin instance
  * EC2 Auto Scaling concepts




