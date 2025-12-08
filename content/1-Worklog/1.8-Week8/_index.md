---
title: "Week 8 Worklog"
date: "2025-10-27"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Analyze requirements and design the architecture for the Serverless Student Management system.
* Design API endpoints for the Class Management module.
* Design DynamoDB table structures.

### Tasks to be implemented this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Analyze project requirements: <br>&emsp; + Identify actors: Lecturer, Student <br>&emsp; + List main use cases <br>&emsp; + Draw Use Case Diagram | 27/10/2025 | 27/10/2025 | [Proposal Document](../../2-Proposal/) |
| 3   | - Design system architecture: <br>&emsp; + Draw Architecture Diagram <br>&emsp; + Identify AWS services used <br>&emsp; + Design data flow | 28/10/2025 | 28/10/2025 | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) |
| 4   | - Design API endpoints for Class Management: <br>&emsp; + `POST /lecturer/classes` - Create Class <br>&emsp; + `GET /lecturer/classes` - List Classes <br>&emsp; + `PUT /lecturer/classes/{id}` - Edit Class <br>&emsp; + `DELETE /lecturer/classes/{id}` - Deactivate Class | 29/10/2025 | 29/10/2025 | [REST API Best Practices](https://docs.aws.amazon.com/apigateway/latest/developerguide/rest-api-develop.html) |
| 5   | - Design DynamoDB Tables: <br>&emsp; + Table `Classes`: PK, SK, GSI design <br>&emsp; + Table `Students`: Schema and indexes <br>&emsp; + Single-table design patterns | 30/10/2025 | 30/10/2025 | [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html) |
| 6   | - Design API Request/Response: <br>&emsp; + Define JSON schemas <br>&emsp; + Error handling patterns <br>&emsp; + Validation rules | 31/10/2025 | 31/10/2025 | [API Gateway Request Validation](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-request-validation.html) |

### Week 8 Achievements:

* Completed requirements analysis:
  * Identified 2 main actors: Lecturer and Student
  * 15+ use cases for the system
  * Completed Use Case Diagram

* Designed Serverless architecture:
  * API Gateway + Lambda + DynamoDB
  * Cognito for authentication
  * AppSync for real-time features

* Designed Class Management APIs:
  * `POST /lecturer/classes` - Create new class (with quantity limit)
  * `GET /lecturer/classes` - List lecturer's classes
  * `PUT /lecturer/classes/{id}` - Update class information
  * `DELETE /lecturer/classes/{id}` - Soft delete (status: 1 → 0)

* Designed DynamoDB schema:
  * Single-table design for performance
  * GSI for query patterns
  * Status field for soft delete




