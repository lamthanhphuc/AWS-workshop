---
title: "Week 3 Worklog"
date: "2025-09-22"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Practice deploying a complete VPC with EC2 instances.
* Understand how to connect EC2s in different subnets.
* Practice EC2 Instance Connect Endpoint.

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - **Practice Lab03-03:** Create VPC from scratch <br>&emsp; + Create VPC <br>&emsp; + Create Subnets (Public/Private) <br>&emsp; + Create Internet Gateway <br>&emsp; + Create Route Table for Outbound Internet Routing | 22/09/2025 | 22/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 3   | - **Practice Lab03-03 (cont.):** <br>&emsp; + Create Security Groups <br>&emsp; + Configure Inbound/Outbound rules | 23/09/2025 | 23/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 4   | - **Practice Lab03-04:** EC2 Instances in Subnets <br>&emsp; + Create EC2 Instances in Public Subnet <br>&emsp; + Create EC2 Instances in Private Subnet <br>&emsp; + Test connectivity between instances | 24/09/2025 | 24/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 5   | - **Practice Lab03-04 (cont.):** <br>&emsp; + Create NAT Gateway <br>&emsp; + Configure Route Table for Private Subnet <br>&emsp; + Test Internet connectivity from Private Subnet | 25/09/2025 | 25/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |
| 6   | - **Practice:** EC2 Instance Connect Endpoint <br>&emsp; + Create EC2 Instance Connect Endpoint <br>&emsp; + Connect to Private EC2 without Bastion Host <br>&emsp; + Clean up resources | 26/09/2025 | 26/09/2025 | <https://www.youtube.com/@AWSStudyGroup> |

### Week 3 Achievements:

* Successfully practiced creating a complete VPC:
  * Created VPC with appropriate CIDR block
  * Created Public and Private Subnets
  * Configured Internet Gateway and NAT Gateway
  * Set up Route Tables for each subnet type

* Deployed EC2 instances in VPC:
  * Created EC2 in Public Subnet with Public IP
  * Created EC2 in Private Subnet
  * Tested SSH and Internet connectivity

* Used EC2 Instance Connect Endpoint:
  * Understood how to connect to Private EC2 without Bastion Host
  * Saved costs and increased security

* Learned how to clean up resources to avoid unnecessary costs:
  * Terminate EC2 instances
  * Delete NAT Gateway
  * Release Elastic IPs
  * Delete VPC and related components




