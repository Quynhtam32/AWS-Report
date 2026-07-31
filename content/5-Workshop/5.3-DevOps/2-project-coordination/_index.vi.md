---
title: "Project Coordination"
date: 2026-07-31
weight: 2
chapter: false
pre: "<b>2. </b>"
---

# Project Coordination

## 1. Muc tieu

Quan ly tien do, phan cong trach nhiem va theo doi dependency giua cac module de tranh tinh trang tung thanh vien hoan thanh phan rieng nhung he thong khong the tich hop.

## 2. GitHub Task Management

Cac task duoc chia theo cac track:

```text
Ingestion
Data
Machine Learning
Backend
DevOps
Integration
```

Moi task gom:

- Task name
- Owner
- Primary owner
- Goal
- Priority
- Checklist
- Prerequisite
- Evidence
- Definition of Done
- Next task

Trang thai duoc thong nhat:

```text
Todo
In Progress
Blocked
Review
Done
```

Task chi duoc chuyen sang `Done` khi co output thuc te va evidence di kem. Neu task bi chan, can ghi ro dang blocked boi thanh vien nao, resource nao, hoac phu thuoc vao dau ra nao.

## 3. Dependency & Integration Coordination

Luong phu thuoc chinh cua du an:

```text
Simulator
-> S3 Raw
-> S3 Processed
-> ML Dataset
-> Forecast Output
-> Backend
-> SNS
```

### 3.1. Ingestion -> Data

Hai nhom can thong nhat:

- MQTT topic
- Telemetry payload
- Telemetry schema v1
- Timestamp UTC
- Station ID
- Ten field
- Kieu du lieu
- Don vi do

### 3.2. Data -> Machine Learning

Hai nhom can thong nhat:

- S3 URI dau vao
- Cau truc dataset processed
- Tan suat du lieu theo gio
- Quy tac xu ly missing value
- Quy tac xu ly duplicate
- Partition convention
- Khoang thoi gian va danh sach station

### 3.3. Machine Learning -> Backend

Hai nhom can thong nhat:

- Model input
- Forecast output
- Forecast horizon
- Station ID
- Error response
- Nguong canh bao
- Cach backend doc forecast artifact hoac goi model

## 4. Quy trinh bao cao tien do

Khi cap nhat task, thanh vien can tra loi:

```text
Dang lam task nao?
Da hoan thanh phan nao?
Output hien tai la gi?
Evidence nam o dau?
Dang bi blocked boi ai hoac resource nao?
Task tiep theo la gi?
Co tao them AWS resource khong?
Co phat sinh chi phi khong?
```

## 5. Documentation Coordination

Moi thanh vien tu viet tai lieu cho module minh phu trach. Vai tro DevOps se:

- chot format chung
- kiem tra ten resource va region
- kiem tra su thong nhat giua input va output cua cac nhom
- tong hop evidence
- ra soat noi dung truoc khi dua vao bao cao va demo

Tai lieu duoc tong hop theo nhom noi dung:

- architecture overview
- telemetry schema
- processed dataset definition
- test plan
- demo script
- evidence checklist

## 6. Definition of Done chung

Mot task chi duoc nghiem thu khi:

- Code hoac AWS resource hoat dong
- Co test thuc te
- Co expected output
- Co log, screenshot, S3 object hoac API response
- Co huong dan chay lai
- Khong de lo credential
- Tai lieu lien quan da duoc cap nhat

## 7. Ket qua dat duoc

- Task duoc phan chia theo track va dependency
- Thanh vien co owner va trach nhiem cu the
- Cac diem ban giao giua nhom duoc xac dinh ro
- Task bi blocked duoc phat hien som
- Tai lieu va evidence duoc chuan hoa truoc khi tong hop
