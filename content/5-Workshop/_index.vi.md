---
title: "Dọn dẹp tài nguyên"
date: 2026-07-31
weight: 6
chapter: false
pre: " <b> 6. </b> "
--------------------

## Tổng quan

Sau khi hoàn thành quá trình triển khai và kiểm thử dự án **Local AQI Forecasting & Alert System**, các tài nguyên AWS không còn sử dụng cần được dừng hoặc xóa để tránh phát sinh chi phí.

Một số dịch vụ có thể tiếp tục tính phí ngay cả khi hệ thống không có người truy cập, đặc biệt là:

* Amazon SageMaker Endpoint.
* SageMaker Notebook Instance hoặc Studio Application.
* Amazon EC2 Instance.
* Amazon Data Firehose.
* NAT Gateway nếu có.
* Elastic IP không được gắn với tài nguyên.
* Dữ liệu lưu trong Amazon S3.
* CloudWatch Logs lưu trong thời gian dài.

Quá trình cleanup cần được thực hiện theo thứ tự để tránh lỗi do tài nguyên đang phụ thuộc lẫn nhau.

> **Cảnh báo:** Chỉ xóa tài nguyên sau khi đã xác nhận dữ liệu, ảnh chụp và kết quả cần thiết cho báo cáo đã được sao lưu. Dữ liệu trong Amazon S3 hoặc SageMaker có thể không khôi phục được sau khi xóa.

---

## 1. Kiểm tra tài nguyên trước khi dọn dẹp

Trước khi xóa tài nguyên, cần kiểm tra:

* Đúng AWS Account của dự án.
* Đúng Region `ap-southeast-1`.
* Đã tải về dữ liệu cần dùng cho báo cáo.
* Đã lưu model artifact và kết quả đánh giá.
* Đã chụp màn hình kiến trúc và kết quả kiểm thử.
* Không còn thành viên đang chạy Processing Job hoặc Training Job.
* Không còn demo hoặc buổi review cần sử dụng hệ thống.

Có thể kiểm tra tài nguyên theo tag:

```text
Project = local-aqi-forecasting
Environment = dev
```

<!-- ẢNH CẦN CHỤP:
AWS Resource Groups hoặc Tag Editor.
Lọc tài nguyên theo Project=local-aqi-forecasting.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/resources-by-tag.png
-->

![Danh sách tài nguyên của dự án](/images/6-Cleanup/resources-by-tag.png)

---

## 2. Sao lưu dữ liệu cần thiết

Trước khi xóa S3 Bucket, tải dữ liệu cần lưu về máy cục bộ.

### Sao lưu dữ liệu thô

```bash
aws s3 sync \
  s3://local-aqi-dev-s3-raw \
  ./backup/local-aqi-dev-s3-raw
```

### Sao lưu dữ liệu đã xử lý

```bash
aws s3 sync \
  s3://local-aqi-dev-s3-processed \
  ./backup/local-aqi-dev-s3-processed
```

Các dữ liệu cần ưu tiên sao lưu gồm:

* Telemetry JSON được simulator gửi lên.
* Dataset sau tiền xử lý.
* File kết quả dự báo.
* Model artifact.
* Metrics đánh giá mô hình.
* Log hoặc file minh chứng cho luồng end-to-end.

Sau khi sao lưu, kiểm tra số lượng file:

```bash
aws s3 ls \
  s3://local-aqi-dev-s3-raw \
  --recursive \
  --summarize
```

---

## 3. Xóa SageMaker Endpoint

SageMaker Endpoint là tài nguyên cần ưu tiên xóa vì endpoint có thể phát sinh chi phí liên tục khi vẫn ở trạng thái `InService`.

Truy cập:

```text
Amazon SageMaker
→ Inference
→ Endpoints
```

Chọn endpoint của dự án và thực hiện **Delete**.

<!-- ẢNH CẦN CHỤP:
SageMaker > Inference > Endpoints.
Hiển thị endpoint của dự án trước khi xóa.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/sagemaker-endpoint.png
-->

![SageMaker Endpoint](/images/6-Cleanup/sagemaker-endpoint.png)

Có thể xóa bằng AWS CLI:

