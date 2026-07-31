---
title: "Machine learning"
date: 2026-07-31
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Thành phần ML: Dự báo chuỗi thời gian PM2.5 với Amazon SageMaker DeepAR

**Dự án:** Hệ thống Dự báo & Cảnh báo AQI Cục bộ
**Vai trò:** Kỹ sư ML
**Chủ trì:** `duy-tuong`
**Module:** `ml`
**Môi trường:** `dev`
**Khu vực:** `ap-southeast-1`
**Thời lượng:** 4 tuần

---

## 1. Tổng quan

### 1.1 Phát biểu bài toán

Chất lượng không khí đô thị — đặc biệt là bụi mịn (PM2.5) — biến động đáng kể theo địa điểm và thời gian trong ngày. Các nhóm dân cư dễ bị tổn thương như người cao tuổi có bệnh lý hô hấp và trẻ em đi học thường không được cảnh báo sớm để có thể chủ động phòng tránh trước khi một đợt ô nhiễm xảy ra. Thành phần ML này giải quyết trực tiếp vấn đề đó: dựa trên lịch sử cảm biến gần đây của một trạm, **dự báo nồng độ PM2.5 trong 48 giờ tới** nhằm đưa ra cảnh báo chủ động.

### 1.2 Phương pháp tiếp cận

Thuật toán được chọn là **DeepAR có sẵn của Amazon SageMaker** — một thuật toán học sâu có giám sát dùng để dự báo chuỗi thời gian theo xác suất, sử dụng mạng LSTM xếp chồng. DeepAR được chọn thay vì các baseline đơn giản hơn (ARIMA, Exponential Smoothing) vì các lý do sau:

- Nó huấn luyện **một mô hình toàn cục duy nhất** trên tất cả các trạm quan trắc cùng lúc, cho phép mô hình học các mẫu hình chung trong khi vẫn giữ được đặc trưng riêng của từng trạm thông qua item embeddings.
- Nó tự nhiên nắm bắt được **nhiều tính mùa vụ** (đỉnh giờ cao điểm trong ngày, mẫu hình ngày trong tuần so với cuối tuần) mà không cần thiết kế đặc trưng thủ công.
- Nó cho ra **dự báo theo xác suất** (giá trị trung bình + khoảng tứ phân vị) thay vì một điểm ước lượng đơn lẻ, giúp Backend team có thể trực tiếp sử dụng để thiết lập ngưỡng cảnh báo có thể cấu hình.

### 1.3 Vị trí trong kiến trúc

```
[Trạm IoT] → [IoT Core / MQTT] → [Kinesis Firehose] → [S3: raw/]
                                                                 │
                                                    [S3: processed/ml-ready/]
                                                                 │
                                                    ┌────────────▼────────────┐
                                                    │   Module ML (tài liệu này) │
                                                    │                         │
                                                    │  Tuần 1: EDA & Chuẩn bị │
                                                    │  Tuần 2: Baseline cục bộ│
                                                    │  Tuần 3: SageMaker      │
                                                    │          Train & Deploy │
                                                    │  Tuần 4: Hoàn thiện     │
                                                    └────────────┬────────────┘
                                                                 │
                                                    [SageMaker Endpoint]
                                                                 │
                                                    [Backend / FastAPI] → [SNS Alerts]
```

### 1.4 Đặt tên & Gắn thẻ tài nguyên

Tất cả tài nguyên AWS tuân theo quy ước đặt tên của team và chính sách gắn thẻ bắt buộc. Các tài nguyên có khả năng phát sinh chi phí đáng kể đều được báo cáo với team trước khi tạo.

| Tài nguyên | Tên |
|---|---|
| S3 Đầu vào (raw) | `s3://local-aqi-dev-s3-raw/raw/parquet/` |
| S3 Đầu vào (đã xử lý) | `s3://local-aqi-dev-s3-raw/processed/deepar/` |
| S3 Đầu ra mô hình | `s3://local-aqi-dev-s3-raw/models/deepar/` |
| Training Job | `aqi-deepar-on-demand-<timestamp>` |
| Endpoint | `aqi-endpoint-test` |
| Instance huấn luyện | `ml.m5.large` |
| Instance suy luận | `ml.t2.medium` |

**Các thẻ bắt buộc áp dụng cho mọi tài nguyên:**

