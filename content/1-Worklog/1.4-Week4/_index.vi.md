---
title: "Worklog Tuần 4"
date: "2006-01-02"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Thiết lập Hybrid DNS với Route 53 Resolver.
* Hiểu và thực hành VPC Peering để kết nối nhiều VPCs.
* Sử dụng CloudFormation để triển khai infrastructure.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - **Thực hành Lab10:** Hybrid DNS với Route 53 Resolver <br>&emsp; + Giới thiệu về Hybrid DNS <br>&emsp; + Generate Key Pair <br>&emsp; + Initialize CloudFormation Template | 29/09/2025 | 29/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 3   | - **Thực hành Lab10 (tiếp):** <br>&emsp; + Configure Security Group <br>&emsp; + Connect to RDGW (Remote Desktop Gateway) <br>&emsp; + Set up DNS | 30/09/2025 | 30/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 4   | - **Thực hành Lab10 (tiếp):** <br>&emsp; + Create Route 53 Outbound Endpoint <br>&emsp; + Create Route 53 Resolver Rules <br>&emsp; + Create Route 53 Inbound Endpoints <br>&emsp; + Test results và Clean up | 01/10/2025 | 01/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 5   | - **Thực hành Lab19:** VPC Peering <br>&emsp; + Giới thiệu VPC Peering <br>&emsp; + Initialize CloudFormation Templates <br>&emsp; + Create Security Groups <br>&emsp; + Create EC2 instances | 02/10/2025 | 02/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 6   | - **Thực hành Lab19 (tiếp):** <br>&emsp; + Update Network ACLs <br>&emsp; + Create Peering Connection <br>&emsp; + Configure Route Tables <br>&emsp; + Enable Cross-Peer DNS <br>&emsp; + Clean up resources | 03/10/2025 | 03/10/2025 | <https://www.youtube.com/@AWSStudyGroup> |

### Kết quả đạt được tuần 4:

* Hiểu và thiết lập Hybrid DNS với Route 53 Resolver:
  * Tạo Route 53 Outbound và Inbound Endpoints
  * Cấu hình Resolver Rules để forward DNS queries
  * Kết nối DNS giữa on-premises và AWS

* Thực hành VPC Peering thành công:
  * Tạo Peering Connection giữa hai VPCs
  * Cấu hình Route Tables để enable connectivity
  * Enable Cross-Peer DNS resolution
  * Hiểu cách update Network ACLs cho peering

* Sử dụng CloudFormation:
  * Triển khai infrastructure từ templates
  * Hiểu cách quản lý resources bằng Infrastructure as Code

* Biết cách clean up resources sau khi hoàn thành lab




