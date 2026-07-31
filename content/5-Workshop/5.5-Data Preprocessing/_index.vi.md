---
title: "Xây dựng Data Pipeline"
date : 2026-07-31
weight : 1
chapter : false
pre : " <b> 5.3. </b> "
---

Trong module này, chúng ta sẽ thực hành xây dựng một luồng dữ liệu (Data Pipeline) hoàn chỉnh trên AWS. 

Dữ liệu môi trường (PM2.5, nhiệt độ, độ ẩm) từ thiết bị IoT giả lập sẽ được thu thập, lưu trữ vào S3 Data Lake, sau đó đi qua bước kiểm định (Validation) và tiền xử lý (Processing) để sẵn sàng phục vụ cho mô hình Machine Learning ở các bước sau.

**Nội dung thực hành bao gồm:**
* **5.3.1:** Khởi tạo kho lưu trữ trung tâm S3 Data Lake.
* **5.3.2:** Thiết lập luồng Kinesis Data Firehose để gom lô dữ liệu.
* **5.3.3:** Viết kịch bản kiểm định chất lượng dữ liệu thô (Raw Data).
* **5.3.4:** Tiền xử lý, chuẩn hóa tần suất (1H) và xuất định dạng Parquet.