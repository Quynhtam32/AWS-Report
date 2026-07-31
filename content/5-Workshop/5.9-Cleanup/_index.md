---
title : "Machine Learning: training and forecast generation"
date : 2026-07-31
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Role objective

The Machine Learning role uses the processed dataset to train a PM2.5 forecasting model and generate forecast outputs for the next 24 hours.

#### Technical goals

+ Read datasets from `local-aqi-dev-s3-processed`
+ Split train / validation / test by time
+ Train an appropriate time-series model
+ Evaluate results with metrics such as MAE and RMSE
+ Save the model artifact and forecast output

#### Writing template for this section

Once the team provides real implementation details and evidence, this section will be written as:

1. Prepare train, validation, and test datasets.
2. Explain why random splitting is not used.
3. Configure the SageMaker training job or local fallback training if quota is not available.
4. Review training logs, model artifacts, and evaluation metrics.
5. Produce forecast result files for the API and alerting flow.

#### Outcomes that must be proven

+ There is real evidence of a training job or local training run.
+ Evaluation metrics are clearly reported.
+ There is a forecast result for at least one station.
+ There is a model artifact or output file that can be reused by the Backend.

{{% notice note %}}
The detailed Machine Learning content will be added after the team shares the final training job details, metrics, and forecast outputs.
{{% /notice %}}
