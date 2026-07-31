---
title : "Machine Learning: huấn luyện và tạo forecast"
date : 2026-07-31
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

#### Muc tieu cua role

Role Machine Learning phu trach su dung dataset processed de huan luyen mo hinh du bao PM2.5 va tao ket qua forecast cho 24 gio tiep theo.

#### Muc tieu ky thuat

+ Doc dataset tu `local-aqi-dev-s3-processed`
+ Chia train / validation / test theo thoi gian
+ Huan luyen mo hinh time-series phu hop
+ Danh gia bang metric nhu MAE, RMSE
+ Luu model artifact va ket qua forecast

#### Khuon trinh bay cho phan nay

Khi nhom gui implementation va evidence thuc te, phan nay se duoc viet theo:

1. Chuan bi dataset train, validation va test.
2. Giai thich vi sao khong random split.
3. Cau hinh training job tren SageMaker hoac local training neu quota chua san sang.
4. Kiem tra log training, model artifact va ket qua metric.
5. Tao file forecast result cho API va canh bao.

#### Ket qua can chung minh

+ Co training job that hoac local training evidence that su.
+ Co metric danh gia ro rang.
+ Co forecast result cho it nhat mot station.
+ Co model artifact hoac output file co the dung lai cho Backend.

{{% notice note %}}
Noi dung chi tiet cua role Machine Learning se duoc bo sung sau khi nhom gui them thong tin training job, metric va output forecast.
{{% /notice %}}
