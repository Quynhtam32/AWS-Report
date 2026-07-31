---
title: "Blog 1"
date: 2026-07-31
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tăng Memory cho AWS Lambda có thể giúp giảm chi phí – Tại sao?

Khi cấu hình AWS Lambda, nhiều người thường cho rằng lựa chọn mức memory thấp nhất sẽ giúp tiết kiệm chi phí. Tuy nhiên, chi phí của AWS Lambda không chỉ phụ thuộc vào lượng memory được cấp mà còn phụ thuộc vào thời gian thực thi của function. Khi tăng memory, AWS đồng thời cấp thêm CPU và các tài nguyên xử lý khác, giúp function có thể hoàn thành công việc nhanh hơn.

Bài viết này phân tích mối quan hệ giữa **memory, CPU, thời gian thực thi và chi phí tính theo GB-second** của AWS Lambda. Đồng thời giải thích vì sao trong nhiều trường hợp, một function được cấu hình memory cao hơn lại có thể hoàn thành nhanh hơn và thậm chí có tổng chi phí thấp hơn so với cấu hình memory thấp.

## Nội dung chính

* Tìm hiểu cách AWS Lambda phân bổ CPU dựa trên mức memory được cấu hình.
* Giải thích cơ chế tính chi phí của AWS Lambda theo mô hình GB-second.
* Phân tích lý do vì sao tăng memory có thể giúp giảm thời gian thực thi và tối ưu chi phí.
* Các loại workload hưởng lợi nhiều nhất khi tăng memory (CPU-bound, memory-bound và xử lý dữ liệu).
* Những trường hợp tăng memory không mang lại hiệu quả rõ rệt.
* Các nguyên tắc lựa chọn mức memory phù hợp cho từng loại ứng dụng.
* Giới thiệu công cụ **AWS Lambda Power Tuning** để kiểm thử nhiều mức memory khác nhau.
* Giới thiệu **AWS Compute Optimizer** giúp đưa ra khuyến nghị tối ưu memory dựa trên dữ liệu sử dụng thực tế.
* Ví dụ thực tế về tối ưu Lambda Function xử lý dữ liệu AQI.

## Kết luận

Qua bài viết, người đọc có thể thấy rằng mức memory tối ưu không phải lúc nào cũng là mức thấp nhất. Thay vào đó, cần kiểm thử với dữ liệu thực tế để lựa chọn cấu hình mang lại sự cân bằng tốt nhất giữa **hiệu năng** và **chi phí**. Việc tận dụng các công cụ như AWS Lambda Power Tuning và AWS Compute Optimizer cũng giúp quá trình tối ưu Lambda trở nên dễ dàng và chính xác hơn.

## Link bài viết

Facebook (AWS Study Group FCJ):  
https://www.facebook.com/groups/660548818043427/?multi_permalinks=2228234364608190&ref=share

## Tài liệu tham khảo

* AWS Lambda – Cấu hình Memory cho Function  
  https://docs.aws.amazon.com/lambda/latest/dg/configuration-memory.html

* AWS Lambda – Các Best Practices  
  https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

* AWS Lambda Pricing  
  https://aws.amazon.com/lambda/pricing/

* AWS Compute Optimizer – Khuyến nghị tối ưu cho AWS Lambda  
  https://docs.aws.amazon.com/compute-optimizer/latest/ug/view-lambda-recommendations.html

* AWS Lambda Power Tuning (GitHub)  
  https://github.com/alexcasalboni/aws-lambda-power-tuning

* Building Well-Architected Serverless Applications – Optimizing Application Costs  
  https://aws.amazon.com/blogs/compute/building-well-architected-serverless-applications-optimizing-application-costs/

* Troubleshoot AWS Lambda Functions  
  https://docs.aws.amazon.com/lambda/latest/dg/troubleshooting-execution.html

* AWS Lambda Runtime Environment  
  https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtime-environment.html