```bash
aws sagemaker delete-endpoint \
  --endpoint-name <ENDPOINT_NAME>
```

Kiểm tra endpoint đã được xóa:

```bash
aws sagemaker list-endpoints
```

Endpoint không còn xuất hiện trong danh sách sau khi quá trình xóa hoàn tất.

---

## 4. Xóa SageMaker Endpoint Configuration

Sau khi xóa endpoint, tiếp tục xóa Endpoint Configuration.

Truy cập:

```text
Amazon SageMaker
→ Inference
→ Endpoint configurations
```

Hoặc sử dụng AWS CLI:

```bash
aws sagemaker delete-endpoint-config \
  --endpoint-config-name <ENDPOINT_CONFIG_NAME>
```

Endpoint Configuration không trực tiếp chạy compute instance, nhưng nên được xóa để tránh để lại cấu hình không còn sử dụng.

---

## 5. Xóa SageMaker Model

Sau khi endpoint và endpoint configuration đã được xóa, xóa SageMaker Model.

Truy cập:

```text
Amazon SageMaker
→ Inference
→ Models
```

Hoặc sử dụng AWS CLI:

```bash
aws sagemaker delete-model \
  --model-name <MODEL_NAME>
```

Lệnh này xóa metadata của model trong SageMaker. Model artifact lưu trong S3 cần được xóa riêng nếu không còn sử dụng.

---

## 6. Dừng hoặc xóa SageMaker Notebook

Nếu dự án sử dụng Notebook Instance, cần kiểm tra trạng thái và dừng instance sau khi hoàn thành công việc.

Truy cập:

```text
Amazon SageMaker
→ Notebook
→ Notebook instances
```

Chọn notebook và thực hiện:

```text
Stop
```

Nếu không còn sử dụng, tiếp tục chọn:

```text
Delete
```

<!-- ẢNH CẦN CHỤP:
Danh sách SageMaker Notebook Instance hoặc Studio Application.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/sagemaker-notebook.png
-->

![SageMaker Notebook](/images/6-Cleanup/sagemaker-notebook.png)

Đối với SageMaker Studio, cần kiểm tra và xóa các ứng dụng đang chạy như:

* JupyterLab.
* Code Editor.
* Kernel Gateway.
* Canvas.

Chỉ đóng tab trình duyệt không đồng nghĩa với việc compute resource đã được dừng.

---

## 7. Kiểm tra Processing Job và Training Job

Truy cập:

```text
Amazon SageMaker
→ Processing
→ Processing jobs
```

và:

```text
Amazon SageMaker
→ Training
→ Training jobs
```

Đảm bảo không còn job ở trạng thái:

```text
InProgress
```

Các job đã hoàn thành không tiếp tục sử dụng compute, nhưng output và model artifact của chúng vẫn có thể được lưu trong S3.

Có thể kiểm tra bằng AWS CLI:

```bash
aws sagemaker list-processing-jobs
```

```bash
aws sagemaker list-training-jobs
```

Nếu một job đang chạy không còn cần thiết, có thể dừng bằng:

```bash
aws sagemaker stop-processing-job \
  --processing-job-name <PROCESSING_JOB_NAME>
```

```bash
aws sagemaker stop-training-job \
  --training-job-name <TRAINING_JOB_NAME>
```

---

## 8. Dừng và xóa EC2 Instance

Amazon EC2 được sử dụng để triển khai FastAPI hoặc phục vụ demo hệ thống.

Truy cập:

```text
Amazon EC2
→ Instances
```

Nếu chỉ tạm ngừng dự án, có thể chọn:

```text
Stop instance
```

Nếu đã hoàn thành hoàn toàn, chọn:

```text
Terminate instance
```

<!-- ẢNH CẦN CHỤP:
EC2 > Instances.
Hiển thị instance backend của dự án và trạng thái trước khi dừng.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/ec2-instance.png
-->

![EC2 Instance](/images/6-Cleanup/ec2-instance.png)

Có thể dừng instance bằng AWS CLI:

```bash
aws ec2 stop-instances \
  --instance-ids <INSTANCE_ID>
```

Hoặc terminate:

