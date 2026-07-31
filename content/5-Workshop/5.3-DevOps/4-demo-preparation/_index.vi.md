---
title: "Demo Preparation"
date: 2026-07-31
weight: 4
chapter: false
pre: "<b>4. </b>"
---

# Demo Preparation

## 1. Mục tiêu

Chuẩn bị một kịch bản demo ngắn gọn nhưng chứng minh được hệ thống hoạt động xuyên suốt từ dữ liệu đầu vào đến dự báo và cảnh báo.

## 2. Nguyên tắc trình bày

Mỗi phần demo phải có:

```text
Input
Hành động
Output
Bằng chứng
```

Không chỉ mở giao diện AWS và mô tả cấu hình.

## 3. Thứ tự demo

1. Giới thiệu bài toán và phạm vi MVP.
2. Trình bày sơ đồ kiến trúc.
3. Chạy Simulator với nhiều station.
4. Xác minh message trong MQTT Test Client.
5. Kiểm tra Firehose và S3 Raw.
6. Chạy Data Processing và tạo Parquet.
7. Trình bày SageMaker Training và forecast.
8. Test Backend API.
9. Kích hoạt SNS Alert.
10. Trình bày Monitoring và AWS Budget.

## 4. Phân công trình bày

```text
Kiến trúc và DevOps        → M5
Simulator và IoT Core      → Ingestion Engineer
S3 và Data Pipeline        → Data Engineer
SageMaker và Forecast      → ML Engineer
Backend và SNS             → Backend Engineer
Kết luận                   → Project Lead
```

{{% notice info %}}
📸 **Ảnh cần chụp 01 — Checklist hoặc phân công demo**

Chụp bảng phân công hoặc checklist tổng duyệt.

**Tên ảnh:** `demo-assignment-checklist.png`

**Codex chèn ảnh sau phần Phân công trình bày.**
{{% /notice %}}

## 5. Checklist trước khi quay

```markdown
- [ ] Đăng nhập đúng AWS Account.
- [ ] Region là ap-southeast-1.
- [ ] Simulator chạy thành công.
- [ ] MQTT Test Client subscribe đúng topic.
- [ ] Firehose ở trạng thái Active.
- [ ] S3 Raw có object mới.
- [ ] Data Processing chạy thành công.
- [ ] S3 Processed có Parquet.
- [ ] SageMaker Training Job đã Completed.
- [ ] Forecast output đã sẵn sàng.
- [ ] Backend API hoạt động.
- [ ] SNS subscription đã Confirmed.
- [ ] Email cảnh báo đã test.
- [ ] CloudWatch có log.
- [ ] AWS Budget mở được.
- [ ] Không để lộ credential.
```

## 6. Phương án dự phòng

Chuẩn bị:

- Screenshot Training Job `Completed`.
- Forecast JSON hoặc CSV.
- Video ngắn của ingestion flow.
- S3 object mẫu.
- API response mẫu.
- Email SNS đã nhận.
- Sơ đồ kiến trúc offline.
- File log dùng làm evidence.

Khi dùng evidence dự phòng cần nói rõ đó là kết quả của lần test trước.

## 7. Evidence bắt buộc

```text
Sơ đồ kiến trúc
GitHub Project Board
Simulator output
MQTT Test Client
Firehose Monitoring
S3 Raw object
S3 Processed Parquet
SageMaker Training Job
Forecast output
API response
SNS email
CloudWatch Logs
AWS Budget
```

{{% notice info %}}
📸 **Ảnh cần chụp 02 — Flow demo tổng quát**

Dùng sơ đồ kiến trúc đã đánh số hoặc hình tổng hợp các bước demo.

**Tên ảnh:** `demo-flow-overview.png`

**Codex chèn ảnh sau phần Evidence bắt buộc.**
{{% /notice %}}

## 8. Kết quả mong đợi

- Dữ liệu được gửi từ nhiều station.
- IoT Core nhận được message.
- Firehose ghi dữ liệu xuống S3.
- Pipeline tạo dữ liệu processed.
- ML tạo forecast.
- Backend cung cấp kết quả.
- SNS gửi cảnh báo.
- Hệ thống có monitoring và kiểm soát chi phí.
