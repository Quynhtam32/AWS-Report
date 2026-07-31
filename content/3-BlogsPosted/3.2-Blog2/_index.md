---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Why REST APIs Are Not Always the Best Choice: The Power of Pub/Sub and MQTT in Distributed Systems

This blog discusses the limitations of traditional REST APIs when building distributed and real-time systems. It introduces the Publish/Subscribe (Pub/Sub) communication model using MQTT and explains how event-driven architecture on AWS improves scalability, flexibility, and system decoupling.

Key points covered:

* The limitations of synchronous HTTP/REST communication in large-scale distributed systems.
* The Publish/Subscribe communication model and the role of MQTT Broker.
* Advantages of MQTT for IoT applications, including lightweight communication and asynchronous message delivery.
* AWS services that support event-driven architecture:
  * AWS IoT Core
  * Amazon SNS
  * Amazon EventBridge
* A practical IoT data pipeline using:
  * MQTT Gateway
  * AWS IoT Core
  * IoT Rules Engine
  * Amazon Kinesis Data Firehose
  * Amazon S3
  * AWS Lambda
* The benefits of decoupling services to improve scalability, reliability, and maintainability.

This article also compares REST APIs and Pub/Sub architecture, helping readers understand when an event-driven approach is more suitable for cloud-native and IoT systems.

### Blog Link

https://www.facebook.com/groups/660548818043427/?multi_permalinks=2228776647887295&ref=share

### Reference Materials

* AWS IoT Core Developer Guide  
  https://docs.aws.amazon.com/iot/latest/developerguide/

* AWS IoT Core Rules Engine  
  https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html

* MQTT Version 5.0 Specification  
  https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html

* Amazon EventBridge Documentation  
  https://docs.aws.amazon.com/eventbridge/

* Amazon SNS Documentation  
  https://docs.aws.amazon.com/sns/

* Amazon Kinesis Data Firehose Documentation  
  https://docs.aws.amazon.com/firehose/

* AWS Well-Architected Framework – Event-Driven Architecture Best Practices  
  https://docs.aws.amazon.com/wellarchitected/

