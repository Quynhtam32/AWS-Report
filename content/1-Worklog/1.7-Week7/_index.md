---
title: "Week 7 Worklog"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Verify data lineage and ensure data consistency throughout the Data Pipeline.
* Integrate the Storage/Data component with Machine Learning and Backend services.
* Test the end-to-end workflow from data storage to prediction service.

### Tasks Completed This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review the end-to-end data flow from the Amazon S3 **raw** layer to the **processed** layer and document the overall data transformation pipeline. | 13/07/2026 | 13/07/2026 | System Design Document |
| 2 | Verify data lineage by tracing processed datasets back to their original sources and validating data consistency across each stage of the pipeline. | 14/07/2026 | 14/07/2026 | Project Documentation |
| 3 | Coordinate with the Machine Learning team to ensure the processed datasets satisfy the requirements for model training, validation, and future retraining. | 15/07/2026 | 15/07/2026 | Team Meeting Notes |
| 4 | Review the integration between Amazon S3 and Amazon SageMaker Endpoint, and validate the data flow between storage, inference, and backend services. | 16/07/2026 | 16/07/2026 | Amazon SageMaker Documentation |
| 5 | Perform end-to-end testing of the complete workflow from Amazon S3 → SageMaker Endpoint → Backend API, identify pipeline issues, and verify successful data processing. | 17/07/2026 | 17/07/2026 | System Test Report |

### Lessons Learned:

* Understood the importance of data lineage in maintaining reliable Data Pipelines.

* Learned how to monitor and validate data flow between different AWS services.

* Gained practical experience in integrating cloud storage with Machine Learning services.

* Improved understanding of the complete workflow:
  * Data collection.
  * Data storage.
  * Data processing.
  * Machine Learning prediction.
  * Application integration.