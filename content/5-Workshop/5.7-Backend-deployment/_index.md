---
title : "Deploying the FastAPI Backend"
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Overview

In this section, you will deploy the Backend of the **Local AQI Forecasting & Alert System** to Amazon EC2.

The Backend is built with FastAPI and performs the following functions:

- Provides an API for checking system status.
- Receives requests to register AQI alert thresholds.
- Stores subscription information and cooldown status in Amazon DynamoDB.
- Calls the SageMaker Endpoint to retrieve forecast results.
- Sends alert emails through Amazon SNS.

Backend workflow:

```text
User
    → FastAPI on Amazon EC2
        → Amazon DynamoDB
        → Amazon SageMaker Endpoint
        → Amazon SNS
```

The Backend uses the EC2's IAM role to access AWS services. The application runs under `systemd` so that it restarts automatically when EC2 boots or if the process fails.

<!-- Add image: Backend deployment architecture diagram -->

#### Contents

- [Preparing EC2 and IAM Role](5.7.1-prepare/)
- [Installing and Configuring the Backend](5.7.2-install/)
- [Starting the Backend with systemd](5.7.3-systemd/)
- [Testing the API and Alert Cycle](5.7.4-test/)