---
title: "Translated Blogs"
date: "2025-12-07"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - Simulating Partial Failures with AWS Fault Injection Service](3.1-Blog1/)
This blog guides you through simulating partial failures in distributed systems using AWS Fault Injection Service (FIS). You will understand why resilience testing is needed by controlled fault injection, how to combine FIS with Application Load Balancer and an AWS Lambda function to redirect a portion of traffic to simulated error responses, and the benefits of this approach for evaluating fault tolerance without disrupting the entire service. The article also presents practical deployment steps—from deploying a CloudFormation template, verifying the Lambda error response, launching the FIS experiment, observing application behavior and automatic rollback—along with notes on IAM permissions, testing in non-production environments, and cleaning up resources after completion.
###  [Blog 2 - Transferring Data from Amazon S3 to IoT Edge Devices](3.2-Blog2/)
This blog provides a step-by-step guide to transferring files from Amazon S3 to IoT Edge devices using AWS IoT Greengrass and Amazon S3 Transfer Manager; you will learn how to build and package a custom component (Download Manager) to download content, deploy that component to a simulated device on EC2, and use AWS IoT Jobs to orchestrate download commands. The article also lists prerequisites (AWS account, EC2, IAM, S3, CLI), configuration and deployment instructions, how to monitor logs and job status to verify downloads, and resource cleanup steps after completion—while highlighting practical benefits such as software updates, firmware delivery, and telemetry collection from edge devices.
###  [Blog 3 - Connecting Amazon WorkSpaces Personal with AWS PrivateLink](3.3-Blog3/)
This blog provides a detailed guide on integrating AWS PrivateLink with Amazon WorkSpaces Personal to establish a private, secure streaming connection without using the public internet; you will understand how PrivateLink reduces the attack surface and keeps all traffic within the AWS network. The article presents practical benefits, deployment steps—from creating the appropriate security group, setting up a VPC Interface Endpoint with DNS and subnet configuration, to configuring the WorkSpaces directory to use the endpoint—along with notes on IP record type (IPv4 only), streaming compatibility, IAM permissions, and recommendations to test only in non-production environments. Finally, the blog outlines verification steps (confirming endpoint status, testing new streaming sessions), options to allow internet streaming for some WorkSpaces, and resource cleanup after completion.


