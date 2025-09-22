# Monitoring & Logging

## Q1: What is CloudWatch?
- Monitoring service for AWS resources and applications.  
- Features: metrics, logs, alarms, dashboards.  

**Example – Create Alarm (CLI):**
```bash
aws cloudwatch put-metric-alarm   --alarm-name HighCPU   --metric-name CPUUtilization   --namespace AWS/EC2   --statistic Average   --period 300   --threshold 80   --comparison-operator GreaterThanThreshold   --dimensions Name=InstanceId,Value=i-1234567890abcdef0   --evaluation-periods 2   --alarm-actions arn:aws:sns:us-east-1:123456789012:NotifyMe
```

---

## Q2: CloudTrail vs CloudWatch.
- **CloudTrail** → Logs all **API calls**. Auditing & compliance.  
- **CloudWatch** → Logs **metrics & application performance**. Monitoring & alerting.  

---

## Q3: Hands-on – Enable CloudTrail.
```bash
aws cloudtrail create-trail --name orgTrail --s3-bucket-name mylogs-bucket
aws cloudtrail start-logging --name orgTrail
```

---

## Q4: Scenario – Detect suspicious IAM activity.
**Answer:**  
- Enable **CloudTrail** for IAM actions.  
- Create **CloudWatch Alarms** for unusual API calls (e.g., `CreateUser`).  
- Send alerts via **SNS**.  
