---
title: "Desktop and Application Streaming"
date: "2025-06-26"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---



# Kết nối Amazon WorkSpaces Personal với AWS PrivateLink

 Được viết bởi Dave Jaskie, Gekai Zou, Aamir Khan, and Anshu Prabhat | ngày 26 tháng 6 năm2025 | trong [Amazon WorkSpaces](https://aws.amazon.com/blogs/desktop-and-application-streaming/category/end-user-computing/amazon-workspaces/), [AWS PrivateLink](https://aws.amazon.com/blogs/desktop-and-application-streaming/category/networking-content-delivery/aws-privatelink/), [End User Computing](https://aws.amazon.com/blogs/desktop-and-application-streaming/category/end-user-computing/) | [Permalink](https://aws.amazon.com/blogs/desktop-and-application-streaming/connecting-amazon-workspaces-personal-with-aws-privatelink/) [| Share](https://aws.amazon.com/vi/blogs/desktop-and-application-streaming/connecting-amazon-workspaces-personal-with-aws-privatelink/#)


Khách hàng thường hỏi cách tận dụng AWS PrivateLink để kết nối với Amazon WorkSpaces Personal. PrivateLink cung cấp một cách liền mạch để thiết lập kết nối riêng tư, an toàn mà không cần sử dụng các thành phần mạng truyền thống như internet gateway, thiết bị NAT, hoặc cấu hình VPN. Cách tiếp cận này không chỉ đơn giản hóa kiến trúc mạng mà còn tăng cường bảo mật bằng cách giảm đáng kể bề mặt tấn công và giữ toàn bộ lưu lượng dữ liệu nằm an toàn bên trong mạng AWS. Trong bài viết này, chúng tôi sẽ hướng dẫn bạn từng bước cách tích hợp PrivateLink với WorkSpaces, giúp bạn mở khóa những lợi ích này trong môi trường của mình.


---

## Điều kiện tiên quyết và các hạn chế

PrivateLink cho WorkSpaces hiện được hỗ trợ cho lưu lượng streaming. Hãy xem [Admin Guide](https://docs.aws.amazon.com/workspaces/latest/adminguide/creating-streaming-vpc-endpoints.html) để biết chi tiết về [prerequisites and limitations with WorkSpaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/creating-streaming-vpc-endpoints.html). Để cấu hình PrivateLink, bạn trước hết cần thiết lập nhóm bảo mật (security group), tạo VPC endpoint và cuối cùng cấu hình thư mục WorkSpaces để dùng VPC endpoint.

---

## Bước 1: Tạo nhóm bảo mật

Trong bước này, bạn tạo một security group cho phép các client WorkSpaces giao tiếp với VPC endpoint mà bạn sẽ tạo.

1. Trong bảng điều hướng của [Amazon EC2 console](https://console.aws.amazon.com/ec2), vào **Network & Security**, rồi chọn **Security Groups**.
2. Chọn **Create security group**.
3. Tại phần **Basic details**, nhập:  
   For **Security group name** – Nhập tên duy nhất để xác định nhóm bảo mật.  
   For **Description** – Nhập mô tả về mục đích của nhóm bảo mật.  
   For **VPC** – Chọn VPC mà VPC endpoint của bạn sẽ nằm trong đó.
4. Vào phần **Inbound rules** và chọn **Add rule** để tạo quy tắc inbound cho TCP.
5. Nhập các thông tin sau:  
   For **Type** – Chọn “Custom TCP”.  
   For **Port range** – Nhập các cổng: 443, 4195.  
   For **Source type** – Chọn “Custom”.  
   For **Source** – Nhập dải IP CIDR riêng hoặc Security Group IDs khác mà người dùng của bạn kết nối với điểm cuối VPC. Đảm bảo chỉ cho phép lưu lượng truy cập đến từ nguồn địa chỉ IPv4. Chắc chắn cho phép lưu lượng inbound từ địa chỉ IPv4.
6. Lặp lại bước 4 và 5 cho mỗi phạm vi CIDR hoặc security group.  
7. Vào **Inbound rules**, chọn **Add rule** để tạo quy tắc inbound cho UDP.  
8. Nhập:  
   For **Type** – Chọn “Custom TCP”.  
   For **Port range** – Nhập các cổng: 443, 4195.  
   For **Source type** – Chọn “Custom”.  
   For **Source** – Nhập cùng dải IP CIDR riêng tư hoặc Security Group IDs đã nhập ở Bước 5.
9. Lặp lại bước 7 và 8 cho từng nguồn UDP tùy chỉnh.  
10. Chọn **Create security group**.

---

## Bước 2: Tạo VPC endpoint

Trong Amazon VPC, VPC endpoint cho phép bạn kết nối VPC tới các dịch vụ AWS được hỗ trợ. Trong ví dụ này, bạn cấu hình VPC để người dùng WorkSpaces có thể thực hiện stream từ WorkSpaces.

1. Mở [Amazon VPC console](https://console.aws.amazon.com/vpc/).
2. Trong bảng điều hướng, vào **Endpoints**, rồi chọn **Create Endpoint**.
3. Chọn **Create Endpoint**.
4. Đảm bảo các thiết lập sau:  
   **Service category** – Chọn “AWS services”.  
   **Service Name** – Chọn **com.amazonaws.Region.prod.highlander.**  
   **VPC** – Chọn VPC mà bạn muốn tạo interface endpoint. Bạn có thể chọn một VPC khác với VPC có tài nguyên WorkSpaces miễn là mạng có thể định tuyến lưu lượng đó đến VPC endpoint.  
   **Enable Private DNS Name** – Bỏ chọn nếu người dùng sử dụng proxy mạng để truy cập các phiên streaming; vô hiệu hóa cache proxy trên domain và tên DNS liên quan đến endpoint riêng.  
   **DNS record IP type** – Chọn IPv4, vì IPv6 và Dualstack hiện chưa được hỗ trợ.  
   **Subnets** – Chọn các subnet (các Availability Zones) để tạo VPC endpoint. Khuyên chọn ít nhất hai subnet.  
   **IP address type** – Chọn IPv4.  
   **Security groups panel** – Chọn security group bạn đã tạo ở bước 1.  
   (Tuỳ chọn) Ở thẻ **Tags**, bạn có thể thêm tag nếu muốn.
5. Chọn **Create endpoint**.
6. Khi endpoint sẵn sàng dùng, trạng thái trong cột **Status** chuyển sang Available.

---

## Bước 3: Cấu hình thư mục WorkSpaces để sử dụng VPC endpoint

Bạn cần cấu hình thư mục WorkSpaces để sử dụng VPC endpoint mà bạn đã tạo cho streaming.

1. Mở [WorkSpaces console](https://console.aws.amazon.com/workspaces/v2/home) trong cùng vùng AWS như VPC endpoint.  
2. Trong **Navigation pane**, chọn **Directories**.  
3. Chọn thư mục (**directory**) bạn muốn sử dụng.  
4. Vào phần **VPC Endpoints section**, rồi chọn **Edit.**


![VPC1-1](/images/3-BlogsTranslated/3.3-Blog3/VPC2.png)

5. Trong **Edit VPC Endpoint dialog box**, dưới mục **Streaming Endpoint**, chọn VPC endpoint bạn đã tạo.

![VPC1-1](/images/3-BlogsTranslated/3.3-Blog3/VPC1-1.png)

6. **Tuỳ chọn:** bạn có thể **bật Allow users with PCoIP WorkSpaces** có thể stream từ internet. Nếu bật, người dùng PCoIP WorkSpaces có thể stream qua Internet công cộng. Nếu không bật, các PCoIP WorkSpaces trong thư mục không dùng được khi không có internet vì PCoIP WorkSpaces không hỗ trợ streaming qua VPC endpoint.
7. Chọn **Save**.  
   Lưu ý: Lưu lượng cho các phiên streaming mới sẽ được định tuyến qua VPC endpoint này. Tuy nhiên, các phiên đang chạy vẫn tiếp tục sử dụng endpoint cũ đã cấu hình trước đó.

---

## Kết luận

Trong bài viết này, chúng tôi đã thảo luận cách thiết lập Amazon WorkSpaces Personal với AWS PrivateLink. Nếu bạn cần thêm thông tin về PrivateLink, hãy xem trang sản phẩm AWS. Nếu có câu hỏi, bạn có thể liên hệ đội ngũ hỗ trợ AWS. Để cập nhật các tính năng mới cho End User Compute, hãy kiểm tra “What’s New with AWS” và playlist YouTube tương ứng.

---

## Tác giả

---

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.3-Blog3/Jaskie1.jpeg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Dave Jaskie** là Kiến trúc sư Giải pháp Điện toán Người dùng Cuối Cấp cao của AWS, với 15 năm kinh nghiệm trong lĩnh vực điện toán người dùng cuối. Ngoài công việc, Dave thích đi du lịch và đi bộ đường dài cùng vợ và 4 con.

</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.3-Blog3/Aamir2.png" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Aamir Khan** Quản lý Chương trình Kỹ thuật Cấp cao kỳ cựu trong nhóm Sản phẩm Máy tính Người dùng Cuối, tự hào với 12 năm kinh nghiệm trong ngành. Với phương châm đặt khách hàng lên hàng đầu, phương pháp làm việc của anh đã đặt ra một chuẩn mực. Ngoài công việc chuyên môn, Aamir còn tìm thấy niềm vui trong những khoảnh khắc gia đình và thỉnh thoảng tận hưởng những chuyến phiêu lưu địa hình trên chiếc LC-100 của mình, khám phá những điều kỳ diệu của vùng Tây Bắc Thái Bình Dương.

</td>
</tr>
</table>


<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.3-Blog3/Anshu.png" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Anshu Prabhat** là Kỹ sư Phát triển Phần mềm III tại AWS thuộc Tổ chức Hỗ trợ Công việc An toàn với hơn 12 năm kinh nghiệm trong lĩnh vực điện toán đám mây an toàn và các dịch vụ có khả năng mở rộng tại Amazon. Anshu thích tìm hiểu và khám phá thế giới GenAI và cố gắng tự động hóa các tác vụ cá nhân thông thường bằng cách sử dụng Agent.
</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.3-Blog3/image-5.png" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Gekai Zou** là Trưởng phòng Kỹ thuật Sản phẩm Cấp cao của AWS End User Computing. Gekai đã làm việc tại AWS từ năm 2019. Ngoài giờ làm việc, Gekai thích cắm trại và trượt tuyết cùng gia đình.

</td>
</tr>
</table>




