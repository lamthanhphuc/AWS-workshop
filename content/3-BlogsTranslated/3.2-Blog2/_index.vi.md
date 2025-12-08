---
title: "Blog 2 - Transfer data from Amazon S3 to IoT Edge device"
date: "2025-06-28"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Chuyển dữ liệu từ Amazon S3 sang thiết bị IoT edge

Được viết bởi Rashmi Varshney, Nilo Bustani, and Tamil Jayakumar | ngày 28 tháng 6 năm 2025 | trong  [AWS IoT Core](https://aws.amazon.com/blogs/iot/category/internet-of-things/aws-iot-platform/), [AWS IoT Greengrass](https://aws.amazon.com/blogs/iot/category/internet-of-things/aws-greengrass/), [Learning Levels](https://aws.amazon.com/blogs/iot/category/learning-levels/), [Technical How-to](https://aws.amazon.com/blogs/iot/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/iot/transfer-data-from-amazon-s3-to-iot-edge-device/) | [Share](https://aws.amazon.com/vi/blogs/iot/transfer-data-from-amazon-s3-to-iot-edge-device/#)

Việc truyền dữ liệu một cách liền mạch giữa đám mây và các thiết bị edge là rất quan trọng cho các ứng dụng IoT trong nhiều ngành như chăm sóc sức khỏe, sản xuất, phương tiện tự hành và hàng không vũ trụ. Ví dụ, nó [enables aircraft operators to seamlessly transfer software updates](https://aws.amazon.com/blogs/industries/aws-and-safran-passenger-innovations/) toàn đội bay mà không cần sử dụng thiết bị lưu trữ vật lý thủ công. Bằng cách tận dụng [AWS IoT](https://aws.amazon.com/iot/) và [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/pm/serv-s3/?gclid=CjwKCAjw1K-zBhBIEiwAWeCOF4sL-QIlVtb-xKGajtiSz2t9K29QR4JX6KAWojyIO5LzC3g-sQu2VxoCH3oQAvD_BwE&trk=20e04791-939c-4db9-8964-ee54c41bc6ad&sc_channel=ps&ef_id=CjwKCAjw1K-zBhBIEiwAWeCOF4sL-QIlVtb-xKGajtiSz2t9K29QR4JX6KAWojyIO5LzC3g-sQu2VxoCH3oQAvD_BwE:G:s&s_kwcid=AL!4422!3!651751060962!e!!g!!amazon%20s3!19852662362!145019251177), bạn có thể thiết lập một cơ chế truyền dữ liệu cho phép trao đổi dữ liệu thời gian thực và lịch sử giữa đám mây và thiết bị edge.

# Giới thiệu

Bài viết này hướng dẫn bạn từng bước cách truyền dữ liệu dưới dạng tệp từ Amazon S3 đến thiết bị IoT Edge của bạn.

 Chúng ta sẽ sử dụng [AWS IoT Greengrass](https://aws.amazon.com/greengrass/), một runtime mã nguồn mở cho edge và dịch vụ đám mây để xây dựng, triển khai từ xa và quản lý phần mềm thiết bị trên hàng triệu thiết bị. IoT Greengrass cung cấp các component được xây dựng sẵn cho các trường hợp sử dụng phổ biến, giúp bạn khám phá, nhập, cấu hình và triển khai ứng dụng và dịch vụ tại edge mà không cần hiểu các giao thức thiết bị khác nhau, quản lý thông tin xác thực hoặc tương tác với các API bên ngoài. Bạn cũng có thể tạo các component tùy chỉnh dựa trên trường hợp sử dụng IoT của bạn.

Trong bài này, chúng tôi sẽ xây dựng và triển khai một component IoT Greengrass tùy chỉnh tận dụng khả năng của [Amazon S3 Transfer Manager](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/examples-s3-transfermanager.html). Component IoT Greengrass này thực hiện các hành động như tải xuống thông qua các topic [IoT Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-what-is.html). Các tham số được đặt trong IoT Jobs xác định các hành động đó.

S3 Transfer Manager sử dụng API multipart upload và các byte-range fetches để truyền tệp từ Amazon S3 đến thiết bị edge. Bạn có thể xem chi tiết thêm về khả năng của S3 Transfer Manager trong [blog](https://aws.amazon.com/blogs/developer/introducing-crt-based-s3-client-and-the-s3-transfer-manager-in-the-aws-sdk-for-java-2-x/) gốc. 

# **Các điều kiện tiên quyết**

Để giả lập thiết bị edge, chúng ta sẽ dùng một EC2 instance. Trước khi tiến hành các bước truyền tệp từ Amazon S3 đến instance của bạn, hãy đảm bảo bạn có các điều kiện sau:

1. Một [AWS account](https://console.aws.amazon.com/) với quyền tạo và truy cập các Amazon EC2 instances,  [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/), [AWS Cloudformation](https://aws.amazon.com/cloudformation/) stack,  [AWS IAM](https://aws.amazon.com/iam) Roles và Policies,  [Amazon S3](https://aws.amazon.com/s3/), [AWS IoT Core](https://aws.amazon.com/iot-core/) và  [Amazon S3](https://aws.amazon.com/s3/), [AWS IoT Core](https://aws.amazon.com/iot-core/)

2. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) đã được cài đặt và cấu hình trên máy tính của bạn, kèm [SSM Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html).

3. Làm theo các bước trong repository  [Visual Studio Code on EC2 for Prototyping](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md) để triển khai một EC2 instance. Sử dụng IDE VS Code chạy trên trình duyệt để chỉnh sửa file và thực thi các hướng dẫn.

*Khi triển khai, EC2 instance sẽ được gắn một IAM Role cho phép truy cập không hạn chế đến mọi tài nguyên AWS. Chúng tôi khuyến nghị bạn xem xét lại role gắn vào EC2 instance và chỉnh sửa để giới hạn quyền chỉ cho SSM, S3, IoT Core và IoT Greengrass.*

---

# **Tổng quan giải pháp**
Việc truyền tệp từ Amazon S3 đến thiết bị edge gồm việc tạo một IoT Greengrass component tùy chỉnh gọi là “Download Manager”. Component này chịu trách nhiệm tải tệp từ Amazon S3 về thiết bị edge, trong trường hợp này là một EC2 instance giả lập thiết bị edge. Quy trình có thể được chia thành các bước sau:

* Bước 1: Phát triển và đóng gói IoT Greengrass Download Manager Component tùy chỉnh, chịu logic truyền tệp. Sau khi đóng gói, truyền component này lên Component được chỉ định and  Content Bucket trên Amazon S3.


* Bước 3: Tải các tệp cần truyền đến thiết bị edge vào ‘Component and Content Bucket’ trên Amazon S3.

* Bước 4: Download Manager Component trên thiết bị edge (EC2) sẽ tải các tệp từ Amazon S3 bucket và lưu chúng vào hệ thống file ở thiết bị edge.

![iotb-727-hla](/images/3-BlogsTranslated/3.2-Blog2/iotb-727-hla.jpeg)

*Hình 1 – Chuyển tệp từ Amazon S3 sang EC2 instance  mô phỏng thiết bị biên*


## **Bước 1: Phát triển và đóng gói component IoT Greengrass Download Manager tùy chỉnh**

1.1 Sao chép thành phần IoT Greengrass tùy chỉnh từ [aws-samples repository](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot)
```bash
git clone https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot.git
cd download-manager
```
1.2 Làm theo [instructions](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot?tab=readme-ov-file#aws-iot-greengrass-core-device-setup) để cấu hình EC2 instance như một thiết bị core IoT Greengrass.  
1.3 The IoT Greengrass Development Kit Command-Line Interface (GDK CLI) đọc từ file cấu hình gdk-config.json để xây dựng và công bố component. Cập nhật file gdk-config.json, thay us-west-2 bằng vùng (region) bạn triển khai và cập nhật gdk\_version theo phiên bản gdk CLI bạn sử dụng:
```json
{
  "component": {
    "com.example.DownloadManager": {
      "author": "Amazon",
      "build": {
        "build_system": "zip",
        "options": {
          "zip_name": ""
        }
      },
      "publish": {
        "bucket": "greengrass-artifacts",
        "region": "us-west-2"
      }
    }
  },
  "gdk_version": "1.3.0"
}
```


## **Bước 2: Xây dựng, công bố và triển khai component Download Manager**

 2.1 Bạn có thể [build](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-development-kit-cli-component.html#greengrass-development-kit-cli-component-build) và [publish](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-development-kit-cli-component.html#greengrass-development-kit-cli-component-publish) Download Manager Component lên Amazon S3 bucket theo [instructions here](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot/blob/main/README.md#build-and-publish-the-component). 

Bước này sẽ tự động tạo một Amazon S3 bucket tên greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID. Các thành phần được xây dựng được lưu trữ dưới dạng các đối tượng trong Amazon S3 bucket này. Chúng tôi sẽ sử dụng Amazon S3 bucket này để xuất bản thành phần Download Manager tùy chỉnh và cũng sử dụng thành phần này để lưu trữ các tài sản sẽ được tải xuống EC2 instance.

 2.2 Thực hiện theo hướng dẫn được đề cập [here](https://docs.aws.amazon.com/greengrass/v2/developerguide/device-service-role.html#device-service-role-access-s3-bucket) để cho phép thiết bị IoT Greengrass core  truy cập vào Amazon S3 bucket.

 2.3 Sau khi xuất bản thành công thành phần Download Manager, bạn có thể tìm thấy nó trong AWS Management Console → AWS IoT Core → Greengrass Devices → Components → My Components.

![IOTB-727-GGComponents](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-GGComponents.jpg)*Hình 2 – Danh sách các thành phần Greengrass của AWS IoTCore*

 2.4 Để cho phép chuyển tệp từ Amazon S3 bucket sang thiết bị biên, chúng tôi sẽ triển khai thành phần Download Manager lên thiết bị Greengrass mô phỏng đang chạy trên EC2 instance. Từ danh sách thành phần ở trên, nhấp vào thành phần có tiêu đề com.example. DownloadManager và nhấn Deploy, chọn Create new deployment và nhấn Next.

 2.5 Nhập tên triển khai là "My Deployment" và "Deployment Target" là "Core Device". Nhập tên thiết bị lõi có thể tìm thấy trong AWS Management Console → AWS IoT Core → Greengrass Devices → Core devices, rồi nhấn "Next".

 2.6 Chọn thành phần: Cùng với thành phần tùy chỉnh, chúng tôi cũng sẽ triển khai các thành phần công khai được cung cấp bởi AWS được liệt kê dưới đây:

* aws.greengrass.Nucleus – Thành phần hạt nhân IoT Greengrass là thành phần bắt buộc và là yêu cầu tối thiểu để chạy phần mềm IoT Greengrass Core trên thiết bị biên.  
* aws.greengrass.Cli – Thành phần IoT Greengrass CLI cung cấp giao diện dòng lệnh cục bộ mà bạn có thể sử dụng trên thiết bị biên để phát triển và gỡ lỗi các thành phần cục bộ. IoT Greengrass CLI cho phép bạn tạo các triển khai cục bộ và khởi động lại các thành phần trên thiết bị biên.  
* aws.greengrass.TokenExchangeService – Dịch vụ trao đổi mã thông báo cung cấp thông tin xác thực AWS có thể được sử dụng để tương tác với các dịch vụ AWS từ các thành phần tùy chỉnh. Điều này rất cần thiết để thư viện boto3 tải xuống các tệp từ Amazon S3 bucket xuống thiết bị biên.

![IOTB-727-DMComponent](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-DMComponent.jpg)

*Hình 3 – Chọn các thành phần để triển khai*

 2.7 Cấu hình Thành phần: Từ danh sách các thành phần Công khai, hãy cấu hình [Nucleus component](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-nucleus-component.html#greengrass-nucleus-component-configuration-interpolate-component-configuration) và bật cờ \`interpolateComponentConfiguration\` thành true. Nên đặt tùy chọn này thành true để thiết bị biên có thể chạy các thành phần IoT Greengrass bằng các [recipe variables](https://docs.aws.amazon.com/greengrass/v2/developerguide/component-recipe-reference.html) từ cấu hình. Thao tác này cũng sẽ tham chiếu đến thingName trong cơ sở mã từ biến môi trường AWS\_IOT\_THING\_NAME và không cần phải mã hóa cứng thingName.

Trong danh sách Cấu hình thành phần, hãy chọn thành phần Nucleus và nhấn Configure Component. Cập nhật phần Configuration để Merge như sau và nhấn Xác nhận.
```json
{
  "interpolateComponentConfiguration": true
}
```
![IOTB-727-configureNucleus](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-configureNucleus.jpg)

*Hình 4 – Cấu hình aws.greengrass.Nucleus*

 2.8 Giữ nguyên cấu hình triển khai mặc định và tiếp tục đến trang Xem lại và nhấp vào Triển khai.

 2.9 Bạn có thể theo dõi quá trình bằng cách xem tệp IoT Greengrass log trên thiết bị IoT Greengrass được mô phỏng đang chạy trên EC2 instance. Bạn sẽ thấy "status=SUCCEEDED" trong log.

sudo tail -f /greengrass/v2/logs/greengrass.log

 2.10 Sau khi triển khai thành công, bạn có thể theo dõi nhật ký cho thành phần Download Manager tùy chỉnh trên thiết bị IoT Greengrass mô phỏng đang chạy trên EC2 instance như hiển thị bên dưới. Bạn sẽ thấy currentState=RUNNING trong nhật ký.

sudo tail -f /greengrass/v2/logs/com.example.DownloadManager.log

 2.11 Thư mục tải xuống được cấu hình thành /opt/downloads khi triển khai Download Manager component Tải xuống tùy chỉnh. Giám sát quá trình tải xuống bằng cách mở cửa sổ terminal trong IDE bằng lệnh sau:
```bash
sudo su
cd /opt/downloads
ls
```

 

## **Bước 3: Upload tệp cần tải về thiết bị edge**

Thành phần Download Manager hỗ trợ việc truyền tệp từ Amazon S3 đến thiết bị biên của bạn. AWS IoT Jobs đóng vai trò quan trọng trong quá trình này bằng cách cho phép bạn xác định và thực hiện các thao tác từ xa trên các thiết bị được kết nối. Với AWS IoT Jobs, bạn có thể tạo một tác vụ hướng dẫn thiết bị biên tải xuống tệp từ một vị trí Amazon S3 bucket được chỉ định. Tác vụ này đóng vai trò như một tập hợp các hướng dẫn, chỉ dẫn thành phần Download Manager tìm kiếm tệp mong muốn trong Amazon S3 bucket. Sau khi tác vụ được tạo và gửi đến thiết bị biên, thành phần Download Manager sẽ bắt đầu quá trình tải xuống, truyền liền mạch các tệp được chỉ định từ Amazon S3 đến bộ nhớ cục bộ của thiết bị biên.

 3.1 Tạo một thư mục có tên là uploads trong Amazon S3 bucket (greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID) đã tạo ở Bước 2.1. Tải hình ảnh được tạo bởi GenAI bên dưới có tên là owl.png vào thư mục uploads trên Amazon S3 bucket.

![IOTB-727-DownloadImage.jpg](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-DownloadImage.jpg)

*Hình 5 – Hình ảnh được tạo bởi GenAI – owl.png*

Để đơn giản hóa, chúng tôi đang sử dụng lại cùng một Amazon S3 bucket<span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">(greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID)</span>. Tuy nhiên, tốt nhất là nên tạo 2 thùng riêng biệt cho các thành phần IoT Greengrass và các tệp cần tải xuống biên.

3.2 Sau khi tệp đã được tải lên Amazon S3 bucket, hãy sao chép S3 URI của hình ảnh này để sử dụng trong bước tiếp theo. S3 URI sẽ là <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">s3://greengrass-artifacts-REGION-ACCOUNT\_ID/uploads/owl\_logo.png</span>.

### 

## **Bước 4: Viết script Python để đồng bộ dữ liệu**

4.1 Tạo AWS IoT Job Document

4.1.1 Từ AWS Management Console, điều hướng đến AWS IoT Core → Remote actions→ Jobs and click Create job.

4.1.2 Chọn tạo công việc tùy chỉnh

4.1.3 Đặt tên công việc, ví dụ: Test-1 và tùy chọn cung cấp mô tả, sau đó nhấp vào Tiếp theo.

4.1.4 Đối với Job Target, hãy chọn thiết bị lõi được chỉ định bởi tên thiết bị< <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;"> YOUR GREENGRASS DEVICE NAME</span> >. Bạn có thể để Thing group trống ngay bây giờ.

4.1.5 Chọn Job document From từ mẫu và chọn AWS-Download-File từ Mẫu.

4.1.6 Dán S3 URI vào phần downloadUrl. S3 URI phải bắt đầu bằng <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">s3://greengrass-artifacts-REGION-ACCOUNT\_ID/uploads/owl\_logo.png</span>

4.1.7 Đối với tệp Path, hãy nhập thư mục con nơi bạn muốn tệp sẽ được tải xuống. Với blog này, chúng ta sẽ tạo một thư mục có tên là images và nhấp vào Next. Không thêm dấu /vào đường dẫn vì thành phần sẽ tự động thêm tiền tố đường dẫn.

4.1.8 Để cấu hình tác vụ và loại chạy, chọn Snapshot và nhấp vào Submit.

4.2 Theo dõi nhật ký thành phần trên EC2 instance để xem thư mục tải xuống đang được tạo và hình ảnh có tên owl.png đang được tải xuống.

<span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">sudo tail \-f /greengrass/v2/logs/com.example.DownloadManager.log</span>

4.3 Theo dõi Tiến trình Tác vụ: Mỗi tài liệu Tác vụ cũng hỗ trợ cập nhật trạng thái thực thi từ cấp độ tác vụ và cấp độ sự vật. Từ AWS Management Console → Jobs → Test-1→ Job executions.

![IOTB-727-TrackJobExecution](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-TrackJobExecution.jpg)

*Hình 6 – Theo dõi việc thực hiện công việc*

4.4 Để xem trạng thái thực hiện từ thiết bị biên, hãy nhấp vào hộp kiểm cho thiết bị lõi trong phần Thực hiện công việc.  
![IOTB-727-ExecutionStatus](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-ExecutionStatus.jpg)
*Hình 7 – Xem chi tiết trạng thái thực hiện công việc*  
4.5 Sau khi tệp đã được tải xuống EC2 instance, bạn có thể tìm thấy tệp đó trong thư mục <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">/opt/downloads/images</span> trong thiết bị lõi.
```bash
sudo su
# cd /opt/downloads/images/
# ls -alh
total 1.1M
drwxrwxr-x 2 ggc_user ggc_group 4.0K Jun 13 17:10 .
drwx------ 3 ggc_user root      4.0K Jun 13 17:10 ..
-rw-rw-r-- 1 ggc_user ggc_group 1.1M Jun 13 17:10 owl_logo.png
```
# **Dọn dẹp**

Để đảm bảo hiệu quả chi phí, blog này sử dụng AWS Free Tier cho tất cả các dịch vụ, ngoại trừ phiên bản EC2 và ổ đĩa EBS được gắn vào phiên bản này. EC2 instance được sử dụng trong ví dụ này yêu cầu On-Demand t3.medium instance theo yêu cầu để chứa cả môi trường phát triển và thiết bị biên được mô phỏng trong cùng một phiên bản EC2 cơ sở. Để biết thêm thông tin, vui lòng tham khảo chi tiết về [pricing](https://aws.amazon.com/ec2/pricing/on-demand/). Sau khi hoàn thành hướng dẫn này, hãy nhớ truy cập AWS Console và xóa các tài nguyên đã tạo trong quá trình này bằng cách làm theo hướng dẫn được cung cấp. Bước này rất quan trọng để tránh phát sinh bất kỳ khoản phí ngoài ý muốn nào trong tương lai.

Hướng dẫn dọn dẹp:

1. Mở S3 từ AWS console và xóa nội dung của Amazon S3 bucket có tên greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID và Amazon S3 bucket.  
2. Mở IoT Core từ AWS console và xóa tất cả các tác vụ khỏi IoT Jobs Manager Dashboard.  
3. Mở IoT Greengrass từ bảng điều khiển AWS và xóa IoT thing Group, Vật, Chứng chỉ, Chính sách và Vai trò được liên kết với MyGreengrassCore.  
4. Làm theo hướng dẫn [cleanup](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md#cleanup) trong kho lưu trữ aws-samples [VS Code on EC2 repository](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md).  
   

# **Tài liệu tham khảo của khách hàng**

[AWS customers](https://aws.amazon.com/blogs/industries/aws-and-safran-passenger-innovations/) đang sử dụng phương pháp này để chuyển tệp từ Amazon S3 sang thiết bị biên.

# **Kết luận**

Bài đăng trên blog này minh họa cách khách hàng AWS có thể di chuyển dữ liệu hiệu quả từ Amazon S3 sang các thiết bị biên của họ. Các bước được trình bày chi tiết cho phép tải xuống liền mạch các bản cập nhật phần mềm, cập nhật chương trình cơ sở, nội dung và các tệp thiết yếu khác. Khả năng giám sát theo thời gian thực cung cấp khả năng hiển thị và kiểm soát toàn diện mọi hoạt động truyền tệp. Bạn có thể tối ưu hóa hơn nữa hoạt động của mình bằng cách triển khai chức năng [pause and resume](https://aws.amazon.com/blogs/developer/pausing-and-resuming-transfers-using-transfer-manager/) được đề cập trong blog. Ngoài ra, bạn có thể sử dụng AWS IoT Greengrass và Amazon S3 Transfer Manager để triển khai luồng dữ liệu ngược từ các thiết bị biên sang Amazon S3. Hơn nữa, thông qua thành phần IoT Greengrass tùy chỉnh, bạn có thể tạo điều kiện thuận lợi cho việc tải lên nhật ký và dữ liệu đo từ xa, mở ra những cơ hội mạnh mẽ cho bảo trì dự đoán, phân tích thời gian thực và thông tin chi tiết dựa trên dữ liệu.

# **Về các tác giả**
</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.2-Blog2/Tamil_resized.jpg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Tamil Jayakumar** Tamil Jayakumar là Kiến trúc sư Giải pháp Chuyên biệt & Kỹ sư Nguyên mẫu tại Amazon Web Services. Anh có hơn 14 năm kinh nghiệm trong lĩnh vực phát triển phần mềm, phát triển Proof of Concept, tạo ra các Sản phẩm Khả thi Tối thiểu (MVP) bằng cách sử dụng kỹ năng phát triển ứng dụng và kiến ​​trúc sư giải pháp toàn diện. Anh là một chuyên gia công nghệ thực hành, đam mê giải quyết các thách thức công nghệ bằng các giải pháp sáng tạo cả về phần mềm và phần cứng, kết hợp nhu cầu kinh doanh với năng lực CNTT.

</td>
</tr>
</table>

</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.2-Blog2/Rashmi_resized-1.jpg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Rashmi Varshney** Rashmi Varshney là Kiến trúc sư Giải pháp Cấp cao tại Amazon Web Services, có trụ sở tại Austin. Cô có hơn 20 năm kinh nghiệm, chủ yếu trong lĩnh vực phân tích. Cô đam mê và thích hỗ trợ khách hàng xây dựng chiến lược áp dụng đám mây, thiết kế các giải pháp sáng tạo và thúc đẩy sự xuất sắc trong vận hành. Là thành viên của Cộng đồng Kỹ thuật Phân tích tại AWS, cô tích cực đóng góp vào các nỗ lực hợp tác trong ngành.
</td>
</tr>
</table>
</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.2-Blog2/Nilo_resized-1.jpg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Nilo Bustani** Nilo Bustani là Kiến trúc sư Giải pháp Cao cấp tại AWS với hơn 20 năm kinh nghiệm trong lĩnh vực phát triển ứng dụng, kiến ​​trúc đám mây và lãnh đạo kỹ thuật. Cô chuyên hỗ trợ khách hàng xây dựng các chiến lược quan sát và thực hành quản trị mạnh mẽ trên các môi trường đám mây lai và đa đám mây. Cô tận tâm cung cấp cho các tổ chức các công cụ và thực hành cần thiết để thành công trong hành trình chuyển đổi sang đám mây và AI.
</td>
</tr>
</table>
