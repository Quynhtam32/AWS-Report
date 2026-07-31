---
title: "Worklog Tuần 7"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Kiểm tra khả năng truy vết dữ liệu (data lineage) và đảm bảo tính nhất quán của dữ liệu trong Data Pipeline.
* Tích hợp phần Storage/Data với các thành phần Machine Learning và Backend.
* Kiểm thử toàn bộ quy trình từ lưu trữ dữ liệu đến dịch vụ dự báo.

### Công việc đã hoàn thành trong tuần

| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | Rà soát toàn bộ luồng dữ liệu từ vùng **raw** đến vùng **processed** trên Amazon S3 và tài liệu hóa quy trình chuyển đổi dữ liệu của hệ thống. | 13/07/2026 | 13/07/2026 | Tài liệu thiết kế hệ thống |
| 2 | Kiểm tra **data lineage** bằng cách truy vết dữ liệu đã xử lý về nguồn dữ liệu ban đầu và xác nhận tính nhất quán của dữ liệu giữa các giai đoạn trong pipeline. | 14/07/2026 | 14/07/2026 | Tài liệu dự án |
| 3 | Phối hợp với nhóm Machine Learning để bảo đảm bộ dữ liệu đã xử lý đáp ứng yêu cầu cho quá trình huấn luyện, đánh giá và tái huấn luyện mô hình. | 15/07/2026 | 15/07/2026 | Biên bản họp nhóm |
| 4 | Rà soát quá trình tích hợp giữa Amazon S3 và Amazon SageMaker Endpoint, đồng thời kiểm tra luồng dữ liệu giữa dịch vụ lưu trữ, suy luận mô hình và hệ thống backend. | 16/07/2026 | 16/07/2026 | Tài liệu Amazon SageMaker |
| 5 | Thực hiện kiểm thử end-to-end cho toàn bộ quy trình từ Amazon S3 → SageMaker Endpoint → Backend API, xác định các vấn đề trong pipeline và xác nhận dữ liệu được xử lý chính xác. | 17/07/2026 | 17/07/2026 | Báo cáo kiểm thử hệ thống |

### Kiến thức và kinh nghiệm đạt được:

* Hiểu được vai trò của data lineage trong việc duy trì Data Pipeline đáng tin cậy.

* Biết cách kiểm tra và giám sát luồng dữ liệu giữa các dịch vụ AWS.

* Có thêm kinh nghiệm tích hợp hệ thống lưu trữ dữ liệu với các dịch vụ Machine Learning trên cloud.

* Hiểu rõ hơn quy trình hoàn chỉnh của một hệ thống dự báo:
  * Thu thập dữ liệu.
  * Lưu trữ dữ liệu.
  * Xử lý dữ liệu.
  * Dự đoán bằng Machine Learning.
  * Tích hợp với ứng dụng.