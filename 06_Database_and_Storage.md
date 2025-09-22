# Database & Storage

## Q1: RDS vs DynamoDB.
- **RDS** → Relational (SQL). Supports MySQL, Postgres, Oracle, etc.  
- **DynamoDB** → NoSQL, key-value/document store, serverless, auto-scaling.  

---

## Q2: Hands-on – Launch RDS (CLI).
```bash
aws rds create-db-instance   --db-instance-identifier mydb   --db-instance-class db.t3.micro   --engine mysql   --allocated-storage 20   --master-username admin   --master-user-password password123
```

---

## Q3: DynamoDB Example (CLI).
```bash
aws dynamodb create-table   --table-name Users   --attribute-definitions AttributeName=UserId,AttributeType=S   --key-schema AttributeName=UserId,KeyType=HASH   --billing-mode PAY_PER_REQUEST
```

---

## Q4: S3 vs EFS vs EBS.
- **S3** → Object storage, highly scalable.  
- **EFS** → Network file system, shared storage.  
- **EBS** → Block storage, attached to EC2 instances.  

---

## Q5: Scenario – You need high-throughput OLTP workload.
**Answer:**  
- Use **Aurora (MySQL/Postgres compatible)** for high performance.  
- Enable **Aurora Replicas** for read scaling.  
- Use **Global Database** for multi-region replication.  