```bash
aws ec2 terminate-instances \
  --instance-ids <INSTANCE_ID>
```

Sau khi terminate, kiểm tra thêm:

* EBS Volume có được xóa cùng instance hay không.
* Elastic IP có còn được cấp phát hay không.
* Security Group có còn được tài nguyên khác sử dụng hay không.
* Key Pair có còn cần giữ hay không.

---

## 9. Giải phóng Elastic IP

Elastic IP có thể phát sinh chi phí nếu được cấp phát nhưng không được sử dụng đúng điều kiện.

Truy cập:

```text
Amazon EC2
→ Network & Security
→ Elastic IP addresses
```

Nếu Elastic IP không còn được sử dụng:

1. Chọn địa chỉ IP.
2. Chọn **Actions**.
3. Chọn **Release Elastic IP addresses**.

Có thể thực hiện bằng AWS CLI:

```bash
aws ec2 release-address \
  --allocation-id <ALLOCATION_ID>
```

Không giải phóng Elastic IP nếu vẫn còn dịch vụ hoặc domain đang phụ thuộc vào địa chỉ này.

---

## 10. Xóa IoT Rule

Sau khi simulator không còn gửi dữ liệu, xóa IoT Rule để ngừng chuyển message sang Firehose.

Truy cập:

```text
AWS IoT Core
→ Message routing
→ Rules
```

Chọn rule của dự án và thực hiện **Delete**.

<!-- ẢNH CẦN CHỤP:
AWS IoT Core > Message routing > Rules.
Hiển thị rule IoT sang Firehose.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/iot-rule.png
-->

![IoT Rule](/images/6-Cleanup/iot-rule.png)

Có thể xóa bằng AWS CLI:

```bash
aws iot delete-topic-rule \
  --rule-name <IOT_RULE_NAME>
```

Xóa IoT Rule trước khi xóa IAM Role liên quan để tránh cấu hình tham chiếu đến role không còn tồn tại.

---

## 11. Vô hiệu hóa và xóa IoT Certificate

Truy cập:

```text
AWS IoT Core
→ Security
→ Certificates
```

Trước khi xóa certificate, cần:

1. Chuyển certificate từ `Active` sang `Inactive`.
2. Gỡ policy khỏi certificate.
3. Gỡ certificate khỏi Thing nếu có.
4. Xóa certificate.

Có thể cập nhật trạng thái bằng AWS CLI:

```bash
aws iot update-certificate \
  --certificate-id <CERTIFICATE_ID> \
  --new-status INACTIVE
```

Gỡ policy:

```bash
aws iot detach-policy \
  --policy-name <POLICY_NAME> \
  --target <CERTIFICATE_ARN>
```

Xóa certificate:

```bash
aws iot delete-certificate \
  --certificate-id <CERTIFICATE_ID>
```

Private key và certificate đã tải xuống máy cục bộ cũng cần được xóa an toàn nếu không còn sử dụng.

Không đưa private key hoặc certificate vào GitHub repository.

---

## 12. Xóa AWS IoT Policy

Sau khi policy đã được gỡ khỏi toàn bộ certificate, xóa policy tại:

```text
AWS IoT Core
→ Security
→ Policies
```

Có thể kiểm tra các target đang gắn policy:

```bash
aws iot list-targets-for-policy \
  --policy-name <POLICY_NAME>
```

Sau khi không còn target, xóa policy:

```bash
aws iot delete-policy \
  --policy-name <POLICY_NAME>
```

Nếu policy có nhiều version, cần xóa các version không mặc định trước khi xóa policy.

---

## 13. Xóa Amazon Data Firehose Delivery Stream

Sau khi IoT Rule đã được xóa và không còn dữ liệu mới được gửi, xóa Firehose delivery stream.

Truy cập:

```text
Amazon Data Firehose
→ Delivery streams
```

Chọn delivery stream của dự án và thực hiện **Delete**.

<!-- ẢNH CẦN CHỤP:
Amazon Data Firehose > Delivery streams.
Hiển thị delivery stream trước khi xóa.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/firehose-stream.png
-->

![Firehose Delivery Stream](/images/6-Cleanup/firehose-stream.png)