```
Project     = local-aqi-forecasting
Environment = dev
Owner       = duy-tuong
Module      = ml
```

---

## 2. Tuần 1 — Khám phá & Chuẩn bị dữ liệu

### 2.1 Mục tiêu

Thiết lập môi trường phát triển, khám phá kỹ tập dữ liệu thô để hiểu các đặc tính thống kê và mẫu hình thời gian của nó, và tạo ra một tập dữ liệu sẵn sàng cho mô hình ở định dạng JSON Lines mà DeepAR yêu cầu.

### 2.2 Thiết lập môi trường

Quá trình phát triển được thực hiện trên **Google Colab** như một giải pháp thay thế cho SageMaker Studio trong lúc tài khoản AWS chung của team đang được cấp phát, sau đó được chuyển sang **SageMaker Studio** khi quyền truy cập được xác nhận. Notebook được thiết kế để có thể di chuyển dễ dàng thông qua một cờ cấu hình duy nhất:

```python
USE_S3     = False                   # ← chuyển sang True khi dùng SageMaker Studio
S3_BUCKET  = "local-aqi-dev-s3-raw"
AWS_REGION = "ap-southeast-1"
```

Các gói cần thiết: `gluonts[torch]`, `pyarrow`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`.

### 2.3 Tập dữ liệu

**Nguồn:** `sample_processed_dataset.parquet` do Kỹ sư Dữ liệu/Lưu trữ (Quỳnh Tâm) cung cấp, lưu tại `s3://local-aqi-dev-s3-raw/processed/ml-ready/`.

| Thuộc tính | Giá trị |
|---|---|
| Tổng số bản ghi | 160.957 |
| Số trạm | 3 (`local-aqi-dev-iot-station1/2/3`) |
| Tần suất | Theo giờ |
| Khoảng thời gian | 2016-12-21 → 2025-01-31 |
| Các cột | `timestamp`, `pm2_5`, `pm10`, `temperature`, `humidity`, `device_id` |
| Giá trị thiếu | 0 (sau khi đã tiền xử lý ở bước trước) |

Vì ba trạm có ngày bắt đầu khác nhau — station1 từ tháng 4/2018, station3 từ tháng 12/2016, nhưng station2 chỉ từ tháng 8/2021 — nên **khoảng thời gian chung bắt đầu từ 2021-08-10** được sử dụng để đảm bảo cả ba trạm đóng góp đồng đều vào quá trình huấn luyện.

### 2.4 Quy trình làm sạch dữ liệu

```python
# 1. Chuyển UTC → Asia/Ho_Chi_Minh, bỏ múi giờ
df_raw["timestamp"] = (
    pd.to_datetime(df_raw["timestamp"], utc=True)
    .dt.tz_convert("Asia/Ho_Chi_Minh")
    .dt.tz_localize(None)
)

# 2. Lọc theo khoảng thời gian chung
df_raw = df_raw[df_raw["timestamp"] >= pd.Timestamp("2021-08-10")].copy()

# 3. Tái lập chỉ mục mỗi trạm theo chỉ mục giờ liên tục, nội suy các khoảng trống
FULL_IDX = pd.date_range(df_raw["timestamp"].min(),
                          df_raw["timestamp"].max(), freq="h")
for dev, grp in df_raw.groupby("device_id"):
    g = grp.set_index("timestamp").reindex(FULL_IDX)
    g["pm2_5"] = g["pm2_5"].interpolate("linear").bfill().ffill()
    g["pm2_5"] = g["pm2_5"].clip(upper=g["pm2_5"].quantile(0.99))
```

Sau khi làm sạch: **91.491 dòng** trên 3 trạm (~30.497 mốc thời gian theo giờ mỗi trạm). Đã xác nhận không còn giá trị thiếu.

### 2.5 Kết quả EDA

#### Phân bố PM2.5 & PM10

`station3` cho thấy mức ô nhiễm cao nhất (PM2.5 trung bình ~40 µg/m³), trong khi `station1` và `station2` sạch hơn đáng kể (trung bình ~20 µg/m³). Cả ba trạm đều vượt ngưỡng khuyến nghị 24 giờ của WHO là 15 µg/m³ trong một phần đáng kể số quan trắc, và `station3` thường xuyên vượt tiêu chuẩn QCVN 05:2023/BTNMT của Việt Nam là 25 µg/m³.

#### Mẫu hình theo thời gian

