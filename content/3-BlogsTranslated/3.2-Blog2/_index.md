---
title: "Blog 2 - Transfer data from Amazon S3 to IoT Edge device"
date: "2025-06-28"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Transferring Data from Amazon S3 to IoT Edge Device

Written by Rashmi Varshney, Nilo Bustani, and Tamil Jayakumar | June 28, 2025 | in [AWS IoT Core](https://aws.amazon.com/blogs/iot/category/internet-of-things/aws-iot-platform/), [AWS IoT Greengrass](https://aws.amazon.com/blogs/iot/category/internet-of-things/aws-greengrass/), [Learning Levels](https://aws.amazon.com/blogs/iot/category/learning-levels/), [Technical How-to](https://aws.amazon.com/blogs/iot/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/iot/transfer-data-from-amazon-s3-to-iot-edge-device/) | [Share](https://aws.amazon.com/vi/blogs/iot/transfer-data-from-amazon-s3-to-iot-edge-device/#)

Seamless data transfer between the cloud and edge devices is critical for IoT applications in many industries such as healthcare, manufacturing, autonomous vehicles, and aerospace. For example, it [enables aircraft operators to seamlessly transfer software updates](https://aws.amazon.com/blogs/industries/aws-and-safran-passenger-innovations/) to the entire fleet without manual physical storage devices. By leveraging [AWS IoT](https://aws.amazon.com/iot/) and [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/pm/serv-s3/?gclid=CjwKCAjw1K-zBhBIEiwAWeCOF4sL-QIlVtb-xKGajtiSz2t9K29QR4JX6KAWojyIO5LzC3g-sQu2VxoCH3oQAvD_BwE&trk=20e04791-939c-4db9-8964-ee54c41bc6ad&sc_channel=ps&ef_id=CjwKCAjw1K-zBhBIEiwAWeCOF4sL-QIlVtb-xKGajtiSz2t9K29QR4JX6KAWojyIO5LzC3g-sQu2VxoCH3oQAvD_BwE:G:s&s_kwcid=AL!4422!3!651751060962!e!!g!!amazon%20s3!19852662362!145019251177), you can set up a data transfer mechanism that enables real-time and historical data exchange between the cloud and edge devices.

# Introduction

This article guides you step-by-step on how to transfer data as files from Amazon S3 to your IoT Edge device.

We will use [AWS IoT Greengrass](https://aws.amazon.com/greengrass/), an open-source edge runtime and cloud service to build, remotely deploy, and manage device software on millions of devices. IoT Greengrass provides built-in components for common use cases, allowing you to discover, import, configure, and deploy applications and services at the edge without needing to understand different device protocols, manage credentials, or interact with external APIs. You can also create custom components based on your IoT use case.

In this article, we will build and deploy a custom IoT Greengrass component leveraging the capabilities of [Amazon S3 Transfer Manager](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/examples-s3-transfermanager.html). This IoT Greengrass component performs actions such as downloading via [IoT Jobs](https://docs.aws.amazon.com/iot/latest/developerguide/jobs-what-is.html) topics. The parameters set in IoT Jobs define these actions.

S3 Transfer Manager uses multipart upload APIs and byte-range fetches to transfer files from Amazon S3 to edge devices. You can read more about S3 Transfer Manager capabilities in the [original blog](https://aws.amazon.com/blogs/developer/introducing-crt-based-s3-client-and-the-s3-transfer-manager-in-the-aws-sdk-for-java-2-x/).

# **Prerequisites**

To simulate an edge device, we will use an EC2 instance. Before proceeding with the steps to transfer files from Amazon S3 to your instance, ensure you have the following:

1. An [AWS account](https://console.aws.amazon.com/) with permissions to create and access Amazon EC2 instances, [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/), [AWS CloudFormation](https://aws.amazon.com/cloudformation/) stacks, [AWS IAM](https://aws.amazon.com/iam) Roles and Policies, [Amazon S3](https://aws.amazon.com/s3/), [AWS IoT Core](https://aws.amazon.com/iot-core/).
2. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) installed and configured on your machine, along with the [SSM Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html).
3. Follow the steps in the [Visual Studio Code on EC2 for Prototyping](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md) repository to deploy an EC2 instance. Use the browser-based VS Code IDE to edit files and execute instructions.

*When deploying, the EC2 instance will be attached to an IAM Role that allows unrestricted access to all AWS resources. We recommend reviewing the role attached to the EC2 instance and editing it to restrict permissions to only SSM, S3, IoT Core, and IoT Greengrass.*

---

# **Solution Overview**
Transferring files from Amazon S3 to an edge device involves creating a custom IoT Greengrass component called “Download Manager.” This component is responsible for downloading files from Amazon S3 to the edge device, in this case, an EC2 instance simulating an edge device. The process can be broken down into the following steps:

* Step 1: Develop and package the custom IoT Greengrass Download Manager Component, which handles file transfer logic. After packaging, upload this component to the designated Component and Content Bucket on Amazon S3.
* Step 3: Upload the files to be transferred to the ‘Component and Content Bucket’ on Amazon S3.
* Step 4: The Download Manager Component on the edge device (EC2) will download files from the Amazon S3 bucket and save them to the file system on the edge device.

![iotb-727-hla](/images/3-BlogsTranslated/3.2-Blog2/iotb-727-hla.jpeg)

*Figure 1 – Transferring files from Amazon S3 to an EC2 instance simulating an edge device*

## **Step 1: Develop and Package the Custom IoT Greengrass Download Manager Component**

1.1 Clone the custom IoT Greengrass component from the [aws-samples repository](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot)
```bash
git clone https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot.git
cd download-manager
```
1.2 Follow the [instructions](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot?tab=readme-ov-file#aws-iot-greengrass-core-device-setup) to configure the EC2 instance as an IoT Greengrass core device.  
1.3 The IoT Greengrass Development Kit Command-Line Interface (GDK CLI) reads from the gdk-config.json configuration file to build and publish the component. Update the gdk-config.json file, replace us-west-2 with the region you are deploying to, and update gdk_version to match your GDK CLI version:
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


## **Step 2: Build, publish and deploy the Download Manager component** 

2.1 You can [build](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-development-kit-cli-component.html#greengrass-development-kit-cli-component-build) and [publish](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-development-kit-cli-component.html#greengrass-development-kit-cli-component-publish) Download Manager Component to Amazon S3 bucket according to [instructions here](https://github.com/aws-samples/sample-asset-transfer-manager-for-edge-iot/blob/main/README.md#build-and-publish-the-component).

This step will automatically create an Amazon S3 bucket named greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID. The components built are stored as objects in this Amazon S3 bucket. We will use this Amazon S3 bucket to publish the custom Download Manager component and also use it to store the assets that will be downloaded to the EC2 instance.

2.2 Follow the instructions mentioned [here](https://docs.aws.amazon.com/greengrass/v2/developerguide/device-service-role.html#device-service-role-access-s3-bucket) to allow the Greengrass core IoT device to access the Amazon S3 bucket.

2.3 After successfully publishing the Download Manager component, you can find it in AWS Management Console → AWS IoT Core → Greengrass Devices → Components → My Components.

![IOTB-727-GGComponents](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-GGComponents.jpg)*Figure 2 – AWS IoTCore Greengrass Components List*

2.4 To enable file transfers from an Amazon S3 bucket to an edge device, we will deploy the Download Manager component to a simulated Greengrass device running on an EC2 instance. From the components list above, click on the component titled com.example.DownloadManager and click Deploy, select Create new deployment and click Next.

2.5 Enter the deployment name as "My Deployment" and the "Deployment Target" as "Core Device". Enter the core device name found in AWS Management Console → AWS IoT Core → Greengrass Devices → Core devices, then click "Next".

2.6 Select Components: Along with the custom component, we will also deploy the public components provided by AWS listed below:

* aws.greengrass.Nucleus – The IoT Greengrass kernel component is a mandatory component and is the minimum requirement to run the IoT Greengrass Core software on the edge.

* aws.greengrass.Cli – The IoT Greengrass CLI component provides a local command line interface that you can use on the edge to develop and debug local components. The IoT Greengrass CLI allows you to create local deployments and reboot components on the edge.

* aws.greengrass.TokenExchangeService – The token exchange service provides AWS credentials that can be used to interact with AWS services from the custom components. This is required for the boto3 library to download files from the Amazon S3 bucket to the edge.

![IOTB-727-DMComponent](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-DMComponent.jpg)

*Figure 3 – Selecting Components to Deploy*

2.7 Component Configuration: From the list of Public components, configure the [Nucleus component](https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-nucleus-component.html#greengrass-nucleus-component-configuration-interpolate-component-configuration) and set the \`interpolateComponentConfiguration\` flag to true. This option should be set to true so that the edge device can run Greengrass IoT components using the [recipe variables](https://docs.aws.amazon.com/greengrass/v2/developerguide/component-recipe-reference.html) from the configuration. This will also reference the thingName in the codebase from the AWS\_IOT\_THING\_NAME environment variable and there is no need to hardcode the thingName.

In the Component Configuration list, select the Nucleus component and click Configure Component. Update the Configuration section to Merge as follows and click Confirm.
```json
{
"interpolateComponentConfiguration": true
}
```
![IOTB-727-configureNucleus](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-configureNucleus.jpg)

*Figure 4 – Configuring aws.greengrass.Nucleus*

2.8 Keep the default deployment configuration and proceed to the Review page and click Deploy.

2.9 You can monitor the process by viewing the IoT Greengrass log file on the simulated IoT Greengrass device running on the EC2 instance. You will see "status=SUCCEEDED" in the log.

sudo tail -f /greengrass/v2/logs/greengrass.log

2.10 After successful deployment, you can monitor the logs for the custom Download Manager component on the simulated Greengrass IoT device running on the EC2 instance as shown below. You will see currentState=RUNNING in the log.

sudo tail -f /greengrass/v2/logs/com.example.DownloadManager.log

2.11 The download directory is configured to /opt/downloads when deploying the Download Manager component Custom Download. Monitoring the download process
```bash
sudo su
cd /opt/downloads
ls
```

 

## **Step 3: Upload the file to the edge device**

The Download Manager component facilitates the transfer of files from Amazon S3 to your edge device. AWS IoT Jobs plays an important role in this process by allowing you to define and execute remote operations on connected devices. With AWS IoT Jobs, you can create a job that instructs the edge device to download a file from a specified Amazon S3 bucket location. This job acts as a set of instructions, instructing the Download Manager component to search for the desired file in the Amazon S3 bucket. Once the job is created and sent to the edge device, the Download Manager component initiates the download process, seamlessly transferring the specified files from Amazon S3 to the edge device's local storage.

3.1 Create a folder named uploads in the Amazon S3 bucket (greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID) created in Step 2.1. Upload the image generated by GenAI below named owl.png to the uploads folder in the Amazon S3 bucket.

![IOTB-727-DownloadImage.jpg](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-DownloadImage.jpg)

*Figure 5 – Image generated by GenAI – owl.png*

For simplicity, we are reusing the same Amazon S3 bucket<span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">(greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID)</span>. However, it is best to create 2 separate buckets for the Greengrass IoT components and the files that need to be downloaded from the edge.

3.2 Once the file has been uploaded to the Amazon S3 bucket, copy the S3 URI of this image for use in the next step. The S3 URI will be <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">s3://greengrass-artifacts-REGION-ACCOUNT\_ID/uploads/owl\_logo.png</span>.

###

## **Step 4: Write a Python script to sync data**

4.1 Create an AWS IoT Job Document

4.1.1 From the AWS Management Console, navigate to AWS IoT Core → Remote actions→ Jobs and click Create job.

4.1.2 Choose to create a custom job

4.1.3 Name the job, e.g., Test-1, and optionally provide a description, then click Next.

4.1.4 For Job Target, select the core device specified by device name< <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;"> YOUR GREENGRASS DEVICE NAME</span> >. You can leave the Thing group blank for now.

4.1.5 Select Job document From from the template and select AWS-Download-File from the Template.

4.1.6 Paste the S3 URI into the downloadUrl section. The S3 URI must start with <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">s3://greengrass-artifacts-REGION-ACCOUNT\_ID/uploads/owl\_logo.png</span>

4.1.7 For File Path, enter the subdirectory where you want the file to be downloaded. For this blog, we will create a folder called images and click Next. Do not add a / to the path as the component will automatically prefix the path.

4.1.8 To configure the task and run type, select Snapshot and click Submit.

4.2 Monitor the component log on the EC2 instance to see the download folder being created and the image named owl.png being downloaded.

<span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">sudo tail \-f /greengrass/v2/logs/com.example.DownloadManager.log</span>

4.3 Monitor the Task Progress: Each Task document also supports updating the execution status from the task level and the thing level. From AWS Management Console → Jobs → Test-1→ Job executions.

![IOTB-727-TrackJobExecution](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-TrackJobExecution.jpg)

*Figure 6 – Tracking Job Execution*

4.4 To view the execution status from the edge device, click the checkbox for the core device in the Job Execution section.

![IOTB-727-ExecutionStatus](/images/3-BlogsTranslated/3.2-Blog2/IOTB-727-ExecutionStatus.jpg)
*Figure 7 – Viewing Job Execution Status Details*
4.5 Once the file has been downloaded to the EC2 instance, you can find it in the <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">/opt/downloads/images</span> folder in the core device.
```bash
sudo su
# cd /opt/downloads/images/
# ls -alh
total 1.1M
drwxrwxr-x 2 ggc_user ggc_group 4.0K Jun 13 17:10 .
drwx------- 3 ggc_user root 4.0K Jun 13 17:10 ..
-rw-rw-r-- 1 ggc_user ggc_group 1.1M Jun 13 17:10 owl_logo.png
```

# **Cleanup**

To ensure cost efficiency, this blog uses the AWS Free Tier for all services, except for the EC2 instance and the EBS volumes attached to it. The EC2 instance used in this example requires an On-Demand t3.medium instance on demand to house both the development environment and the simulated edge device in the same underlying EC2 instance. For more information, please refer to the [pricing](https://aws.amazon.com/ec2/pricing/on-demand/) details. After completing this tutorial, be sure to go to the AWS Console and delete the resources created during this process by following the instructions provided. This step is important to avoid any unexpected charges in the future.

Cleanup Instructions:

1. Open S3 from the AWS console and delete the contents of the Amazon S3 bucket named greengrass-artifacts-YOUR\_REGION-YOUR\_AWS\_ACCOUNT\_ID and the Amazon S3 bucket.

2. Open IoT Core from the AWS console and delete all tasks from the IoT Jobs Manager Dashboard.

3. Open IoT Greengrass from the AWS console and delete the IoT thing Group, Object, Certificate, Policy, and Role associated with MyGreengrassCore.

4. Follow the [cleanup](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md#cleanup) instructions in the aws-samples [VS Code on EC2 repository](https://github.com/aws-samples/vscode-on-ec2-for-prototyping/blob/main/README.md).

# **Customer References**

[AWS customers](https://aws.amazon.com/blogs/industries/aws-and-safran-passenger-innovations/) are using this approach to transfer files from Amazon S3 to edge devices.

# **Conclusion**

This blog post illustrates how AWS customers can efficiently move data from Amazon S3 to their edge devices. The detailed steps enable seamless downloads of software updates, firmware updates, content, and other essential files. Real-time monitoring provides complete visibility and control of all file transfers. You can further optimize your operations by implementing the [pause and resume](https://aws.amazon.com/blogs/developer/pausing-and-resuming-transfers-using-transfer-manager/) functionality mentioned in the blog. Additionally, you can use AWS IoT Greengrass and Amazon S3 Transfer Manager to implement upstream data flows from edge devices to Amazon S3. Furthermore, through a custom IoT Greengrass component, you can facilitate the upload of logs and telemetry data, opening up powerful opportunities for predictive maintenance, real-time analytics, and data-driven insights.

# **About the Authors**
</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.2-Blog2/Tamil_resized.jpg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Tamil Jayakumar** Tamil Jayakumar is a Dedicated Solutions Architect & Prototype Engineer at Amazon Web Services. He has over 14 years of experience in software development, developing Proof of Concept, creating Minimum Viable Products (MVPs) using end-to-end application development and solution architect skills. He is a hands-on technologist, passionate about solving technology challenges with innovative software and hardware solutions, combining business needs with IT capabilities.

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

**Rashmi Varshney** Rashmi Varshney is a Senior Solutions Architect at Amazon Web Services, based in Austin. She has over 20 years of experience, primarily in analytics. She is passionate about helping customers build cloud adoption strategies, design innovative solutions, and drive operational excellence. As a member of the AWS Analytics Engineering Community, she actively contributes to industry collaboration efforts. </td>
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

**Nilo Bustani** Nilo Bustani is a Senior Solutions Architect at AWS with over 20 years of experience in application development, cloud architecture, and technical leadership. She specializes in helping customers build strong observability strategies and governance practices across hybrid and multi-cloud environments. She is dedicated to providing organizations with the tools and practices needed to succeed in their journey to cloud and AI transformation.

</td>
</tr>
</table>