Có thể xóa bằng AWS CLI:

```bash
aws firehose delete-delivery-stream \
  --delivery-stream-name <FIREHOSE_STREAM_NAME> \
  --allow-force-delete
```

Sau khi gửi lệnh xóa, kiểm tra lại:

```bash
aws firehose list-delivery-streams
```

---

## 14. Xóa SNS Subscription và SNS Topic

Truy cập:

```text
Amazon SNS
→ Subscriptions
```

Xóa subscription email nếu không còn sử dụng.

Sau đó truy cập:

```text
Amazon SNS
→ Topics
```

Chọn topic cảnh báo của dự án và thực hiện **Delete**.

<!-- ẢNH CẦN CHỤP:
SNS Topic và subscription trước khi xóa.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/sns-topic-subscription.png
-->

![SNS Topic và Subscription](/images/6-Cleanup/sns-topic-subscription.png)

Có thể xóa topic bằng AWS CLI:

```bash
aws sns delete-topic \
  --topic-arn arn:aws:sns:ap-southeast-1:<AWS_ACCOUNT_ID>:<SNS_TOPIC_NAME>
```

Khi SNS Topic bị xóa, các subscription gắn với topic cũng không còn hoạt động.

---

## 15. Xóa dữ liệu trong S3 Bucket

Amazon S3 Bucket chỉ có thể được xóa khi không còn object bên trong.

### Xóa dữ liệu trong S3 Raw Bucket

```bash
aws s3 rm \
  s3://local-aqi-dev-s3-raw \
  --recursive
```

### Xóa dữ liệu trong S3 Processed Bucket

```bash
aws s3 rm \
  s3://local-aqi-dev-s3-processed \
  --recursive
```

Nếu bucket đã bật versioning, lệnh trên chỉ xóa phiên bản hiện tại. Các object version và delete marker vẫn cần được xóa riêng.

Kiểm tra trạng thái versioning:

```bash
aws s3api get-bucket-versioning \
  --bucket local-aqi-dev-s3-raw
```

```bash
aws s3api get-bucket-versioning \
  --bucket local-aqi-dev-s3-processed
```

<!-- ẢNH CẦN CHỤP:
Amazon S3 > bucket > Objects.
Hiển thị dữ liệu trước khi xóa hoặc bucket sau khi đã empty.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/s3-empty-bucket.png
-->

![Dọn dữ liệu trong S3 Bucket](/images/6-Cleanup/s3-empty-bucket.png)

---

## 16. Xóa S3 Bucket

Sau khi bucket đã trống, xóa hai bucket:

```bash
aws s3api delete-bucket \
  --bucket local-aqi-dev-s3-raw \
  --region ap-southeast-1
```

```bash
aws s3api delete-bucket \
  --bucket local-aqi-dev-s3-processed \
  --region ap-southeast-1
```

Chỉ xóa bucket sau khi đã xác nhận:

* Dữ liệu telemetry cần thiết đã được sao lưu.
* Dataset processed đã được tải về.
* Model artifact đã được lưu.
* Không còn SageMaker Job hoặc ứng dụng nào sử dụng bucket.

Nếu cần tiếp tục demo hoặc viết báo cáo, có thể giữ lại bucket trong thời gian ngắn và chỉ xóa các tài nguyên compute trước.

---

## 17. Xóa CloudWatch Log Group

Các dịch vụ như Firehose, EC2, SageMaker và ứng dụng backend có thể tạo CloudWatch Log Group.

Truy cập:

```text
Amazon CloudWatch
→ Logs
→ Log groups
```

Tìm log group theo tên dịch vụ hoặc project tag.

Có thể xóa bằng AWS CLI:

```bash
aws logs delete-log-group \
  --log-group-name <LOG_GROUP_NAME>
```

Nếu vẫn cần log để viết báo cáo, có thể giữ lại log group nhưng cấu hình retention ngắn hơn, ví dụ 7 ngày:

```bash
aws logs put-retention-policy \
  --log-group-name <LOG_GROUP_NAME> \
  --retention-in-days 7
```

Việc đặt retention giúp tránh log được lưu vô thời hạn.

