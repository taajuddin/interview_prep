# Security & IAM

## Q1: What is IAM and its key components?
- **Users** → Individual accounts.  
- **Groups** → Collection of users.  
- **Roles** → Temporary credentials for AWS services.  
- **Policies** → JSON documents defining permissions.  

**Example IAM Policy (JSON):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:ListBucket"],
    "Resource": ["arn:aws:s3:::my-bucket"]
  }]
}
```

---

## Q2: Hands-on – Create IAM User with CLI
```bash
aws iam create-user --user-name dev-user
aws iam attach-user-policy   --user-name dev-user   --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

---

## Q3: Difference between KMS and Secrets Manager
- **KMS** → Key Management Service (create/manage encryption keys).  
- **Secrets Manager** → Store, rotate, and retrieve secrets (DB passwords, API keys).  

---

## Q4: Scenario – Secure cross-account S3 bucket access.
**Answer:**  
- Use **Bucket Policy** to allow cross-account role access.  
- Enable **IAM Role assumption** with trust policies.  

**Bucket Policy Example:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::111122223333:role/CrossAccountRole"},
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::mybucket",
      "arn:aws:s3:::mybucket/*"
    ]
  }]
}
```

---

## Q5: Scenario – How to enforce MFA for sensitive actions?
- Create an **IAM Policy** requiring MFA condition:  
```json
"Condition": { "Bool": { "aws:MultiFactorAuthPresent": "true" } }
```  
- Attach it to IAM users/roles.  
