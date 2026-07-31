---
title: "Xây dựng Data Pipeline"
date: 2026-07-31
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Xây dựng Data Pipeline

Phần này trình bày quá trình xây dựng **Data Pipeline** cho dự án **Local AQI Forecasting & Alert System**.

Dữ liệu môi trường (PM2.5, nhiệt độ và độ ẩm) được tạo từ các thiết bị IoT mô phỏng sẽ được thu nhận, kiểm định và xử lý trước khi được lưu trữ trong **Amazon S3 Data Lake**. Sau đó, dữ liệu được chuyển đổi thành định dạng sẵn sàng cho Machine Learning để phục vụ mô hình dự báo ở các phần tiếp theo.

Nội dung được chia thành bốn phần:

1. **Khởi tạo Amazon S3 Data Lake**
2. **Cấu hình Amazon Kinesis Data Firehose**
3. **Kiểm định dữ liệu thô**
4. **Tiền xử lý và xuất bộ dữ liệu**

Cụ thể:

- **Khởi tạo Amazon S3 Data Lake** hướng dẫn cách tạo và cấu hình bucket Amazon S3 làm kho lưu trữ trung tâm cho dữ liệu thô và dữ liệu đã xử lý.
- **Cấu hình Amazon Kinesis Data Firehose** trình bày cách cấu hình Firehose để tiếp nhận dữ liệu luồng từ AWS IoT Core và tự động lưu vào Amazon S3.
- **Kiểm định dữ liệu thô** hướng dẫn quy trình kiểm tra nhằm đảm bảo dữ liệu cảm biến đầu vào đáp ứng đúng cấu trúc và các yêu cầu về chất lượng trước khi tiếp tục xử lý.
- **Tiền xử lý và xuất bộ dữ liệu** trình bày các bước làm sạch dữ liệu, chuẩn hóa tần suất theo giờ (1H), chuẩn bị đặc trưng và xuất dữ liệu sang định dạng Apache Parquet để phục vụ các bước Machine Learning tiếp theo.

### Điều hướng

- [5.5.1. Khởi tạo Amazon S3 Data Lake](5.5.1-S3-Data-Lake/)
- [5.5.2. Cấu hình Amazon Kinesis Data Firehose](5.5.2-Firehose/)
- [5.5.3. Kiểm định dữ liệu thô](5.5.3-Data_Validation/)
- [5.5.4. Tiền xử lý và xuất bộ dữ liệu](5.5.4-Data-Processing/)