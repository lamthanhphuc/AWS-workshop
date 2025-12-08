---
title: "Các bài blogs đã dịch"
date: "2025-12-07"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - Mô phỏng sự cố từng phần với AWS Fault Injection Service](3.1-Blog1/)
Blog này hướng dẫn cách mô phỏng các sự cố từng phần trong hệ thống phân tán bằng AWS Fault Injection Service (FIS). Bạn sẽ hiểu lý do cần kiểm thử độ bền hệ thống bằng cách tiêm lỗi có kiểm soát, cách kết hợp FIS với Application Load Balancer và một hàm AWS Lambda để chuyển một tỷ lệ lưu lượng nhất định sang phản hồi lỗi giả lập, và lợi ích của phương pháp này trong việc đánh giá khả năng chịu lỗi mà không làm gián đoạn toàn bộ dịch vụ. Bài viết cũng trình bày các bước triển khai thực tế — từ triển khai mẫu CloudFormation, xác minh hàm Lambda trả lỗi, khởi chạy thí nghiệm FIS, quan sát hành vi ứng dụng và cơ chế rollback tự động — cùng những lưu ý về quyền hạn IAM, kiểm thử ở môi trường không phải production và dọn dẹp tài nguyên sau khi hoàn tất.
###  [Blog 2 - Chuyển dữ liệu từ Amazon S3 sang thiết bị IoT edge](3.2-Blog2/)
Blog này hướng dẫn quy trình từng bước để chuyển file từ Amazon S3 đến các thiết bị IoT Edge bằng cách sử dụng AWS IoT Greengrass và Amazon S3 Transfer Manager; bạn sẽ học cách xây dựng và đóng gói một component tùy chỉnh (Download Manager) để tải xuống nội dung, triển khai component đó lên một thiết bị giả lập trên EC2, và sử dụng AWS IoT Jobs để điều phối các lệnh tải về. Bài viết cũng liệt kê các yêu cầu tiền đề (tài khoản AWS, EC2, IAM, S3, CLI), chỉ dẫn cấu hình và triển khai, cách theo dõi nhật ký và trạng thái job để xác minh việc tải xuống, cùng các bước dọn dẹp tài nguyên sau khi hoàn tất — đồng thời nêu rõ lợi ích thực tiễn như cập nhật phần mềm, truyền firmware và thu thập telemetry từ edge thiết bị.
###  [Blog 3 - Kết nối Amazon WorkSpaces Personal với AWS PrivateLink](3.3-Blog3/)
Blog này hướng dẫn chi tiết cách tích hợp AWS PrivateLink với Amazon WorkSpaces Personal để thiết lập kết nối riêng tư, an toàn cho luồng streaming mà không cần dùng internet công cộng; bạn sẽ hiểu vì sao PrivateLink giúp giảm bề mặt tấn công và giữ toàn bộ lưu lượng trong mạng AWS. Bài viết trình bày lợi ích thực tiễn, các bước triển khai — từ tạo security group phù hợp, thiết lập VPC Interface Endpoint với cấu hình DNS và subnet, đến cấu hình directory của WorkSpaces để sử dụng endpoint — cùng các lưu ý về loại bản ghi IP (IPv4 only), tương thích streaming, quyền IAM, và khuyến cáo chỉ thử nghiệm trên môi trường không phải production. Cuối cùng, bài nêu các bước kiểm chứng hoạt động (xác nhận trạng thái endpoint, kiểm thử phiên streaming mới), tùy chọn cho phép streaming qua internet cho một số WorkSpaces, và việc dọn dẹp tài nguyên sau khi hoàn tất.


