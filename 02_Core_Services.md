# Core AWS Services

## Q1: Explain EC2 and its pricing models.
- **EC2** → Elastic Compute Cloud (virtual servers).  
- Pricing Models:  
  - **On-Demand** – pay hourly/second.  
  - **Reserved Instances** – 1–3 year commitment, big discount.  
  - **Spot Instances** – unused capacity, up to 90% cheaper.  
  - **Savings Plans** – flexible compute savings.  

**CLI Example:**
```bash
aws ec2 describe-instances --region us-east-1
```

---

## Q2: S3 – Storage Classes
- **Standard** – frequently accessed.  
- **IA (Infrequent Access)** – lower cost, retrieval fees.  
- **Glacier / Glacier Deep Archive** – archival storage.  

**Terraform Example:**
```hcl
resource "aws_s3_bucket" "mybucket" {
  bucket = "my-app-bucket"
  acl    = "private"
}
```

---

## Q3: Load Balancer Types
- **ALB** (Application) – Layer 7, HTTP/HTTPS, path-based routing.  
- **NLB** (Network) – Layer 4, TCP/UDP, ultra-low latency.  
- **CLB** (Classic) – Legacy.  

**CloudFormation Example:**
```yaml
Resources:
  MyLoadBalancer:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties:
      Type: application
```

---

## Q4: Scenario – You need to handle millions of image uploads per day.
**Answer:**  
- Use **S3** for storage.  
- Enable **S3 Multipart Upload**.  
- Add **CloudFront** for global caching.  
- Trigger **Lambda** for processing (e.g., image resizing).  
