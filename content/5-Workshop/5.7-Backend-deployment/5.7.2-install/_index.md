---
title : "Backend Installation and Configuration"
weight : 2
chapter : false
pre : " <b> 5.7.2 </b> "
---

#### Connect to EC2

1. Open the **Amazon EC2** service.
2. Select the instance `local-aqi-dev-ec2-backend`.
3. Select **Connect**.
4. Select the **Session Manager** tab.
5. Select **Connect**.

<!-- Add image: Session Manager connection session -->

Check the Python version:

```bash
python3 --version
```

Install the required tools:

```bash
sudo dnf install -y python3-pip git rsync
```

#### Download the source code

Navigate to the EC2 user's home directory:

```bash
cd /home/ec2-user
```

Clone the repository:

```bash
git clone <REPOSITORY_URL> AWS-FCJ-local_aqi_forecast
cd AWS-FCJ-local_aqi_forecast
```

Run the bootstrap script to create the application directory and the `systemd` service:

```bash
sudo bash backend/deploy/ec2-user-data.sh
```

Sync the Backend source code into `/opt/local-aqi-backend`:

```bash
sudo rsync -a --delete \
  --exclude '.env' \
  --exclude '.venv' \
  backend/ /opt/local-aqi-backend/

sudo chown -R ec2-user:ec2-user /opt/local-aqi-backend
```

<!-- Add image: Directory structure of /opt/local-aqi-backend -->

#### Install dependencies

Navigate to the Backend directory:

```bash
cd /opt/local-aqi-backend
```

Create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

#### Configure environment variables

Create the `.env` file:

```bash
cp .env.example .env
chmod 600 .env
```

Open the file:

```bash
nano .env
```

Update the content:

```dotenv
AWS_REGION=ap-southeast-1
SUBSCRIBERS_TABLE=local-aqi-subscribers-dev
ALERTS_TOPIC_ARN=arn:aws:sns:ap-southeast-1:ACCOUNT_ID:local-aqi-alerts-dev

SAGEMAKER_ENDPOINT_NAME=ENDPOINT_NAME
SAGEMAKER_CONTENT_TYPE=application/json
SAGEMAKER_TIMEOUT_SECONDS=5
SAGEMAKER_MAX_ATTEMPTS=3
FORECAST_HORIZON_HOURS=24

STATION_DATA_FILE=/opt/local-aqi-backend/runtime/stations.json
ALLOW_SAMPLE_STATION_DATA=false
ALERT_COOLDOWN_SECONDS=3600
```

Replace `ACCOUNT_ID` and `ENDPOINT_NAME` with the actual values.

Only configure `SAGEMAKER_ENDPOINT_NAME` once the endpoint is in the `InService` state and the request/response format has been agreed upon with the ML team.

Do not store AWS Access Keys, Secret Access Keys, or personal email addresses in the `.env` file.

<!-- Add image: Configuration file with Account ID and ARN redacted -->

#### Verify the source code

Run the test suite:

```bash
python -m unittest discover -s tests -v
```

Check syntax:

```bash
python -m compileall -q app tests
```

Validate the OpenAPI documentation:

```bash
python scripts/validate_openapi.py
```

If all commands complete successfully, proceed to the step of launching the Backend.

<!-- Add image: Test run and OpenAPI validation results -->