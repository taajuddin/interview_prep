# Advanced Topics

## Q1: Explain AWS Global Accelerator vs CloudFront.
- **CloudFront** → Optimizes content delivery (CDN).  
- **Global Accelerator** → Improves performance for non-HTTP apps by routing traffic via AWS backbone.  

---

## Q2: Hands-on – SQS Example.
```bash
aws sqs create-queue --queue-name myQueue
aws sqs send-message --queue-url <QUEUE_URL> --message-body "Hello"
aws sqs receive-message --queue-url <QUEUE_URL>
```

---

## Q3: SNS vs SQS vs Kinesis.
- **SNS** → Pub/Sub messaging.  
- **SQS** → Queue-based, async messaging.  
- **Kinesis** → Real-time streaming data.  

---

## Q4: Scenario – Process 1M messages per hour.
**Answer:**  
- Use **SQS** for buffering.  
- Trigger **Lambda** to process.  
- For real-time analytics, use **Kinesis + Firehose + Redshift**.  
