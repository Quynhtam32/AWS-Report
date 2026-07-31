---
title: "Monitoring & Quality Assurance"
date: 2026-07-31
weight: 3
chapter: false
pre: "<b>3. </b>"
---

# Monitoring & Quality Assurance

The monitored components include AWS IoT Core, Firehose, S3, SageMaker, the backend API, and Amazon SNS.

The end-to-end validation flow is:

```text
Simulator → IoT Core → Firehose → S3 Raw → Processing → S3 Processed → ML → Backend → SNS
```

Each test records its input, expected result, actual result, status, evidence, and owner.
