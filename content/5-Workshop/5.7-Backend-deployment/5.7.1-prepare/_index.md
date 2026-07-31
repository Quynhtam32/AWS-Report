---
title : "Preparing EC2 and IAM Role"
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

#### Create IAM Role for Backend

1. Access **AWS Management Console**.
2. Search for and open the **IAM** service.
3. In the left navigation panel, select **Roles**.
4. Select **Create role**.
5. Under **Trusted entity type**, choose **AWS service**.
6. Under **Use case**, select **EC2**.
7. Click **Next**.

<!-- Add image: EC2 trusted entity selection screen -->

Attach the `AmazonSSMManagedInstanceCore` policy to allow EC2 management through AWS Systems Manager Session Manager.

Next, create the runtime policy for the Backend application:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SubscriberTable",
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:Scan"
      ],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:ACCOUNT_ID:table/local-aqi-subscribers-dev"
    },
    {
      "Sid": "AlertTopic",
      "Effect": "Allow",
      "Action": [
        "sns:GetSubscriptionAttributes",
        "sns:ListSubscriptionsByTopic",
        "sns:Publish",
        "sns:Subscribe"
      ],
      "Resource": "arn:aws:sns:ap-southeast-1:ACCOUNT_ID:local-aqi-alerts-dev"
    },
    {
      "Sid": "ForecastEndpoint",
      "Effect": "Allow",
      "Action": "sagemaker:InvokeEndpoint",
      "Resource": "arn:aws:sagemaker:ap-southeast-1:ACCOUNT_ID:endpoint/ENDPOINT_NAME"
    }
  ]
}
```

Replace `ACCOUNT_ID` and `ENDPOINT_NAME` with the values for your deployment environment.

Name the role:

```text
local-aqi-backend-ec2-role
```

Select **Create role** to finish.

<!-- Add image: IAM role after successful creation -->

#### Create Security Group

1. Open the **Amazon EC2** service.
2. In the left navigation panel, select **Security Groups**.
3. Select **Create security group**.
4. Enter the name:

```text
local-aqi-backend-sg
```

5. Select the VPC used for the project.
6. Under **Inbound rules**, add the following rule:

| Type | Protocol | Port range | Source |
| --- | --- | --- | --- |
| Custom TCP | TCP | `8000` | My IP or a CIDR allowed for the demo |

Do not open SSH port `22`. Server administration is done through Session Manager.

<!-- Add image: TCP 8000 inbound rule -->

#### Launch EC2

1. In the **Amazon EC2** console, select **Instances**.
2. Select **Launch instances**.
3. Enter the name:

```text
local-aqi-dev-ec2-backend
```

4. Choose Amazon Linux as the AMI.
5. Choose an instance type suitable for the test environment, e.g. `t3.micro`.
6. Under **Key pair**, select **Proceed without a key pair**.
7. Select the `local-aqi-backend-sg` security group.
8. Under **Advanced details**, select the IAM instance profile corresponding to the `local-aqi-backend-ec2-role` role.
9. Enable the requirement to use **IMDSv2**.
10. Configure `gp3` EBS storage, enable encryption, and select delete-on-termination for the volume.

Add the following tags:
```text
Project=local-aqi-forecasting
Environment=dev
Owner=quang-tuan
Module=backend
```

Select **Launch instance**.

<!-- Add image: EC2 in Running state -->

Wait for the instance to reach the `Running` state and appear in Systems Manager before moving on to the next section.