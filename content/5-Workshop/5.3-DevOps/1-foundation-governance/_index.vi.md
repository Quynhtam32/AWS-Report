---
title: "Foundation & Governance"
date: 2026-07-31
weight: 3
chapter: false
pre: "<b>1. </b>"
---

# Foundation & Governance

## 1. Muc tieu

Thiet lap mot moi truong AWS thong nhat cho toan nhom, kiem soat chi phi phat sinh, va bao dam cac thanh vien trien khai tai nguyen theo cung kien truc, region va quy uoc quan ly.

## 2. Chuan hoa moi truong AWS

Nhom thong nhat trien khai toan bo tai nguyen tai:

```text
ap-southeast-1 - Asia Pacific (Singapore)
```

Cac yeu cau chung:

- Kiem tra dung AWS Account va region truoc khi tao resource.
- Khong tu y trien khai tai region khac.
- Bao lai ten resource, service va muc dich su dung sau khi tao.
- Su dung cung naming convention va tag convention.
- Dung AWS CLI profile hoac AWS Console dung tai khoan duoc cap.

## 3. Budget & Cost Monitoring

AWS Budgets duoc thiet lap de theo doi dong thoi **chi phi phat sinh** va **muc su dung tai nguyen** trong qua trinh trien khai du an.

Hien tai, nhom su dung hai budget chinh:

- `My Monthly Cost Budget`: theo doi tong chi phi AWS theo thang, voi han muc `100 USD`.
- `Daily usage`: theo doi muc su dung cua tai nguyen theo don vi gio, voi han muc `0.2 gio`.

Tai thoi diem kiem tra, tong chi phi phat sinh trong thang van o muc thap so voi han muc da thiet lap. Budget dang o trang thai `Healthy` va chua co nguong canh bao nao bi vuot qua.

### Cac nguong canh bao chi phi

Doi voi budget `My Monthly Cost Budget`, he thong duoc cau hinh cac nguong canh bao sau:

```text
12.5%  -> Canh bao khi chi phi thuc te vuot 12.50 USD
25%    -> Canh bao khi chi phi thuc te vuot 25.00 USD
50%    -> Canh bao khi chi phi thuc te vuot 50.00 USD
85%    -> Canh bao khi chi phi thuc te vuot 85.00 USD
90%    -> Canh bao khi chi phi thuc te vuot 90.00 USD
100%   -> Canh bao khi chi phi thuc te vuot 100.00 USD
100%   -> Canh bao khi chi phi du bao vuot 100.00 USD
```

Cac nguong nay giup nhom phat hien som viec su dung tai nguyen vuot ke hoach va co bien phap kiem tra truoc khi chi phi tang cao.

### Quy uoc kiem soat noi bo

Dua tren cac nguong AWS Budgets, nhom ap dung quy trinh kiem soat nhu sau:

```text
Tu 12.5 USD -> Kiem tra dich vu dang phat sinh chi phi
Tu 25 USD   -> Ra soat tai nguyen dang chay va owner phu trach
Tu 50 USD   -> Han che tao them tai nguyen khong can thiet
Tu 85 USD   -> Tam dung cac tai nguyen thu nghiem
Tu 90 USD   -> Chi duy tri tai nguyen can thiet cho MVP
Tu 100 USD  -> Dung va kiem tra toan bo tai nguyen tinh phi
```

### Cac dich vu can theo doi sat

Nhung dich vu co kha nang phat sinh chi phi dang ke gom:

* Amazon EC2.
* SageMaker Processing va Training.
* SageMaker Endpoint.
* Amazon CloudWatch Logs.
* Data transfer.
* Cac tai nguyen hoat dong lien tuc hoac tinh phi theo thoi gian.

Truoc khi tao tai nguyen co kha nang phat sinh chi phi, thanh vien can thong bao:

```text
Service:
Resource name:
Instance type hoac cau hinh:
Muc dich:
Thoi gian du kien chay:
Owner:
```

## 4. Service Quotas

Ben canh budget, nhom can theo doi Service Quotas de tranh bi chan khi trien khai MVP, dac biet voi cac dich vu nhu SageMaker hoac cac tai nguyen can quota theo instance type.

Viec kiem tra quota som giup nhom:

- biet truoc tai nguyen nao co the tao duoc,
- tranh phu thuoc vao mot demo live neu quota chua san sang,
- va chuan bi phuong an du phong khi can.

## 5. Architecture, Naming & Tag Convention

Kien truc tong the duoc thong nhat theo luong:

```text
Simulator
-> AWS IoT Core
-> IoT Rule
-> Kinesis Data Firehose
-> S3 Raw
-> Data Processing
-> S3 Processed
-> SageMaker
-> FastAPI
-> SNS
```

Naming convention duoc ap dung:

```text
local-aqi-{environment}-{resource-purpose}
```

Vi du:

```text
local-aqi-dev-s3-raw
local-aqi-dev-s3-processed
local-aqi-dev-firehose-to-s3
local-aqi-dev-sagemaker-execution-role
```

Tag convention toi thieu:

```text
Project=local-aqi
Environment=dev
Owner=<member-name>
ManagedBy=manual
CostCenter=student-project
```
