---
title: "Building the Data Pipeline"
date: 2026-07-31
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Building the Data Pipeline

This section describes the implementation of the **Data Pipeline** for the **Local AQI Forecasting & Alert System** project.

Environmental data (PM2.5, temperature, and humidity) generated from simulated IoT devices is collected, validated, and transformed before being stored in an Amazon S3 Data Lake. The processed dataset is then prepared in a machine learning–ready format for subsequent forecasting tasks.

It is organized into four sub-sections:

1. **Create the Amazon S3 Data Lake**
2. **Configure Amazon Kinesis Data Firehose**
3. **Validate Raw Data**
4. **Preprocess and Export the Dataset**

In particular:

- `1. Create the Amazon S3 Data Lake` explains how to create and configure the central S3 bucket used for storing raw and processed datasets.
- `2. Configure Amazon Kinesis Data Firehose` describes how to configure Firehose to ingest streaming data from AWS IoT Core and deliver it to Amazon S3.
- `3. Validate Raw Data` demonstrates the validation process to ensure that incoming sensor data conforms to the required schema and quality standards.
- `4. Preprocess and Export the Dataset` covers data cleaning, hourly resampling, feature preparation, and exporting the processed dataset in Apache Parquet format for downstream Machine Learning workflows.

### Navigation

- [5.5.1. Create the Amazon S3 Data Lake](5.5.1-S3-Data-Lake/)
- [5.5.2. Configure Amazon Kinesis Data Firehose](5.5.2-Firehose/)
- [5.5.3. Validate Raw Data](5.5.3-Data_Validation/)
- [5.5.4. Preprocess and Export the Dataset](5.5.4-Data-Processing/)