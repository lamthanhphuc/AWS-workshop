---
title: "LAMBDA + API GATEWAY"
date: "2025-12-07"
weight: 4
chapter: false
pre: "<b> 5.3.4 </b>"
---

## LAMBDA + GATEWAY API (Backend)

### 1. Prepare Java Spring for Lambda

**Add dependencies to pom.xml:**

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

**Create Lambda Handler:**

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

### 3. Create Lambda Function

1. Go to **AWS Console → Lambda → Create function**
2. Configuration:

- Function name: `student-management-api`
- Runtime: Java 17
- Architecture: x86_64
- Memory: 512 MB (or 1024 MB for better performance)
- Timeout: 30 seconds

![Lambda](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/Lambda.png)

3. Upload JAR file or from S3

![Jar](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/Jar.png)

4. Handler: `com.example.StreamLambdaHandler::handleRequest`

### 4. Configure IAM Role for Lambda

Attach policies:

- `AmazonDynamoDBFullAccess`
- `AWSLambdaBasicExecutionRole`
- `AmazonCognitoPowerUser`

### 5. Create API Gateway

1. Go to **AWS Console → API Gateway → Create API**
2. Select **HTTP API** (recommended) or REST API
3. Configuration:

- API name: `student-management-api`
- Integration: Lambda function
- Route: `ANY /{proxy+}`

![APIGateway](/images/5-Workshop/5.3-Backend/5.3.4-LAMBDA&APIGATEWAY/APIGateway.png)

4. **Enable CORS:**

- Access-Control-Allow-Origin: `*` (or specific domain)
- Access-Control-Allow-Methods: `GET, POST, PUT, PATCH, DELETE, OPTIONS`
- Access-Control-Allow-Headers: `Content-Type, Authorization`

5. **Deploy API:**

- Create stage: `prod`
- Save Invoke URL: `https://xxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod`

---

## Summary

This section guides you through the implementation of a backend for a serverless student management system on AWS, using key services such as DynamoDB, Lambda, API Gateway, and Cognito. You have practiced:

- Designing and creating DynamoDB tables to store student, class, subject, and notification data, and configuring GSI to optimize queries.
- Setting up Cognito User Pool to authenticate and authorize users.
- Using S3 to store avatars, class materials, and build artifacts.
- Building a backend with Java Spring Boot, packaging the application into a JAR, and deploying it to Lambda.
- Configuring IAM Roles for Lambda to ensure necessary service access.
- Create and configure API Gateway to connect frontend with backend, fully support HTTP methods and CORS security.

This process helps you build a modern, automated, scalable and secure backend, fully meeting student management tasks on AWS platform.