---

## 18. Xóa IAM Role và Inline Policy

IAM Role nên được xóa sau khi các dịch vụ sử dụng role đã được xóa.

Các role chính của dự án có thể bao gồm:

```text
local-aqi-dev-iot-to-firehose
local-aqi-dev-firehose-to-s3
local-aqi-dev-sagemaker-execution-role
```

Trước khi xóa role cần:

1. Gỡ role khỏi dịch vụ.
2. Xóa inline policy.
3. Detach managed policy.
4. Xóa role.

Liệt kê inline policy:

```bash
aws iam list-role-policies \
  --role-name <ROLE_NAME>
```

Xóa inline policy:

```bash
aws iam delete-role-policy \
  --role-name <ROLE_NAME> \
  --policy-name <POLICY_NAME>
```

Liệt kê managed policy đã gắn:

```bash
aws iam list-attached-role-policies \
  --role-name <ROLE_NAME>
```

Gỡ managed policy:

```bash
aws iam detach-role-policy \
  --role-name <ROLE_NAME> \
  --policy-arn <POLICY_ARN>
```

Xóa role:

```bash
aws iam delete-role \
  --role-name <ROLE_NAME>
```

<!-- ẢNH CẦN CHỤP:
IAM > Roles, lọc theo local-aqi-dev.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/iam-project-roles.png
-->

![IAM Role của dự án](/images/6-Cleanup/iam-project-roles.png)

Không xóa role dùng chung với dự án khác.

---

## 19. Thu hồi quyền tạm thời của IAM User

Trong quá trình triển khai, một số thành viên có thể được cấp quyền tạm thời như:

* `iam:CreateRole`
* `iam:AttachRolePolicy`
* `iam:PutRolePolicy`
* `iam:PassRole`
* Quyền tạo hoặc xóa tài nguyên trong một số dịch vụ.

Sau khi hoàn thành công việc, người quản trị cần:

* Gỡ policy tạm thời.
* Xóa access key không còn sử dụng.
* Disable hoặc xóa IAM User của thành viên không còn tham gia.
* Kiểm tra MFA của tài khoản quản trị.
* Không để credentials trong source code hoặc GitHub repository.

Liệt kê access key:

```bash
aws iam list-access-keys \
  --user-name <IAM_USER_NAME>
```

Vô hiệu hóa access key:

```bash
aws iam update-access-key \
  --user-name <IAM_USER_NAME> \
  --access-key-id <ACCESS_KEY_ID> \
  --status Inactive
```

Sau khi xác nhận không còn sử dụng, xóa access key:

```bash
aws iam delete-access-key \
  --user-name <IAM_USER_NAME> \
  --access-key-id <ACCESS_KEY_ID>
```

---

## 20. Kiểm tra Security Group và tài nguyên mạng

Sau khi xóa EC2, kiểm tra các tài nguyên mạng còn tồn tại:

* Security Group.
* Elastic IP.
* Network Interface.
* EBS Volume.
* Load Balancer nếu có.
* Target Group nếu có.
* NAT Gateway nếu có.
* VPC Endpoint nếu có.

Dự án chỉ xóa các tài nguyên mạng được tạo riêng cho hệ thống AQI.

Không xóa:

* Default VPC.
* Default Security Group.
* Route Table mặc định.
* Internet Gateway dùng chung.
* Tài nguyên đang được dự án khác sử dụng.

Có thể kiểm tra Network Interface:

```bash
aws ec2 describe-network-interfaces
```

Kiểm tra EBS Volume chưa gắn:

```bash
aws ec2 describe-volumes \
  --filters Name=status,Values=available
```

---

## 21. Kiểm tra AWS Budgets và Billing

Sau khi cleanup, truy cập:

```text
AWS Billing and Cost Management
→ Bills
```

Kiểm tra chi phí theo từng dịch vụ.

Tiếp tục kiểm tra:

```text
AWS Billing and Cost Management
→ Budgets
```

Các budget cảnh báo có thể được giữ lại để giám sát tài khoản AWS trong những tháng tiếp theo.

