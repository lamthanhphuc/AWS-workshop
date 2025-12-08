---
title: "LAMBDA + API GATEWAY"
date: "2025-12-07"
weight: 4
chapter: false
pre: "<b> 5.3.4 </b>"
---

## LAMBDA + API GATEWAY (Backend)

### 1. Chuẩn bị Java Spring cho Lambda

**Thêm dependencies vào pom.xml:**

```xml
<dependencies>
    <!-- AWS Lambda -->
    <dependency>
        <groupId>com.amazonaws.serverless</groupId>
        <artifactId>aws-serverless-java-container-springboot3</artifactId>
        <version>2.0.0</version>
    </dependency>

    <!-- AWS SDK -->
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>dynamodb</artifactId>
        <version>2.21.0</version>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.5.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals><goal>shade</goal></goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

**Tạo Lambda Handler:**

```java
// src/main/java/com/example/StreamLambdaHandler.java
package com.example;

import com.amazonaws.serverless.exceptions.ContainerInitializationException;
import com.amazonaws.serverless.proxy.model.AwsProxyRequest;
import com.amazonaws.serverless.proxy.model.AwsProxyResponse;
import com.amazonaws.serverless.proxy.spring.SpringBootLambdaContainerHandler;
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestStreamHandler;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

public class StreamLambdaHandler implements RequestStreamHandler {
    private static SpringBootLambdaContainerHandler<AwsProxyRequest, AwsProxyResponse> handler;

    static {
        try {
            handler = SpringBootLambdaContainerHandler.getAwsProxyHandler(Application.class);
        } catch (ContainerInitializationException e) {
            throw new RuntimeException("Could not initialize Spring Boot application", e);
        }
    }

    @Override
    public void handleRequest(InputStream inputStream, OutputStream outputStream, Context context)
            throws IOException {
        handler.proxyStream(inputStream, outputStream, context);
    }
}
```

### 2. Build JAR

```bash
cd backend-project
mvn clean package -DskipTests
# Output: target/your-app.jar
```

### 3. Tạo Lambda Function

1. Vào **AWS Console → Lambda → Create function**
2. Cấu hình:

   - Function name: `student-management-api`
   - Runtime: Java 17
   - Architecture: x86_64
   - Memory: 512 MB (hoặc 1024 MB cho performance tốt hơn)
   - Timeout: 30 seconds

![Lambda](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/Lambda.png)

3. Upload JAR file hoặc từ S3

![Jar](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/Jar.png)

4. Handler: `com.example.StreamLambdaHandler::handleRequest`

### 4. Cấu hình IAM Role cho Lambda

Attach policies:

- `AmazonDynamoDBFullAccess`
- `AWSLambdaBasicExecutionRole`
- `AmazonCognitoPowerUser`

### 5. Tạo API Gateway

1. Vào **AWS Console → API Gateway → Create API**
2. Chọn **HTTP API** (recommended) hoặc REST API
3. Cấu hình:

   - API name: `student-management-api`
   - Integration: Lambda function
   - Route: `ANY /{proxy+}`

![APIGateway](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/APIGateway.png)

4. **Enable CORS:**

   - Access-Control-Allow-Origin: `*` (hoặc domain cụ thể)
   - Access-Control-Allow-Methods: `GET, POST, PUT, PATCH, DELETE, OPTIONS`
   - Access-Control-Allow-Headers: `Content-Type, Authorization`

5. **Deploy API:**
   - Create stage: `prod`
   - Lưu Invoke URL: `https://xxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod`

---

## Tổng kết

Phần này hướng dẫn triển khai backend cho hệ thống quản lý sinh viên serverless trên AWS, sử dụng các dịch vụ chủ chốt như DynamoDB, Lambda, API Gateway và Cognito. Bạn đã thực hành:

- Thiết kế và tạo các bảng DynamoDB cho lưu trữ dữ liệu sinh viên, lớp học, môn học, thông báo, cùng cấu hình GSI để tối ưu truy vấn.
- Thiết lập Cognito User Pool để xác thực và phân quyền người dùng.
- Sử dụng S3 cho lưu trữ avatar, tài liệu lớp học và build artifacts.
- Xây dựng backend với Java Spring Boot, đóng gói ứng dụng thành JAR và triển khai lên Lambda.
- Cấu hình IAM Role cho Lambda đảm bảo quyền truy cập dịch vụ cần thiết.
- Tạo và cấu hình API Gateway để kết nối frontend với backend, hỗ trợ đầy đủ các phương thức HTTP và bảo mật CORS.

Quy trình này giúp bạn xây dựng một backend hiện đại, tự động hóa, dễ mở rộng và bảo mật, đáp ứng đầy đủ các nghiệp vụ quản lý sinh viên trên nền tảng AWS.
