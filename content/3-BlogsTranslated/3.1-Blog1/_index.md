---
title: "Blog 1 - AWS Fault Injection Service"
date: "2025-06-30"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# **Simulating Partial Failures with AWS Fault Injection Service**

Written by Ozgur Canibeyaz and Pablo Colazurdo | June 30, 2025 | in [AWS Fault Injection Service (FIS)](https://aws.amazon.com/blogs/mt/category/developer-tools/aws-fault-injection-service-fis/), [AWS Resilience Hub (ARH)](https://aws.amazon.com/blogs/mt/category/management-and-governance/aws-resilience-hub/), [AWS Systems Manager](https://aws.amazon.com/blogs/mt/category/management-tools/aws-systems-manager/), [Management Tools](https://aws.amazon.com/blogs/mt/category/management-tools/), [Resilience](https://aws.amazon.com/blogs/mt/category/resilience/), [Technical How-to](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/mt/simulating-partial-failures-with-aws-fault-injection-service/) | [Share](https://aws.amazon.com/vi/blogs/mt/simulating-partial-failures-with-aws-fault-injection-service/#)

A modern distributed system must be able to withstand unexpected disruptions to maintain availability, performance, and stability. [Chaos engineering](https://en.wikipedia.org/wiki/Chaos_engineering) helps teams uncover hidden weaknesses by intentionally injecting faults into the system and observing how it recovers. While traditional testing validates expected behavior, chaos engineering tests the system’s fault tolerance under failure conditions. [AWS Fault Injection Service](https://aws.amazon.com/fis/) (AWS FIS) is a fully managed AWS service that helps teams run fault injection experiments on AWS workloads. It supports scenarios such as terminating [Amazon EC2 instances](https://aws.amazon.com/ec2/), throttling [Amazon API Gateway](https://aws.amazon.com/api-gateway/) requests, and inducing network latency. This allows you to verify fault tolerance in a production-like environment. While these capabilities are powerful, many real-world failures only affect a portion of traffic.

In this article, you’ll learn how to simulate partial failures—a common but rarely tested failure mode—by combining AWS FIS with weighted routing in [Application Load Balancer (ALB)](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/) and a [AWS Lambda](https://aws.amazon.com/lambda/) function that returns custom error responses. This approach lets you test how your application handles degraded conditions without changing code or disrupting normal traffic.

# **Overview of Partial Failures**

Our solution combines AWS FIS with weighted routing on ALB to direct a configurable percentage of traffic to a Lambda function that returns simulated errors. This approach requires no application code changes and automatically reverts to normal operation after the experiment ends.  
![experiment-flow](/images/3-BlogsTranslated/3.1-Blog1/experiment-flow.png)

*Figure 1 illustrates how the solution modifies the Load Balancer to inject faults during the experiment and automatically restores it afterward.*

# **Key Benefits**

This solution provides key benefits for teams implementing chaos engineering:

* Controlled fault simulation.
* No application changes required.
* Automated setup and rollback.
* Configurable error rate.

# **Deployment Guide**

## **Prerequisites**

Before you begin, make sure you have:

* An AWS account with permissions to deploy [AWS CloudFormation](https://aws.amazon.com/cloudformation/) stacks and manage AWS FIS experiments.
* An ALB configured with a target group routing traffic to a running microservice.
* ALB must be active and publicly accessible to test simulated errors.
* [AWS Command Line Interface](https://aws.amazon.com/cli/) (AWS CLI) or access to the AWS Management Console.

## **Step 1: Deploy the CloudFormation Template**

The CloudFormation template sets up all required resources, including:

* A Lambda function to simulate error responses.
* An automation document for [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/).
* An IAM role granting AWS FIS permission to invoke the SSM automation document.
* An [AWS FIS experiment template](https://docs.aws.amazon.com/fis/latest/userguide/experiment-template-example.html).

![overall-architecture](/images/3-BlogsTranslated/3.1-Blog1/overall-architecture.png)
*Figure 2 shows an overview of the solution components and their interactions.*

## **Configurable Experiment Parameters**

The CloudFormation template requires three parameters at deployment:

* Name of the Application Load Balancer.
* ARN of the ALB listener rule to modify.
* Experiment duration in seconds—the time during which partial failure is simulated.

Other experiment settings, such as the percentage of redirected traffic and Lambda response code, are preconfigured in the experiment definition. If you want to customize these values, you have two options:

**Option 1: Edit the CloudFormation Template and Redeploy**  
You can edit the <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">documentParameters</span> field in the experiment definition to change:

* FailurePercentage (e.g., 10, 50, 100).

To change the Lambda function’s HTTP response code (e.g., from 500 to 404), edit the <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">statusCode</span> value directly in the inline code section of the template.

After editing, redeploy the stack to apply changes.

**Option 2: Create a New Version of the SSM Automation Document**  
If you don’t want to redeploy the entire stack:

1. Go to **AWS Systems Manager** → **Documents** console.
2. Find the SSM document created by the template.
3. Select **Create new version** and adjust default values such as FailurePercentage.  
4. Use the updated version in a new AWS FIS experiment (via CLI or console).

**IAM Permissions:**  
You need permissions to create IAM roles and policies when deploying the CloudFormation template. When deploying via AWS Console, confirm that the template creates IAM resources. If using AWS CLI, add the <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;"> --capabilities CAPABILITY_NAMED_IAM</span> flag.

**Download the template:** You can download the [CloudFormation template here](https://d2908q01vomqb2.cloudfront.net/artifacts/MTBlog/cloudops-1899/fis_template.yaml) and save it as <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">fis_template.yaml</span> before deploying via AWS Console or CLI.

```Bash
aws cloudformation create-stack --stack-name alb-fis-experiment \
		--template-body file://fis_template.yaml \
		--parameters \
			ParameterKey=LoadBalancerName,ParameterValue=LoadBalancerName \
			ParameterKey=ListenerRuleArn,ParameterValue=RuleARN \
			ParameterKey=TestDurationInSeconds,ParameterValue=60 \
		--capabilities CAPABILITY_NAMED_IAM
```
<span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">LoadBalancerName</span> and <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">RuleARN</span> correspond to the Load Balancer name and full ARN of the listener rule before the service you want to simulate errors for. The value 60 specifies the experiment duration in seconds.

Note: The <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">FISExperimentRole</span> IAM policy uses <span style="color: red; background-color: #f2f2f2; padding: 2px 4px; border-radius: 3px;">"Resource": "*"</span> for some actions so AWS FIS can dynamically modify load balancer resources. Since target group resource ARNs are not known before deployment, restricting permissions to specific resources is not feasible in this example. While this approach provides flexibility, [AWS security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) recommend limiting permissions to specific resources if you know which resources will be used. If you know exactly which resources will be used, consider updating the policy to restrict access accordingly.

## **Step 2: Verify the Lambda Function**

After deployment, check the Lambda function in the AWS console to confirm it returns the expected error response. The function should return:

```JSON
 {
	 "statusCode": 503,
	 "body": "Service Unavailable - Simulated Error Response"
 }
```
## **Step 3: Start the AWS FIS Experiment**

1. Open the **AWS Fault Injection Service** console.
2. Find the preconfigured template in **Experiment Templates**.
3. Select **Start experiment**.
4. Confirm and launch the experiment.

![fis-console](/images/3-BlogsTranslated/3.1-Blog1/fis-console.png)
*Figure 3 shows the AWS FIS console with a custom experiment template created by the CloudFormation template.*

When you start the experiment, AWS FIS invokes the AWS Systems Manager Automation Document created during deployment. This document performs the following actions:

1. **Creates a new ALB target group** pointing to the Lambda function configured to return simulated error responses.
2. **Modifies the ALB listener rule** to split a portion of traffic to the new target group, simulating partial failure.
3. **Waits for the configured duration** (set via the CloudFormation template).
4. **Reverts the ALB listener rule** to its original state and deletes the temporary target group.

The entire lifecycle is automated—you don’t need to write code or manually update the load balancer. Just start the experiment from the FIS console and observe how your service responds to controlled failure scenarios.

In the screenshot below, you’ll see the initial ALB listener rule with only the default target group configured.

![before](/images/3-BlogsTranslated/3.1-Blog1/before.png)

*Figure 4 shows the ALB listener rule before the experiment starts, with one target group receiving 100% of traffic.*

After the experiment starts, AWS FIS modifies the rule to split traffic—as shown in the next screenshot.

![after](/images/3-BlogsTranslated/3.1-Blog1/after.png)

*Figure 5 shows the ALB listener rule after the experiment starts, with the new Lambda target group receiving 50% of traffic and returning the configured error response.*

## **Step 4: Observe and Analyze Results**

You can verify the experiment by refreshing the ALB DNS in your browser or running a curl loop:

```Bash
while true; do curl -s http://<your-alb-dns-name>; sleep 1; done
```
*![curl-test](/images/3-BlogsTranslated/3.1-Blog1/curl-test.gif) Figure 6 shows CLI output sending continuous requests to the ALB URL to demonstrate fault injection during the experiment.*

You’ll see alternating output between:

* Backend service is healthy (backend service)
* Service Unavailable – Simulated Error Response (Lambda)

You can monitor **Amazon CloudWatch Logs** for Lambda metrics and **Application behavior** (retry, failover, etc.).  
**Note:** After starting the experiment, it may take up to a minute for the new target group to attach to the ALB and for traffic to begin routing to Lambda. During this brief period, all requests may still reach the original backend service.

# **Rollback Mechanism**

The experiment’s rollback mechanism:

* ALB listener rule is automatically reverted when the experiment duration ends.
* Temporary target group is removed and deleted to avoid lingering configuration.
* If the experiment is canceled, the rollback process still returns the system to its original state.

# **Considerations**

This article provides technical information and example configuration. Real-world deployments may require additional consideration for security, compliance, and technical factors. Always test thoroughly in non-production environments first.

# **Cleanup**

To avoid ongoing costs, delete deployed resources:

```Bash
aws cloudformation delete-stack --stack-name alb-fis-experiment
```
# **Conclusion**

In this article, we demonstrated how to extend AWS FIS capabilities by simulating partial failures for workloads behind ALB using Lambda. This solution lets teams test application fault tolerance against disruptions without causing a full outage. By leveraging AWS FIS, Lambda, and ALB routing rules, you can create controlled failure scenarios and improve system resilience.

To learn more, see:

* [AWS Fault Injection Service Documentation](https://docs.aws.amazon.com/fis/latest/userguide/)  
* [Amazon ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)  
* [Use Systems Manager SSM documents with AWS FIS](https://docs.aws.amazon.com/fis/latest/userguide/actions-ssm-agent.html)

Get started with the [CloudFormation template](https://d2908q01vomqb2.cloudfront.net/artifacts/MTBlog/cloudops-1899/fis_template.yaml) and share your experience in the comments below.

TAGS: [aws fault injection simulator](https://aws.amazon.com/blogs/mt/tag/aws-fault-injection-simulator/), [chaos engineering](https://aws.amazon.com/blogs/mt/tag/chaos-engineering/)

</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.1-Blog1/author-canibeya.jpeg" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Ozgur Canibeyaz** Ozgur is a Senior Technical Account Manager at Amazon Web Services with 8 years of experience. Ozgur helps customers optimize their AWS usage by solving technical challenges, discovering cost-saving opportunities, achieving operational excellence, and building innovative services with AWS products.

</td>
</tr>
</table>

</td>
</tr>
</table>

<table>
<tr>
<td style="width: 120px; vertical-align: top;">
<img src="/images/3-BlogsTranslated/3.1-Blog1/author-pablo.png" alt="Dave Jaskie" style="width: 100px; border-radius: 5px;">
</td>
<td style="padding-left: 20px; vertical-align: top;">

**Pablo Colazurdo** Pablo is a Principal Solutions Architect at AWS where he enjoys helping customers launch successful projects on the Cloud. He has many years of experience working with diverse technologies and is always passionate about learning new things. Pablo grew up in Argentina but now enjoys the rain in Ireland while listening to music, reading books, or playing D&D with his kids.

