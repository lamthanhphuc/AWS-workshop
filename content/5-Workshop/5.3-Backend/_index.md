---
title : "Backend Deployment: DynamoDB, Lambda, API Gateway, Cognito"
date: "2025-12-07"
weight : 3
chapter : false
pre : "<b> 5.3. </b>"
---


---

#### Overview
This section guides you through the backend implementation of a serverless student management system on AWS. You will use core services such as DynamoDB for data storage, Lambda for business logic, API Gateway to connect the frontend and backend, and Cognito for user authentication. The implementation process includes designing the data table, configuring authentication, building the backend application with Java Spring Boot, packaging and deploying to Lambda, as well as configuring API Gateway to serve student management tasks in a secure, automated, and scalable way.

![Amplify Architecture](/images/5-Workshop/5.3-Backend/architecture.png)
---

# 1. Amazon DynamoDB

DynamoDB is a serverless NoSQL database that stores all system data:

- Student information
- Classes, subjects
- Lecturers
- Scores

Spring Boot backend system interacts with DynamoDB via:

- AWS SDK for Java 17
- Spring Data DynamoDB or repository written by the team
- Presigned URL (upload documents, profiles)

---

## **DynamoDB table design (Single-Table Design)**

### 1.1 Create Tables

**Table 1: Users**
```
Table name: student-management-users
Partition key: id (String)
Sort key: email (String)
```
**Table 2: Classes**
```
Table name: student-management-classes
Partition key: id (String)
```

**Table 3: Subjects**
```
Table name: student-management-subjects
Partition key: id (String)
```

**Table 4: Notifications**
```
Table name: student-management-notifications
Partition key: id (Number)
Sort key: sent_at (String)
```
### 1.2 Global Secondary Index (GSI) Configuration

For the Users table, add GSI:
- Index name: `role-index`
- Partition key: `role` (String)

---

### 2.1 Create User Pool

1. Go to **AWS Console → Cognito → Create user pool**
2. Configuration: 
- Sign-in: Email 
- Password policy: Minimum 8 characters 
- MFA: Optional 
- Email: Send email with Cognito

3. Create App Clients: 
- App client name: `student-management-app` 
- Generate client secret: No 
- Auth flows: `ALLOW_USER_SRP_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`

### 2.2 Save information

```env
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXX
VITE_COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxxx
VITE_COGNITO_REGION=ap-southeast-1
```

---


# 3. Amazon S3
S3 is used for:

- Save student avatar

- Classroom materials

- Build artifacts (React/Vite)

- Deploy website via S3 + CloudFront

- Log & learning material assets

## 4. LAMBDA + GATEWAY API (Backend)

### 4.1 Prepare Java Spring for Lambda

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
### 4.2 Build JAR

```bash
cd backend-project
mvn clean package -DskipTests
# Output: target/your-app.jar
```

### 4.3 Create Lambda Function

1. Go to **AWS Console → Lambda → Create function**
2. Configuration: 
- Function name: `student-management-api` 
- Runtime: Java 17 
- Architecture: x86_64 
- Memory: 512 MB (or 1024 MB for better performance) 
- Timeout: 30 seconds

3. Upload JAR file or from S3

4. Handler: `com.example.StreamLambdaHandler::handleRequest`

### 4.4 Configure IAM Role for Lambda

Attach policies:
- `AmazonDynamoDBFullAccess`
- `AWSLambdaBasicExecutionRole`
- `AmazonCognitoPowerUser`

### 4.5 Create API Gateway

1. Go to **AWS Console → API Gateway → Create API**
2. Select **HTTP API** (recommended) or REST API
3. Configuration: 
- API name: `student-management-api` 
- Integration: Lambda function 
- Route: `ANY /{proxy+}`

4. **Enable CORS:** 
- Access-Control-Allow-Origin: `*` (or specific domain) 
- Access-Control-Allow-Methods: `GET, POST, PUT, PATCH, DELETE, OPTIONS` 
- Access-Control-Allow-Headers: `Content-Type, Authorization`

5. **Deploy API:** 
- Create stage: `prod`
- Save Invoke URL: `https://xxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/prod`

---

# Summary
This section guides you through the implementation of a backend for a serverless student management system on AWS, using key services such as DynamoDB, Lambda, API Gateway, and Cognito. You have practiced:

- Designing and creating DynamoDB tables to store student, class, subject, and notification data, and configuring GSI to optimize queries.
- Setting up Cognito User Pool to authenticate and authorize users.
- Using S3 to store avatars, class materials, and build artifacts.
- Building a backend with Java Spring Boot, packaging the application into a JAR, and deploying it to Lambda.
- Configuring IAM Roles for Lambda to ensure necessary service access.
- Create and configure API Gateway to connect frontend with backend, fully support HTTP methods and CORS security.

This process helps you build a modern, automated, scalable and secure backend, fully meeting student management tasks on AWS platform.