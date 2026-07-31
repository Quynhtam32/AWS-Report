---
title: "Worklog Tuần 8"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Hoàn thiện Data Lake và rà soát toàn bộ Storage/Data Pipeline.
* Đảm bảo dữ liệu có tính nhất quán và sẵn sàng cho buổi demo cuối kỳ.
* Hoàn thiện tài liệu và hỗ trợ nhóm chuẩn bị báo cáo, trình bày dự án.

### Công việc đã hoàn thành trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát tổng thể kiến trúc **Amazon S3 Data Lake** và kiểm tra tính nhất quán giữa các vùng lưu trữ **raw**, **processed**, **ml**, **models** và **monitoring**. | 20/07/2026 | 20/07/2026 | Tài liệu kiến trúc nội bộ |
| 2 | Kiểm định bộ dữ liệu đã xử lý phục vụ Machine Learning, bao gồm tính nhất quán của schema, định dạng timestamp và dữ liệu đầu ra Parquet trước khi huấn luyện mô hình. | 21/07/2026 | 21/07/2026 | Tài liệu xử lý dữ liệu |
| 3 | Hoàn thiện tài liệu về **Storage** và **Data Pipeline**, bao gồm cấu trúc S3 Data Lake, quy trình Kinesis Data Firehose, kiểm định dữ liệu thô, tiền xử lý dữ liệu và các giải pháp tối ưu chi phí lưu trữ. | 22/07/2026 | 22/07/2026 | Tài liệu dự án |
| 4 | Hỗ trợ nhóm chuẩn bị báo cáo cuối kỳ và slide thuyết trình. Rà soát phần **Storage/Data Pipeline** và đảm bảo toàn bộ tài liệu kỹ thuật đầy đủ, thống nhất. | 23/07/2026 | 23/07/2026 | Biên bản họp nhóm |
| 5 | Tham gia kiểm tra tích hợp toàn bộ hệ thống và chuẩn bị cho buổi demo cuối kỳ. Xác minh luồng dữ liệu hoàn chỉnh từ **AWS IoT Core → Kinesis Data Firehose → Amazon S3 → Data Processing → Machine Learning** nhằm đảm bảo hệ thống hoạt động ổn định trong phần trình bày cuối cùng. | 24/07/2026 | 24/07/2026 | Tài liệu Capstone Project |

### Kiến thức và kinh nghiệm đạt được:

* Hiểu được tầm quan trọng của việc rà soát và hoàn thiện tài liệu trước khi triển khai hệ thống.

* Hiểu rõ quy trình xây dựng Data Lake hoàn chỉnh:

  * Thu thập dữ liệu.
  * Lưu trữ dữ liệu.
  * Xử lý dữ liệu.
  * Tích hợp Machine Learning.
  * Trình diễn ứng dụng.

* Có thêm kinh nghiệm phối hợp với các nhóm khác để hoàn thiện một hệ thống dựa trên AWS.

* Nâng cao hiểu biết về cách thiết kế và vận hành Data Pipeline trên môi trường cloud trong dự án thực tế.