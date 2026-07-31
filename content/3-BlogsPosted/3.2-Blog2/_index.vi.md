---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Tại sao REST API không phải lúc nào cũng tốt? Sức mạnh của Pub/Sub và MQTT trong hệ thống phân tán

Bài viết này phân tích những hạn chế của REST API trong các hệ thống phân tán và thời gian thực, đồng thời giới thiệu mô hình Publish/Subscribe (Pub/Sub) sử dụng giao thức MQTT và cách kiến trúc hướng sự kiện (Event-Driven Architecture) trên AWS giúp hệ thống mở rộng linh hoạt hơn.

Các nội dung chính của bài viết:

* Phân tích những hạn chế của mô hình giao tiếp HTTP/REST đồng bộ trong các hệ thống quy mô lớn.
* Giới thiệu mô hình Publish/Subscribe và vai trò của MQTT Broker.
* Những ưu điểm của MQTT trong các hệ thống IoT như giao tiếp nhẹ, bất đồng bộ và tiết kiệm băng thông.
* Tìm hiểu các dịch vụ AWS hỗ trợ kiến trúc Event-Driven:
  * AWS IoT Core.
  * Amazon SNS.
  * Amazon EventBridge.
* Minh họa luồng dữ liệu IoT thực tế với:
  * MQTT Gateway.
  * AWS IoT Core.
  * IoT Rules Engine.
  * Amazon Kinesis Data Firehose.
  * Amazon S3.
  * AWS Lambda.
* Lợi ích của việc tách rời các thành phần trong hệ thống nhằm tăng khả năng mở rộng, độ tin cậy và dễ bảo trì.

Bài viết cũng so sánh giữa kiến trúc REST API truyền thống và mô hình Pub/Sub, giúp người đọc hiểu khi nào nên áp dụng kiến trúc hướng sự kiện trong các hệ thống Cloud Native và IoT.

### Link bài viết

https://www.facebook.com/groups/660548818043427/?multi_permalinks=2228776647887295&ref=share

### Tài liệu tham khảo

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

* AWS Well-Architected Framework – Kiến trúc hướng sự kiện  
  https://docs.aws.amazon.com/wellarchitected/