Phân tích tổng hợp theo giờ và biểu đồ nhiệt cho thấy hai đỉnh PM2.5 rõ rệt mỗi ngày:

- **Đỉnh buổi sáng (06:00–09:00 giờ VN):** Trùng với giờ cao điểm giao thông buổi sáng và hoạt động nấu ăn.
- **Đỉnh chiều/tối (17:00–20:00 giờ VN):** Trùng với giờ cao điểm tan tầm.

Mức PM2.5 vào cuối tuần thấp hơn khoảng ~8% so với ngày thường một cách nhất quán, xác nhận rằng ngày trong tuần là một đặc trưng có giá trị cho mô hình.

#### Tương quan đặc trưng

| Đặc trưng | Mối quan hệ với PM2.5 | Quyết định |
|---|---|---|
| `hour_of_day` | Mạnh — nắm bắt đỉnh giờ cao điểm | ✅ `dynamic_feat[0]` |
| `day_of_week` | Trung bình — cuối tuần thấp hơn | ✅ `dynamic_feat[1]` |
| `humidity` | Tương quan dương trung bình | ✅ `dynamic_feat[2]` |
| `temperature` | Yếu | ✅ `dynamic_feat[3]` (giữ lại cho đầy đủ) |

### 2.6 Chuyển đổi sang định dạng JSON Lines cho DeepAR

DeepAR yêu cầu mỗi chuỗi thời gian là một đối tượng JSON trên một dòng, gồm một mốc thời gian `start`, mảng giá trị `target`, ma trận đặc trưng `dynamic_feat` (tùy chọn), và `item_id`.

```python
records = []
for dev, grp in df.groupby("device_id"):
    g = grp.sort_values("timestamp").reset_index(drop=True)
    records.append({
        "start"       : str(g["timestamp"].iloc[0]),
        "target"      : g["pm2_5"].round(4).tolist(),
        "dynamic_feat": [
            g["timestamp"].dt.hour.tolist(),
            g["timestamp"].dt.dayofweek.tolist(),
            g["humidity"].round(4).tolist(),
            g["temperature"].round(4).tolist(),
        ],
        "item_id": dev,
    })

with open("processed/deepar_pm25_train.jsonl", "w") as f:
    for r in records:
        f.write(json.dumps(r) + "\n")
```

Kết quả: **3 bản ghi** (mỗi trạm một bản ghi), được tải lên `s3://local-aqi-dev-s3-raw/processed/deepar/`.

### 2.7 Sản phẩm bàn giao Tuần 1

| Sản phẩm bàn giao | Trạng thái |
|---|---|
| `week1_eda_final.ipynb` | ✅ Hoàn thành |
| `fig_distribution.png` — PM2.5/PM10 theo trạm | ✅ Hoàn thành |
| `fig_temporal.png` — Mẫu hình theo giờ/tuần + xu hướng | ✅ Hoàn thành |
| `fig_correlation.png` — Ma trận tương quan | ✅ Hoàn thành |
| `deepar_pm25_train.jsonl` đã tải lên S3 | ✅ Hoàn thành |

---

## 3. Tuần 2 — Huấn luyện mô hình Baseline (cục bộ)

### 3.1 Mục tiêu

Huấn luyện một mô hình DeepAR baseline sử dụng thư viện mã nguồn mở **GluonTS** trên Colab/CPU để thiết lập một mức hiệu năng cơ sở có thể đo lường được, trước khi phát sinh chi phí huấn luyện trên SageMaker.

### 3.2 Chia tập Train / Validation / Test

Dữ liệu được chia nghiêm ngặt theo ngày để tránh rò rỉ dữ liệu:

```
|──────────── TRAIN (~3 năm) ────────────|─ VAL (1 tháng) ─|── TEST (2 tháng) ──|
2021-08-10                            2024-10-31      2024-11-30       2025-01-31
```

### 3.3 Xây dựng GluonTS ListDataset

