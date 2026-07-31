---
title : "Starting the Backend with systemd"
weight : 3
chapter : false
pre : " <b> 5.7.3 </b> "
---

#### Check the service

The bootstrap script has created the file:

```text
/etc/systemd/system/local-aqi-backend.service
```

Check its contents:

```bash
sudo systemctl cat local-aqi-backend
```

The service uses the following configuration:

```ini
[Unit]
Description=Local AQI FastAPI backend
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=ec2-user
Group=ec2-user
WorkingDirectory=/opt/local-aqi-backend
EnvironmentFile=-/opt/local-aqi-backend/.env
ExecStart=/opt/local-aqi-backend/.venv/bin/uvicorn app.main:app --host 0.0.0.0 --port 8000
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

#### Start the Backend

Reload the `systemd` configuration:

```bash
sudo systemctl daemon-reload
```

Enable the service to start automatically with EC2:

```bash
sudo systemctl enable local-aqi-backend
```

Start the service:

```bash
sudo systemctl start local-aqi-backend
```

Check the status:

```bash
sudo systemctl status local-aqi-backend --no-pager
```

Expected result:

```text
Active: active (running)
```

<!-- Add image: Service in active (running) state -->

#### Check the logs

Show the last 100 log lines:

```bash
sudo journalctl -u local-aqi-backend -n 100 --no-pager
```

Follow the logs in real time:

```bash
sudo journalctl -u local-aqi-backend -f
```

The logs must not contain emails, credentials, access keys, or sensitive payloads.

#### Check the health endpoint

Call the API directly from EC2:

```bash
curl -i http://127.0.0.1:8000/health
```

Expected result:

```text
HTTP/1.1 200 OK
```

```json
{"status":"ok"}
```

<!-- Add image: Result of calling /health on EC2 -->

If the API is not working, check the following:

```bash
sudo systemctl status local-aqi-backend --no-pager
sudo journalctl -u local-aqi-backend -n 100 --no-pager
sudo ss -lntp | grep 8000
```