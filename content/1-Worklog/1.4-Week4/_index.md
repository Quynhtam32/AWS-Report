---
title: "Week 4 Worklog"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Start implementing the Storage/Data component of the project.
* Learn advanced Amazon S3 features and apply them to build a Data Lake.
* Design the AQI data schema and prepare data processing workflow for the Machine Learning pipeline.

### Tasks Completed This Week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Study advanced Amazon S3 concepts, including bucket architecture, object organization, storage classes, and best practices for data management. | 22/06/2026 | 22/06/2026 | Amazon S3 Documentation |
| 2 | Research the Data Lake architecture on AWS and design the S3 folder structure for Raw, Processed, ML, Models, and Monitoring data. | 23/06/2026 | 23/06/2026 | AWS Well-Architected Framework, Amazon S3 Documentation |
| 3 | Practice Amazon S3 Versioning and Lifecycle policies to understand data protection and storage cost optimization. | 24/06/2026 | 24/06/2026 | Amazon S3 Versioning & Lifecycle Documentation |
| 4 | Design the initial AQI dataset schema, including device information, timestamps, PM2.5, PM10, temperature, humidity, and other required attributes for the project. | 25/06/2026 | 25/06/2026 | System Design Document |
| 5 | Explore Amazon SageMaker Processing Jobs and perform a basic experiment to read and process sample datasets stored in Amazon S3. | 26/06/2026 | 26/06/2026 | Amazon SageMaker Documentation |

### Lessons Learned:

* Understood the role of Amazon S3 as the foundation for building a Data Lake.

* Learned that organizing data into different zones helps:
  * Improve data management.
  * Support future data processing workflows.
  * Make machine learning pipelines easier to maintain.

* Gained experience in preparing structured data before applying machine learning models.