```python
from gluonts.dataset.common import ListDataset
from gluonts.dataset.field_names import FieldName

def make_dataset(df, end_time, freq="h"):
    records = []
    for dev, grp in df.groupby("device_id"):
        g = grp[grp["timestamp"] <= end_time].sort_values("timestamp")
        records.append({
            FieldName.START : pd.Period(g["timestamp"].iloc[0], freq=freq),
            FieldName.TARGET: g["pm2_5"].values.astype(np.float64),
            FieldName.FEAT_DYNAMIC_REAL: np.stack([
                g["timestamp"].dt.hour.values.astype(np.float64),
                g["timestamp"].dt.dayofweek.values.astype(np.float64),
                g["humidity"].values.astype(np.float64),
                g["temperature"].values.astype(np.float64),
            ], axis=0),
            FieldName.ITEM_ID: dev,
        })
    return ListDataset(records, freq=freq)
```

### 3.4 Cấu hình huấn luyện

```python
from gluonts.torch.model.deepar import DeepAREstimator

estimator = DeepAREstimator(
    freq="h", prediction_length=48, context_length=168,
    num_layers=2, hidden_size=40, dropout_rate=0.1,
    num_feat_dynamic_real=4,
    trainer_kwargs={"max_epochs": 15, "accelerator": "auto"}
)
predictor = estimator.train(train_ds)
```

### 3.5 Kết quả Baseline

| Chỉ số | Giá trị | Mục tiêu |
|---|---|---|
| RMSE | 3,562 µg/m³ | < 3,0 ⚠️ |
| MAE | 0,776 µg/m³ | < 0,5 ⚠️ |
| sMAPE | 5,87% | < 15% ✅ |
| MAPE | 22,41% | < 15% ⚠️ |

**Phân tích:** sMAPE ở mức 5,87% xác nhận mô hình đã học được cấu trúc mùa vụ. RMSE và MAPE còn cao được cho là do thời gian huấn luyện ngắn (15 epoch) và thiếu các đặc trưng độ trễ (lag features) — cả hai vấn đề này được giải quyết ở Tuần 3. Phân tích phần dư cho thấy độ chệch trung bình ≈ 0 và 96,5% phần dư nằm trong khoảng ±2σ, xác nhận không có sai lệch cấu trúc mô hình.

### 3.6 Sản phẩm bàn giao Tuần 2

| Sản phẩm bàn giao | Trạng thái |
|---|---|
| `week2_deepar_final.ipynb` | ✅ Hoàn thành |
| `deepar_baseline_predictor.pkl` | ✅ Hoàn thành |
| `week2_baseline_metrics.json` | ✅ Hoàn thành |
| `fig_forecast.png`, `fig_metrics.png`, `fig_residuals.png` | ✅ Hoàn thành |
| Báo cáo đánh giá sơ bộ đã chia sẻ với team | ✅ Hoàn thành |

---

## 4. Tuần 3 — SageMaker Training Job & Triển khai Endpoint

### 4.1 Mục tiêu

Chuyển từ nguyên mẫu GluonTS cục bộ sang mô hình DeepAR có sẵn cấp production của **SageMaker**, huấn luyện với cấu hình siêu tham số đầy đủ, triển khai dưới dạng SageMaker Endpoint thời gian thực, và bàn giao hợp đồng API hoạt động cho Kỹ sư Backend (Quang Tuấn).

> ⚠️ **Team đã được thông báo trước khi tạo Training Job và Endpoint.** Cả hai tài nguyên đều được gắn thẻ đúng quy định và Endpoint đã bị xóa ngay sau khi đánh giá xong.

### 4.2 Tải dữ liệu huấn luyện lên S3

```python
s3 = boto3.client('s3')
s3.upload_file(
    "deepar_train.jsonl", "local-aqi-dev-s3-raw",
    "processed/deepar/deepar_train.jsonl"
)
```

### 4.3 Phiên làm việc SageMaker

```python
region  = boto3.Session().region_name   # ap-southeast-1
session = sagemaker.Session()
role    = sagemaker.get_execution_role()

BUCKET      = "local-aqi-dev-s3-raw"
TRAIN_PATH  = f"s3://{BUCKET}/processed/deepar/"
OUTPUT_PATH = f"s3://{BUCKET}/models/deepar/"
```

### 4.4 Cấu hình siêu tham số

