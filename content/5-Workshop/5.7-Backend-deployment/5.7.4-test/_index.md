---
title : "API Testing and Alert Cycle"
weight : 4
chapter : false
pre : " <b> 5.7.4 </b> "
---

#### Test the Backend from your personal machine

Get the current public IPv4 address of the EC2 instance. This address may change when the instance is stopped and restarted.

Create the `BASE_URL` variable:

```bash
BASE_URL=http://<PUBLIC_IP>:8000
```

Check the health endpoint:

```bash
curl -i "$BASE_URL/health"
```

Open Swagger UI:

```text
http://<PUBLIC_IP>:8000/docs
```

<!-- Add image: Swagger UI interface -->

#### Test the forecast API

Call the API:

```bash
curl -i "$BASE_URL/forecast/station-01"
```

Once the SageMaker Endpoint and station data are correctly configured, the API returns:

- The timestamp of the source data.
- The forecast timestamp.
- The forecast interval.
- The forecasted PM2.5.
- The AQI index.

If the endpoint or source data has not been configured, the API returns an HTTP `503` instead of generating a fake forecast result.

<!-- Add image: Forecast API call result -->

#### Test alert subscription

Send a subscription request:

```bash
curl -i -X POST "$BASE_URL/subscribe/" \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "APPROVED_TEST_EMAIL",
    "station_id": "station-01",
    "threshold_aqi": 150
  }'
```

Amazon SNS sends a confirmation email. Open the email and select **Confirm subscription**.

Only use an address that has consented to receive emails. Do not include real email addresses in the repository, logs, or screenshots.

<!-- Add image: Subscription status with email redacted -->

#### Test sending an alert

Send a PM2.5 value:

```bash
curl -i -X POST "$BASE_URL/alert/" \
  -H 'Content-Type: application/json' \
  -d '{
    "station_id": "station-01",
    "pm25": 55.4
  }'
```

The Backend performs the following:

1. Converts PM2.5 to AQI.
2. Finds confirmed subscribers.
3. Checks the alert threshold.
4. Checks the cooldown period.
5. Publishes a notification to Amazon SNS when conditions are met.

<!-- Add image: Alert email with recipient address redacted -->

#### Configure the automatic cycle

Only enable the timer once the SageMaker Endpoint, station data, and IAM permissions have been verified.

Install the service and timer:

```bash
cd /opt/local-aqi-backend

sudo install -m 0644 deploy/local-aqi-forecast-cycle.service \
  /etc/systemd/system/

sudo install -m 0644 deploy/local-aqi-forecast-cycle.timer \
  /etc/systemd/system/

sudo systemctl daemon-reload
sudo systemctl enable --now local-aqi-forecast-cycle.timer
```

Check the run schedule:

```bash
sudo systemctl list-timers local-aqi-forecast-cycle.timer
sudo systemctl status local-aqi-forecast-cycle.timer --no-pager
```

Check the most recent run:

```bash
sudo systemctl status local-aqi-forecast-cycle.service --no-pager
sudo journalctl -u local-aqi-forecast-cycle.service -n 100 --no-pager
```

<!-- Add image: systemd timer and most recent run -->

#### Completion

You have completed the following:

- Deployed the FastAPI Backend on Amazon EC2.
- Allowed the Backend to access DynamoDB, SageMaker, and SNS using an IAM role.
- Launched the application using `systemd`.
- Tested the health, forecast, subscribe, and alert APIs.
- Configured the automatic forecast and alert cycle.

When you don't need to run the demo, stop the EC2 instance and SageMaker Endpoint to limit unnecessary costs.