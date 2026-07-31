---
title: "Worklog Tuần 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Bắt đầu triển khai phần Storage/Data trong kiến trúc hệ thống.
* Tìm hiểu các tính năng nâng cao của Amazon S3 và áp dụng để xây dựng Data Lake.
* Thiết kế schema dữ liệu AQI và chuẩn bị quy trình xử lý dữ liệu phục vụ Machine Learning.

### Công việc đã hoàn thành trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Nghiên cứu các kiến thức nâng cao về Amazon S3, bao gồm kiến trúc bucket, cách tổ chức object, các lớp lưu trữ (Storage Class) và các thực tiễn tốt trong quản lý dữ liệu. | 22/06/2026 | 22/06/2026 | Tài liệu Amazon S3 |
| 2 | Tìm hiểu kiến trúc Data Lake trên AWS và thiết kế cấu trúc thư mục S3 cho các vùng dữ liệu Raw, Processed, ML, Models và Monitoring. | 23/06/2026 | 23/06/2026 | AWS Well-Architected Framework, Tài liệu Amazon S3 |
| 3 | Thực hành Amazon S3 Versioning và Lifecycle Policy nhằm tìm hiểu cơ chế bảo vệ dữ liệu và tối ưu chi phí lưu trữ. | 24/06/2026 | 24/06/2026 | Tài liệu Amazon S3 Versioning & Lifecycle |
| 4 | Thiết kế schema dữ liệu ban đầu cho hệ thống dự báo chất lượng không khí (AQI), bao gồm thông tin thiết bị, thời gian, PM2.5, PM10, nhiệt độ, độ ẩm và các thuộc tính cần thiết của dự án. | 25/06/2026 | 25/06/2026 | Tài liệu thiết kế hệ thống |
| 5 | Tìm hiểu Amazon SageMaker Processing Job và thực hiện thử nghiệm đọc, xử lý bộ dữ liệu mẫu được lưu trữ trên Amazon S3. | 26/06/2026 | 26/06/2026 | Tài liệu Amazon SageMaker |

### Kiến thức và kinh nghiệm đạt được:

* Hiểu được vai trò của Amazon S3 trong việc xây dựng Data Lake.

* Hiểu được lợi ích của việc phân chia dữ liệu thành các zone:
  * Dễ quản lý dữ liệu.
  * Hỗ trợ các bước xử lý tiếp theo.
  * Giúp duy trì pipeline Machine Learning hiệu quả hơn.

* Có thêm kinh nghiệm trong việc chuẩn bị và chuẩn hóa dữ liệu trước khi đưa vào mô hình Machine Learning.