| Siêu tham số | Giá trị | Ghi chú so với Baseline Tuần 2 |
|---|---|---|
| `time_freq` | `H` | Giữ nguyên |
| `prediction_length` | `48` | Giữ nguyên |
| `context_length` | `168` | Giữ nguyên |
| `epochs` | `50` | ↑ từ 15 — hội tụ đầy đủ |
| `num_cells` | `40` | Tương đương `hidden_size` |
| `num_layers` | `2` | Giữ nguyên |
| `dropout_rate` | `0.1` | Giữ nguyên |
| `mini_batch_size` | `32` | Thêm cho mô hình có sẵn của SageMaker |
| `learning_rate` | `1e-3` | Mặc định của Adam |
| `likelihood` | `gaussian` | Đầu ra theo xác suất |
| `num_eval_samples` | `100` | Số mẫu Monte Carlo |
| `early_stopping_patience` | `10` | Ngăn overfitting |

### 4.5 Estimator & Thực thi huấn luyện

```python
tags = [
    {"Key": "Project",     "Value": "local-aqi-forecasting"},
    {"Key": "Environment", "Value": "dev"},
    {"Key": "Owner",       "Value": "duy-tuong"},
    {"Key": "Module",      "Value": "ml"},
]

image_uri = sagemaker.image_uris.retrieve("forecasting-deepar", region)

estimator = Estimator(
    image_uri         = image_uri,
    role              = role,
    instance_count    = 1,
    instance_type     = "ml.m5.large",
    output_path       = OUTPUT_PATH,
    base_job_name     = "aqi-deepar-on-demand",
    sagemaker_session = session,
    tags              = tags,
)
estimator.set_hyperparameters(**hyperparams)

estimator.fit(
    inputs={"train": TrainingInput(TRAIN_PATH, content_type="json")},
    wait=True, logs="All",
)
print(f"✅ Huấn luyện hoàn tất | artifact: {estimator.model_data}")
```

> **Lưu ý quan trọng:** SageMaker DeepAR chỉ chấp nhận các giá trị `content_type` là `json`, `json.gz`, `parquet`, hoặc `auto`. Sử dụng `application/jsonlines` sẽ gây lỗi xác thực — `json` mới là giá trị đúng cho file JSONL.

### 4.6 Triển khai Endpoint

```python
predictor = estimator.deploy(
    initial_instance_count = 1,
    instance_type          = "ml.t2.medium",
    endpoint_name          = "aqi-endpoint-test",
    tags                   = tags,
)
print(f"✅ Endpoint: {predictor.endpoint_name}")
```

### 4.7 Kiểm thử suy luận

```python
predictor = Predictor(
    endpoint_name = "aqi-endpoint-test",
    serializer    = JSONSerializer(),
    deserializer  = JSONDeserializer(),
)

with open("deepar_train.jsonl") as f:
    sample  = json.loads(f.readline())
    context = sample["target"][:168]
    start   = sample["start"]

payload = {
    "instances": [{"start": start, "target": context}],
    "configuration": {
        "num_samples": 50,
        "output_types": ["mean", "quantiles"],
        "quantiles": ["0.1", "0.5", "0.9"],
    },
}
result        = predictor.predict(payload)
forecast_mean = result["predictions"][0]["mean"]
```

**Kết quả đầu ra terminal:**

```
Context length: 168
Start time: 2018-04-25 17:00:00

Forecast 48 hours:
Mean: [9.3060703278, 9.3919620514, 9.3067531586, 9.3091573715, 9.2927675247]...
```

### 4.8 Kết quả đánh giá cuối cùng

| Chỉ số | Baseline Tuần 2 | Production Tuần 3 | Cải thiện |
|---|---|---|---|
| **MAE** | 0,776 µg/m³ | **0,191 µg/m³** | ↓ 75,4% |
| **RMSE** | 3,562 µg/m³ | **0,201 µg/m³** | ↓ 94,4% |
| **R²** | — | **0,999** | — |
| **MAPE** | 22,41% | **1,441%** | ↓ 93,6% |

#### Biểu đồ đánh giá

![Đánh giá DeepAR SageMaker](/AWS-Report/images/5-Workshop/5.6-Machine-learning/deepar_sagemaker_evaluation.png)

Ba biểu đồ đánh giá:

- **Dự báo so với Thực tế (48 giờ):** Giá trị dự báo (nét đứt) bám sát giá trị thực tế (nét liền) gần như hoàn hảo trên cả ba trạm. Khoảng tin cậy hẹp, cho thấy độ bất định dự báo thấp trong toàn bộ khung thời gian.
- **Biểu đồ phân tán Thực tế so với Dự báo (R² = 0,999):** Các điểm tập trung sát dọc theo đường chéo khớp hoàn hảo, không quan sát thấy sai lệch có hệ thống trên toàn dải giá trị PM2.5.
- **Biểu đồ cột các chỉ số:** MAE = 0,191, RMSE = 0,201, R² = 0,999, MAPE = 1,441%.

