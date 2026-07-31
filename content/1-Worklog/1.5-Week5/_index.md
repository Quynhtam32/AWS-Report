---
title: "Week 5 Worklog"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Implement the data preprocessing workflow for AQI data collected from OpenAQ.
* Learn how to clean and transform raw data before storing it in the processed zone.
* Explore Amazon Kinesis Data Firehose for streaming data ingestion and integration with Amazon S3.

### Tasks Completed This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review the OpenAQ raw dataset structure, analyze the data format, and identify missing values, duplicate records, and abnormal measurements. | 29/06/2026 | 29/06/2026 | OpenAQ Dataset, Project Documentation |
| 2 | Study the data preprocessing workflow on AWS and explore Amazon SageMaker Processing Jobs for running data cleaning tasks. | 30/06/2026 | 30/06/2026 | Amazon SageMaker Documentation |
| 3 | Develop a preprocessing program for AQI data to handle missing values, detect outliers, and prepare the dataset for further processing. | 01/07/2026 | 01/07/2026 | Project Documentation |
| 4 | Upload the cleaned dataset to the **processed** layer in Amazon S3 and verify that the data conforms to the designed AQI schema. | 02/07/2026 | 02/07/2026 | Amazon S3 Documentation, System Design Document |
| 5 | Study Amazon Kinesis Data Firehose, including streaming data ingestion and its integration with Amazon S3 Data Lake. | 03/07/2026 | 03/07/2026 | Amazon Kinesis Data Firehose Documentation |
| 6 | Create a test Kinesis Data Firehose delivery stream, publish sample streaming data, and verify successful delivery to Amazon S3. | 04/07/2026 | 04/07/2026 | Amazon Kinesis Data Firehose Documentation |

### Lessons Learned:

* Understood the importance of data preprocessing before applying Machine Learning models.

* Learned how AWS services can be combined to build a scalable data pipeline:
  * Amazon S3 for data storage.
  * SageMaker Processing Job for data transformation.
  * Kinesis Data Firehose for streaming data ingestion.

* Gained practical experience in maintaining data consistency between different pipeline components.

* Improved understanding of how raw data can be transformed into high-quality datasets for analysis and prediction.