<!-- ẢNH CẦN CHỤP:
AWS Billing > Bills hoặc Cost Explorer.
Hiển thị chi phí các dịch vụ sau khi cleanup.
Đề xuất đường dẫn ảnh:
/images/6-Cleanup/billing-check.png
-->

![Kiểm tra chi phí AWS](/images/6-Cleanup/billing-check.png)

Lưu ý rằng một số khoản phí có thể xuất hiện trễ so với thời điểm sử dụng. Do đó, cần tiếp tục kiểm tra Billing trong những ngày tiếp theo.

---

## 22. Thứ tự cleanup đề xuất

Để hạn chế lỗi phụ thuộc, các tài nguyên được dọn dẹp theo thứ tự:

```text
1. Sao lưu dữ liệu và kết quả cần thiết
2. Xóa SageMaker Endpoint
3. Xóa SageMaker Endpoint Configuration
4. Xóa SageMaker Model
5. Dừng hoặc xóa SageMaker Notebook/Studio Application
6. Kiểm tra Processing Job và Training Job
7. Dừng hoặc terminate EC2 Instance
8. Giải phóng Elastic IP và EBS không còn sử dụng
9. Xóa IoT Rule
10. Vô hiệu hóa và xóa IoT Certificate
11. Xóa AWS IoT Policy
12. Xóa Firehose Delivery Stream
13. Xóa SNS Subscription và SNS Topic
14. Xóa dữ liệu trong S3
15. Xóa S3 Bucket
16. Xóa CloudWatch Log Group
17. Xóa IAM Role và policy
18. Thu hồi quyền tạm thời của IAM User
19. Kiểm tra tài nguyên mạng còn sót
20. Kiểm tra Billing và Budgets
```

---

## 23. Checklist xác nhận cuối cùng

Sau khi hoàn thành cleanup, kiểm tra các mục sau:

* [ ] Không còn SageMaker Endpoint ở trạng thái `InService`.
* [ ] Không còn SageMaker Notebook hoặc Studio Application đang chạy.
* [ ] Không còn Processing Job hoặc Training Job ở trạng thái `InProgress`.
* [ ] EC2 Instance đã được stop hoặc terminate.
* [ ] Elastic IP không sử dụng đã được giải phóng.
* [ ] EBS Volume không sử dụng đã được xóa.
* [ ] IoT Rule đã được xóa.
* [ ] IoT Certificate đã được chuyển sang `Inactive` và xóa.
* [ ] IoT Policy không còn được gắn với certificate.
* [ ] Firehose Delivery Stream đã được xóa.
* [ ] SNS Topic không còn sử dụng đã được xóa.
* [ ] Dữ liệu S3 cần thiết đã được sao lưu.
* [ ] S3 Bucket không còn sử dụng đã được xóa.
* [ ] CloudWatch Log Group đã được xóa hoặc cấu hình retention.
* [ ] IAM Role của dự án đã được xóa.
* [ ] Quyền IAM tạm thời đã được thu hồi.
* [ ] Access key không còn sử dụng đã được vô hiệu hóa hoặc xóa.
* [ ] Không còn tài nguyên phát sinh chi phí ngoài kế hoạch.
* [ ] AWS Billing và Budgets đã được kiểm tra.

---

## 24. Kết quả

Sau khi thực hiện cleanup:

* Các tài nguyên compute không còn chạy.
* Luồng IoT Core đến Firehose đã được ngừng.
* Dữ liệu cần thiết đã được sao lưu.
* Tài nguyên S3 không còn sử dụng đã được xóa.
* IAM Role và các quyền tạm thời đã được thu hồi.
* Các tài nguyên mạng không còn sử dụng đã được kiểm tra.
* Nguy cơ phát sinh chi phí ngoài kế hoạch được giảm thiểu.
* Tài khoản AWS vẫn được giám sát bằng AWS Budgets.

Việc dọn dẹp tài nguyên là bước bắt buộc sau khi hoàn thành bài lab hoặc dự án thử nghiệm trên AWS. Ngoài việc kiểm soát chi phí, cleanup còn giúp tài khoản AWS dễ quản lý hơn và hạn chế các tài nguyên cũ gây nhầm lẫn trong những lần triển khai tiếp theo.
