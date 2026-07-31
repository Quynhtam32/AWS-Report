---
title: "Worklog Tuần 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Triển khai quy trình xử lý và làm sạch dữ liệu AQI được thu thập từ OpenAQ.
* Tìm hiểu cách sử dụng AWS để thực hiện các bước tiền xử lý dữ liệu trước khi đưa vào Machine Learning.
* Khám phá Amazon Kinesis Data Firehose để thu thập dữ liệu streaming và lưu trữ vào Amazon S3.

### Công việc đã hoàn thành trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Xem xét cấu trúc dữ liệu thô từ OpenAQ, phân tích định dạng dữ liệu và xác định các giá trị thiếu, bản ghi trùng lặp và các giá trị bất thường. | 29/06/2026 | 29/06/2026 | Bộ dữ liệu OpenAQ, Tài liệu dự án |
| 2 | Tìm hiểu quy trình tiền xử lý dữ liệu trên AWS và nghiên cứu Amazon SageMaker Processing Job để thực hiện các tác vụ làm sạch dữ liệu. | 30/06/2026 | 30/06/2026 | Tài liệu Amazon SageMaker |
| 3 | Xây dựng chương trình tiền xử lý dữ liệu AQI nhằm xử lý dữ liệu thiếu, phát hiện giá trị ngoại lai và chuẩn bị dữ liệu cho các bước xử lý tiếp theo. | 01/07/2026 | 01/07/2026 | Tài liệu dự án |
| 4 | Tải bộ dữ liệu đã làm sạch lên vùng **processed** của Amazon S3 và kiểm tra tính nhất quán của dữ liệu theo schema AQI đã thiết kế. | 02/07/2026 | 02/07/2026 | Tài liệu Amazon S3, Tài liệu thiết kế hệ thống |
| 5 | Nghiên cứu Amazon Kinesis Data Firehose, bao gồm quy trình thu nhận dữ liệu dạng streaming và tích hợp với Amazon S3 Data Lake. | 03/07/2026 | 03/07/2026 | Tài liệu Amazon Kinesis Data Firehose |
| 6 | Tạo luồng Kinesis Data Firehose thử nghiệm, gửi dữ liệu streaming mẫu và xác nhận dữ liệu được ghi thành công vào Amazon S3. | 04/07/2026 | 04/07/2026 | Tài liệu Amazon Kinesis Data Firehose |

### Kiến thức và kinh nghiệm đạt được:

* Hiểu được tầm quan trọng của bước tiền xử lý dữ liệu trong quy trình Machine Learning.

* Hiểu cách kết hợp các dịch vụ AWS để xây dựng Data Pipeline:

  * Amazon S3 dùng để lưu trữ dữ liệu.
  * SageMaker Processing Job dùng để xử lý và biến đổi dữ liệu.
  * Kinesis Data Firehose dùng để thu thập dữ liệu streaming.

* Có thêm kinh nghiệm trong việc kiểm tra và duy trì tính nhất quán của dữ liệu giữa các thành phần trong hệ thống.

* Hiểu rõ hơn quá trình biến đổi dữ liệu raw thành dataset có chất lượng cao phục vụ cho việc phân tích và dự báo.