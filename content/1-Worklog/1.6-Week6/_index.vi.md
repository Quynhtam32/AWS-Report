---
title: "Worklog Tuần 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Tối ưu cấu trúc lưu trữ dữ liệu trên Amazon S3 để cải thiện hiệu suất truy vấn.
* Chuẩn hóa dữ liệu time-series phục vụ cho quá trình huấn luyện mô hình Machine Learning.
* Tìm hiểu các phương pháp tối ưu chi phí lưu trữ dữ liệu trên AWS.

### Công việc đã hoàn thành trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Nghiên cứu các chiến lược phân vùng dữ liệu trên Amazon S3 và tìm hiểu cách tổ chức dữ liệu theo trạm quan trắc và thời gian nhằm tối ưu hiệu suất truy vấn. | 06/07/2026 | 06/07/2026 | Tài liệu Amazon S3, AWS Best Practices |
| 2 | Thiết kế lại cấu trúc phân vùng dữ liệu AQI bằng cách áp dụng phân vùng theo `station_id` và thời gian để quản lý dữ liệu hiệu quả hơn. | 07/07/2026 | 07/07/2026 | Tài liệu thiết kế hệ thống |
| 3 | Chuẩn hóa dữ liệu chuỗi thời gian (time-series) từ các trạm quan trắc và kiểm tra định dạng dữ liệu phục vụ cho quá trình huấn luyện mô hình Amazon SageMaker DeepAR. | 08/07/2026 | 08/07/2026 | Tài liệu Amazon SageMaker DeepAR |
| 4 | Tìm hiểu Amazon S3 Storage Classes và các chính sách Lifecycle nâng cao nhằm tối ưu chi phí lưu trữ thông qua việc tự động chuyển đổi dữ liệu giữa các lớp lưu trữ. | 09/07/2026 | 09/07/2026 | Tài liệu Amazon S3 Storage Classes |
| 5 | Cấu hình Lifecycle Policy trên Amazon S3 và thử nghiệm việc tự động chuyển dữ liệu giữa các lớp lưu trữ S3 Standard, S3 Standard-IA và S3 Glacier. | 10/07/2026 | 10/07/2026 | Tài liệu Amazon S3 Lifecycle |

### Kiến thức và kinh nghiệm đạt được:

* Hiểu được vai trò của việc tổ chức dữ liệu hợp lý trong Data Lake.

* Nhận thức được tầm quan trọng của partition trong việc:
  * Giảm thời gian truy vấn dữ liệu.
  * Tăng hiệu quả xử lý dữ liệu cho các hệ thống Machine Learning.
  * Dễ dàng quản lý dữ liệu theo từng nhóm và thời gian.

* Hiểu cách tối ưu chi phí lưu trữ trên AWS thông qua:
  * Lựa chọn Storage Class phù hợp.
  * Sử dụng Lifecycle Policy để tự động quản lý dữ liệu.

* Có thêm kinh nghiệm trong việc chuẩn bị dữ liệu time-series phục vụ cho bài toán dự báo chất lượng không khí.