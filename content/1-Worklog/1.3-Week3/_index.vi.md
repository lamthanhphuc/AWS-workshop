---
title: "Worklog Tuần 3"
date: "2025-09-22"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Thực hành triển khai VPC hoàn chỉnh với EC2 instances.
* Hiểu cách kết nối EC2 trong các subnets khác nhau.
* Thực hành EC2 Instance Connect Endpoint.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - **Thực hành Lab03-03:** Tạo VPC từ đầu <br>&emsp; + Tạo VPC <br>&emsp; + Tạo Subnets (Public/Private) <br>&emsp; + Tạo Internet Gateway <br>&emsp; + Tạo Route Table cho Outbound Internet Routing | 22/09/2025 | 22/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 3   | - **Thực hành Lab03-03 (tiếp):** <br>&emsp; + Tạo Security Groups <br>&emsp; + Cấu hình Inbound/Outbound rules | 23/09/2025 | 23/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 4   | - **Thực hành Lab03-04:** EC2 Instances trong Subnets <br>&emsp; + Tạo EC2 Instances trong Public Subnet <br>&emsp; + Tạo EC2 Instances trong Private Subnet <br>&emsp; + Test kết nối giữa các instances | 24/09/2025 | 24/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 5   | - **Thực hành Lab03-04 (tiếp):** <br>&emsp; + Tạo NAT Gateway <br>&emsp; + Cấu hình Route Table cho Private Subnet <br>&emsp; + Test kết nối Internet từ Private Subnet | 25/09/2025 | 25/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 6   | - **Thực hành:** EC2 Instance Connect Endpoint <br>&emsp; + Tạo EC2 Instance Connect Endpoint <br>&emsp; + Kết nối vào Private EC2 không cần Bastion Host <br>&emsp; + Clean up resources | 26/09/2025 | 26/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |

### Kết quả đạt được tuần 3:

* Thực hành thành công việc tạo VPC hoàn chỉnh:
  * Tạo VPC với CIDR block phù hợp
  * Tạo Public và Private Subnets
  * Cấu hình Internet Gateway và NAT Gateway
  * Thiết lập Route Tables cho từng loại subnet

* Triển khai EC2 instances trong VPC:
  * Tạo EC2 trong Public Subnet với Public IP
  * Tạo EC2 trong Private Subnet
  * Test kết nối SSH và Internet connectivity

* Sử dụng EC2 Instance Connect Endpoint:
  * Hiểu cách kết nối vào Private EC2 mà không cần Bastion Host
  * Tiết kiệm chi phí và tăng bảo mật

* Biết cách clean up resources để tránh phát sinh chi phí:
  * Terminate EC2 instances
  * Delete NAT Gateway
  * Release Elastic IPs
  * Delete VPC và các thành phần liên quan




