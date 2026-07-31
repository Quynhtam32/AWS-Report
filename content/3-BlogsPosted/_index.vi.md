---
title: "Các bài Blog đã đăng"
date: 2026-07-31
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

### [Blog 1 - Tăng Memory cho AWS Lambda có thể giúp giảm chi phí – Tại sao?](3.1-Blog1/)

Bài viết phân tích lý do vì sao việc tăng memory cho AWS Lambda không phải lúc nào cũng làm tăng chi phí, mà trong nhiều trường hợp còn giúp giảm chi phí nhờ rút ngắn thời gian thực thi. Nội dung trình bày mối quan hệ giữa memory, CPU, thời gian chạy và GB-second, đồng thời giới thiệu các công cụ như AWS Lambda Power Tuning và AWS Compute Optimizer cùng những ví dụ thực tế để lựa chọn cấu hình memory phù hợp với từng workload.

### [Blog 2 - Tại sao REST API không phải lúc nào cũng tốt? Sức mạnh của Pub/Sub và MQTT trong hệ thống phân tán](3.2-Blog2/)

Bài viết so sánh mô hình giao tiếp truyền thống REST API với mô hình Publish/Subscribe (Pub/Sub) sử dụng giao thức MQTT. Nội dung giải thích những ưu điểm của kiến trúc Event-Driven trong các hệ thống IoT và hệ thống phân tán, đồng thời giới thiệu cách kết hợp các dịch vụ AWS như AWS IoT Core, Amazon SNS, Amazon EventBridge, Amazon Kinesis Data Firehose và AWS Lambda để xây dựng các hệ thống cloud-native có khả năng mở rộng và hoạt động linh hoạt.

### [Blog 3 - Một lỗi AWS IAM khiến mình hiểu rằng: "Role có quyền" chưa chắc dịch vụ đã dùng được](3.3-Blog3/)

Bài viết chia sẻ một kinh nghiệm thực tế khi làm việc với AWS Identity and Access Management (IAM), giải thích vì sao một IAM Role dù đã được cấp đầy đủ quyền vẫn có thể không hoạt động nếu cấu hình chưa chính xác. Nội dung tập trung làm rõ sự khác biệt giữa Trust Policy, Permission Policy và quyền `iam:PassRole`, đồng thời cung cấp checklist kiểm tra khi dịch vụ AWS không thể sử dụng IAM Role. Bài viết cũng giới thiệu các thực hành bảo mật theo khuyến nghị của AWS như nguyên tắc **Least Privilege** và sử dụng IAM Roles thay cho Access Key dài hạn.