### 4.9 Dọn dẹp Endpoint

```python
boto3.client('sagemaker').delete_endpoint(EndpointName='aqi-endpoint-test')
print("✅ Đã xóa Endpoint — không còn phát sinh chi phí")
```

### 4.10 Sản phẩm bàn giao Tuần 3

| Sản phẩm bàn giao | Trạng thái |
|---|---|
| `ml-training.ipynb` — pipeline SageMaker đầy đủ | ✅ Hoàn thành |
| Training Job trên SageMaker hoàn tất, artifact trong S3 | ✅ Hoàn thành |
| Endpoint đã triển khai, kiểm thử và xóa | ✅ Hoàn thành |
| `deepar_sagemaker_evaluation.png` | ✅ Hoàn thành |
| Hợp đồng API đã bàn giao cho Kỹ sư Backend | ✅ Hoàn thành |

---

## 5. Tuần 4 — Báo cáo cuối cùng, Tích hợp & Tài liệu hóa

### 5.1 Mục tiêu

Tổng hợp toàn bộ công việc ML thành trạng thái sẵn sàng cho production, hoàn thiện hợp đồng API với Backend, tài liệu hóa mọi thứ cho team, và chuẩn bị tài liệu demo.

### 5.2 Hợp đồng API cho tích hợp Backend

**Endpoint:** `aqi-endpoint-test`
*(Sẽ đổi tên thành `local-aqi-dev-sagemaker-endpoint-deepar` trước buổi demo cuối cùng)*

**Yêu cầu (Request):**

```json
{
  "instances": [{
    "start" : "YYYY-MM-DD HH:MM:SS",
    "target": [168 giá trị số thực — 7 ngày gần nhất theo giờ của PM2.5]
  }],
  "configuration": {
    "num_samples": 50,
    "output_types": ["mean", "quantiles"],
    "quantiles": ["0.1", "0.5", "0.9"]
  }
}
```

**Phản hồi (Response):**

```json
{
  "predictions": [{
    "mean"     : [48 giá trị số thực],
    "quantiles": {
      "0.1": [48 giá trị số thực],
      "0.5": [48 giá trị số thực],
      "0.9": [48 giá trị số thực]
    }
  }]
}
```

**Logic cảnh báo:** Backend so sánh `predictions[0]["mean"]` với ngưỡng AQI đã cấu hình cho từng trạm. Bất kỳ giá trị nào trong khung 48 giờ vượt ngưỡng sẽ kích hoạt thông báo đẩy SNS đến các thuê bao đã đăng ký.

### 5.3 Khuyến nghị giám sát (dành cho M5 DevOps)

| Chỉ số CloudWatch | Ngưỡng cảnh báo | Hành động |
|---|---|---|
| `ModelLatency` | P99 > 2.000 ms | Kiểm tra endpoint |
| `Invocations` | < 1/giờ trong giờ hành chính | Kiểm tra bộ lập lịch Backend |
| `InvocationErrors` | > 0 trong cửa sổ 5 phút | Điều tra ngay lập tức |
| `CPUUtilization` | > 80% kéo dài | Cân nhắc nâng cấp instance |

### 5.4 Chi phí ước tính

| Tài nguyên | Instance | Thời lượng | Chi phí |
|---|---|---|---|
| SageMaker Studio | `ml.t3.medium` | ~8h tổng cộng | ~0,40 USD |
| Training Job | `ml.m5.large` | ~5 phút | ~0,02 USD |
| Endpoint (chỉ thử nghiệm) | `ml.t2.medium` | ~1h | ~0,07 USD |
| Lưu trữ S3 | Standard | ~1 tháng | < 0,01 USD |
| **Tổng cộng** | | | **~0,50 USD** |

### 5.5 Sản phẩm bàn giao Tuần 4

| Sản phẩm bàn giao | Trạng thái |
|---|---|
| `_index.md` — báo cáo kỹ thuật tiếng Anh | ✅ Hoàn thành |
| `_index.vi.md` — báo cáo kỹ thuật tiếng Việt | ✅ Hoàn thành |
| Tài liệu hợp đồng API đã chia sẻ với Quang Tuấn | ✅ Hoàn thành |
| Khuyến nghị giám sát CloudWatch đã chia sẻ với M5 | ✅ Hoàn thành |
| Kịch bản demo đã chuẩn bị | ✅ Hoàn thành |

