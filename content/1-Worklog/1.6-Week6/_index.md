---
title: "Week 6 Worklog"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Optimize the data storage structure on Amazon S3 to improve query performance.
* Standardize time-series data for Machine Learning model training.
* Learn methods to optimize AWS storage costs through data lifecycle management.

### Tasks Completed This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Study Amazon S3 data partitioning strategies and learn how to organize datasets by monitoring station and timestamp to improve query performance. | 06/07/2026 | 06/07/2026 | Amazon S3 Documentation, AWS Best Practices |
| 2 | Redesign the AQI data partition structure by applying partitioning based on `station_id` and timestamp for efficient data management. | 07/07/2026 | 07/07/2026 | System Design Document |
| 3 | Standardize time-series data collected from monitoring stations and validate the dataset format for Amazon SageMaker DeepAR model training. | 08/07/2026 | 08/07/2026 | Amazon SageMaker DeepAR Documentation |
| 4 | Study Amazon S3 Storage Classes and advanced Lifecycle policies to understand automatic data transition and storage cost optimization. | 09/07/2026 | 09/07/2026 | Amazon S3 Storage Classes Documentation |
| 5 | Configure Amazon S3 Lifecycle policies and test automatic transitions between S3 Standard, S3 Standard-IA, and S3 Glacier storage classes. | 10/07/2026 | 10/07/2026 | Amazon S3 Lifecycle Documentation |

### Lessons Learned:

* Understood the importance of proper data organization in building an efficient Data Lake.

* Learned how data partitioning helps:
  * Reduce query processing time.
  * Improve Machine Learning data processing efficiency.
  * Manage datasets more effectively.

* Understood how to optimize AWS storage costs through:
  * Selecting suitable Storage Classes.
  * Applying Lifecycle Policy for automatic data management.

* Gained more experience in preparing time-series datasets for air quality forecasting models.