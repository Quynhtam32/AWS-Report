---
title: "Blogs Posted"
date: 2026-07-31
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

### [Blog 1 - Can Increasing AWS Lambda Memory Reduce Costs? Why?](3.1-Blog1/)

This blog explains why increasing AWS Lambda memory does not always increase costs and, in some cases, can even reduce them. It discusses the relationship between memory allocation, CPU resources, execution time, and GB-second pricing. The article also introduces AWS Lambda Power Tuning and AWS Compute Optimizer, along with practical examples and best practices for selecting the optimal memory configuration based on workload characteristics.

### [Blog 2 - Why REST API Is Not Always the Best Choice? The Power of Pub/Sub and MQTT in Distributed Systems](3.2-Blog2/)

This blog compares the traditional REST API communication model with the Publish/Subscribe (Pub/Sub) architecture using MQTT. It explains the advantages of event-driven systems for IoT and distributed applications, introduces AWS services such as AWS IoT Core, Amazon SNS, Amazon EventBridge, Amazon Kinesis Data Firehose, and AWS Lambda, and demonstrates how these services work together to build scalable, loosely coupled cloud-native systems.

### [Blog 3 - An AWS IAM Mistake That Taught Me: Having Permissions Doesn't Always Mean a Service Can Use the Role](3.3-Blog3/)

This blog shares a practical lesson learned while working with AWS Identity and Access Management (IAM). It explains why an IAM Role with the correct permissions may still fail when used by an AWS service, highlighting the importance of Trust Policies, Permission Policies, and the `iam:PassRole` permission. The article also provides a troubleshooting checklist for diagnosing IAM role issues and introduces AWS security best practices such as the principle of least privilege and the use of temporary credentials to build more secure cloud-native applications.