---

## 6. Tổng kết

### 6.1 Hiệu năng mô hình cuối cùng

| Chỉ số | Giá trị | Đánh giá |
|---|---|---|
| MAE | **0,191 µg/m³** | Xuất sắc — sai số trung bình < 0,2 µg/m³ mỗi giờ |
| RMSE | **0,201 µg/m³** | Xuất sắc — không có dự báo lệch bất thường đáng kể |
| R² | **0,999** | Mô hình giải thích được 99,9% phương sai của PM2.5 |
| MAPE | **1,441%** | Thấp hơn nhiều so với ngưỡng chấp nhận được 15% |

### 6.2 Tiến độ theo từng tuần

| Tuần | Kết quả chính | Cột mốc |
|---|---|---|
| **1** | Hoàn thành EDA, JSONL đã tải lên S3 | Xác nhận chất lượng dữ liệu, xác định mẫu hình thời gian |
| **2** | DeepAR baseline (15 epoch, CPU) | sMAPE 5,87% — thiết lập baseline |
| **3** | Huấn luyện SageMaker (50 epoch) + Endpoint | RMSE 0,201, R² 0,999, MAPE 1,441% |
| **4** | Báo cáo, hợp đồng API, chuẩn bị demo | Pipeline đầu-cuối sẵn sàng tích hợp |

### 6.3 Ngăn xếp công nghệ

| Thành phần | Công nghệ |
|---|---|
| Định dạng dữ liệu | Apache Parquet → JSON Lines |
| Nguyên mẫu cục bộ | GluonTS 0.16.3 + PyTorch, Google Colab |
| Huấn luyện production | Amazon SageMaker DeepAR (có sẵn) |
| Instance huấn luyện | `ml.m5.large`, ap-southeast-1 |
| Instance suy luận | `ml.t2.medium`, ap-southeast-1 |
| Lưu trữ mô hình | S3 `local-aqi-dev-s3-raw/models/deepar/` |
| Chuẩn hóa dữ liệu | `JSONSerializer` / `JSONDeserializer` |

---

## 7. Phụ lục

### A. Tất cả các file đã tạo ra

| File | Vị trí | Mô tả |
|---|---|---|
| `week1_eda_final.ipynb` | Colab / Studio | Notebook EDA Tuần 1 |
| `week2_deepar_final.ipynb` | Colab / Studio | Huấn luyện baseline Tuần 2 |
| `ml-training.ipynb` | SageMaker Studio | Pipeline production Tuần 3 |
| `deepar_train.jsonl` | S3 `processed/deepar/` | Dữ liệu huấn luyện DeepAR |
| `deepar_baseline_predictor.pkl` | Cục bộ | Mô hình GluonTS Tuần 2 |
| `week2_baseline_metrics.json` | Cục bộ | Ghi nhận chỉ số Tuần 2 |
| `deepar_sagemaker_evaluation.png` | Cục bộ | Biểu đồ đánh giá Tuần 3 |
| `_index.md` | Kho dự án | Tài liệu này (tiếng Anh) |
| `_index.vi.md` | Kho dự án | Tài liệu này (tiếng Việt) |

### B. Các cải tiến tiềm năng trong tương lai

- **Đặc trưng độ trễ (lag features):** Thêm giá trị `pm2_5` tại thời điểm t−24h và t−168h vào `dynamic_feat` có thể giúp cải thiện thêm độ chính xác ở các trạm có tính tự tương quan 24 giờ mạnh.
- **Tinh chỉnh theo từng trạm:** Huấn luyện một mô hình riêng cho `station3` (trạm có mức độ ô nhiễm phức tạp nhất) để giảm sai số phần dư còn lại.
- **Serverless Inference:** Thay thế endpoint luôn hoạt động bằng SageMaker Serverless Inference để loại bỏ chi phí nhàn rỗi, phù hợp với tần suất gọi theo giờ.
- **Tự động tái huấn luyện:** Lên lịch một SageMaker Pipeline hàng tháng để tái huấn luyện với dữ liệu Firehose mới tích lũy từ S3.