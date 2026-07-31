---
title : "Ingestion: Simulator -> IoT Core -> Firehose -> S3 Raw"
date : 2026-07-31
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Muc tieu cua role

Role Ingestion phu trach dua du lieu telemetry tu simulator vao AWS va chung minh object that xuat hien trong S3 Raw.

Day la milestone uu tien cao nhat cua MVP, vi neu khong co du lieu raw that thi cac phan Data Preparation, Machine Learning va Backend deu chua the xem la hoan tat.

#### Luong can dat

```text
Simulator
-> AWS IoT Core
-> IoT Rule
-> Kinesis Data Firehose
-> S3 Raw
```

## Trien khai tung buoc: Thu thap va dinh tuyen du lieu

### Buoc 1: Kiem tra tai nguyen Cloud

Dam bao delivery stream cua **Amazon Data Firehose** da duoc nhom Data Engineering tao truoc.

Kiem tra Firehose da duoc cau hinh de:

* Nhan du lieu theo lo.
* Ghi du lieu an toan vao bucket Amazon S3.
* Luu du lieu tai prefix:

```text
raw/telemetry/
```

---

### Buoc 2: Cau hinh bao mat AWS IoT Core

Truy cap:

```text
AWS IoT Core
-> Security
-> Policies
```

Tao mot IoT Policy gioi han quyen, chi cho phep cac action:

```text
iot:Connect
iot:Publish
```

Quyen publish can duoc gioi han vao dung topic:

```text
telemetry/aqi/dev
```

#### Gan tag cho tai nguyen

Ap dung bo tag chuan cua du an de thuan tien cho viec theo doi va quan ly tai nguyen:

```text
Project: local-aqi-forecasting
Environment: dev
Owner: [Ten nguoi phu trach]
Module: ingestion
```

Tiep theo, truy cap:

```text
AWS IoT Core
-> Security
-> Certificates
```

Tao certificate moi va tai xuong cac file sau:

* Root CA.
* Private Key.
* Device Certificate.

Sau khi tao certificate, gan IoT Policy vua tao vao certificate nay.

---

### Buoc 3: Cau hinh dinh tuyen message tu IoT Rule sang Firehose

Truy cap:

```text
AWS IoT Core
-> Message routing
-> Rules
```

Tao mot IoT Rule moi, vi du:

```text
Route_To_Firehose
```

#### Cau lenh SQL

Su dung cau lenh sau de lay toan bo du lieu telemetry duoc publish vao topic:

```sql
SELECT * FROM 'telemetry/aqi/dev'
```

#### Cau hinh Rule Action

Tai phan action, chon:

```text
Data Firehose stream
```

hoac:

```text
Send a message to a Data Firehose stream
```

Tai muc Firehose Stream, chon delivery stream da duoc tao truoc do.

#### Cau hinh IAM Role

Chon IAM role da co hoac yeu cau quan tri vien tao role moi, vi du:

```text
local-aqi-dev-iot-to-firehose
```

Role nay can cho phep AWS IoT Core thuc hien action:

```text
firehose:PutRecord
```

tren dung Firehose delivery stream cua du an.

#### Trust relationship

IAM role phai co Trust Policy cho phep service principal sau assume role:

```text
iot.amazonaws.com
```

Vi du:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "iot.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

### Buoc 4: Chuan bi moi truong lam viec cuc bo

Clone repository cua du an ve may:

```bash
git clone <repository-url>
```

Di chuyen cac certificate AWS da tai xuong vao thu muc:

```text
certs/
```

Dam bao thu muc nay da duoc them vao `.gitignore` de tranh lam lo credential.

Vi du:

```gitignore
certs/
*.pem
*.key
*.crt
```

Dat bo du lieu lich su da duoc lam sach vao thu muc:

```text
data/
```

Nen su dung virtual environment truoc khi cai dependency.

Vi du:

```bash
python -m venv .venv
```

Kich hoat moi truong tren Windows:

```bash
.venv\Scripts\activate
```

Kich hoat moi truong tren Linux hoac WSL:

```bash
source .venv/bin/activate
```

Cai dat cac thu vien can thiet:

```bash
pip install paho-mqtt pandas
```

---

### Buoc 5: Chay simulator va kiem tra he thong

Chay script simulator de bat dau gui du lieu:

```bash
python simulator.py
```

#### Kiem tra metric

Truy cap:

```text
AWS Console
-> Amazon Data Firehose
-> Chon delivery stream
-> Monitoring
```

Hoac:

```text
AWS Console
-> CloudWatch
-> Metrics
-> Firehose
```

Kiem tra cac metric:

```text
IncomingRecords
DeliveryToS3.Success
```

Su dung statistic:

```text
Sum
```

Cac metric phai ghi nhan duoc du lieu dang di vao he thong.

#### Kiem tra du lieu tren S3

Truy cap bucket S3 dich va kiem tra Firehose da ghi cac object JSON theo lo vao cau truc phan vung hay chua.

Vi du:

```text
raw/
└── year=2026/
    └── month=07/
        └── day=31/
            └── hour=10/
```

Firehose co the doi den khi dat kich thuoc buffer hoac het thoi gian buffer moi ghi object xuong S3. Vi vay, du lieu co the khong xuat hien ngay sau khi simulator gui message.

Vi du ket qua kiem thu:

```text
Firehose da nhan 37 records va ghi thanh cong mot object da duoc buffer xuong Amazon S3.
```

#### Ket qua can chung minh

+ Publish thanh cong toi dung topic.
+ IoT Rule route duoc record sang Firehose.
+ Firehose nhan record va ghi object thanh cong vao S3 Raw.
+ Payload trong S3 giong du lieu simulator.
+ He thong test duoc nhieu records thay vi chi mot message le.

## Ket qua dat duoc

Sau khi hoan thanh phan ingestion, nhom dat duoc moc quan trong nhat cua MVP:

```text
Simulator
-> AWS IoT Core
-> Firehose
-> S3 Raw
```

Dieu nay tao nen dau vao thuc te cho cac nhom Data Preparation, Machine Learning va Backend tiep tuc xu ly.
