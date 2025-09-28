# AWS Lambda Functions -- Guide with Examples & Use Cases

## 📌 Introduction

AWS Lambda is a **serverless compute service** that lets you run code
without provisioning or managing servers. You pay only for the compute
time you consume.

It automatically scales your application by running code in response to
**events** such as: - API Gateway requests - File uploads to S3 -
Database updates (DynamoDB Streams, RDS) - Scheduled cron jobs
(CloudWatch Events) - SNS/SQS messages

------------------------------------------------------------------------

## ⚡ How AWS Lambda Works

1.  **Event Source** (like API Gateway, S3, DynamoDB, etc.) triggers
    Lambda.
2.  **Lambda Function** executes your code (Node.js, Python, Java,
    etc.).
3.  **Execution Role (IAM)** defines what AWS services the Lambda can
    access.
4.  **Response** is sent back to the event source (if required).

------------------------------------------------------------------------

## 🛠 Lambda Function Structure (Node.js Example)

``` javascript
exports.handler = async (event) => {
  console.log("Event Received:", event);

  // Example logic
  const response = {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello from AWS Lambda!" }),
  };

  return response;
};
```

------------------------------------------------------------------------

## 🚀 Common AWS Lambda Functions & Examples

### 1. **Lambda with API Gateway (REST API)**

Use Case: Building a **serverless backend API**.

``` javascript
exports.handler = async (event) => {
  const name = event.queryStringParameters?.name || "Guest";
  return {
    statusCode: 200,
    body: JSON.stringify({ message: `Hello, ${name}!` }),
  };
};
```

👉 Example: A **serverless user registration API**.

------------------------------------------------------------------------

### 2. **Lambda with S3 (File Processing)**

Use Case: Automatically process files when uploaded.

``` javascript
exports.handler = async (event) => {
  const bucket = event.Records[0].s3.bucket.name;
  const key = event.Records[0].s3.object.key;
  console.log(`New file uploaded: ${bucket}/${key}`);
};
```

👉 Example: Resize images when uploaded to S3.

------------------------------------------------------------------------

### 3. **Lambda with DynamoDB (Database Trigger)**

Use Case: React to database changes.

``` javascript
exports.handler = async (event) => {
  event.Records.forEach((record) => {
    console.log("Change type:", record.eventName);
    console.log("New item:", record.dynamodb.NewImage);
  });
};
```

👉 Example: Update analytics when new order data is inserted.

------------------------------------------------------------------------

### 4. **Lambda with SNS (Notifications)**

Use Case: Process notifications or alerts.

``` javascript
exports.handler = async (event) => {
  event.Records.forEach((record) => {
    console.log("SNS Message:", record.Sns.Message);
  });
};
```

👉 Example: Send alerts when an error occurs in your app.

------------------------------------------------------------------------

### 5. **Lambda with SQS (Queue Processing)**

Use Case: Handle background jobs asynchronously.

``` javascript
exports.handler = async (event) => {
  for (const record of event.Records) {
    console.log("Message from SQS:", record.body);
  }
};
```

👉 Example: Process payment requests from a queue.

------------------------------------------------------------------------

### 6. **Lambda with CloudWatch Events (Cron Jobs)**

Use Case: Scheduled tasks.

``` javascript
exports.handler = async () => {
  console.log("Running scheduled job at", new Date().toISOString());
};
```

👉 Example: Clean up old database records every day.

------------------------------------------------------------------------

### 7. **Lambda with EventBridge**

Use Case: Event-driven architecture.

``` javascript
exports.handler = async (event) => {
  console.log("Event received:", event);
};
```

👉 Example: Trigger workflows when new users sign up.

------------------------------------------------------------------------

### 8. **Lambda with Step Functions**

Use Case: Complex workflows.

``` javascript
exports.handler = async (event) => {
  return { status: "step completed", input: event };
};
```

👉 Example: Orchestrate multi-step order fulfillment.

------------------------------------------------------------------------

## 🔗 Real-World Project Example

**Project:** *Image Processing & Notification System*

-   **S3** → Upload image\
-   **Lambda 1** → Resize image & save optimized copy\
-   **DynamoDB** → Store metadata\
-   **Lambda 2 (DynamoDB Stream)** → Update analytics\
-   **SNS** → Notify users of successful upload

Architecture:

    User Upload → S3 → Lambda → DynamoDB → Lambda → SNS

------------------------------------------------------------------------

## 📊 Benefits of AWS Lambda

-   **No servers** to manage
-   **Auto scaling**
-   **Cost-effective** (pay-per-use)
-   **Event-driven**
-   **Supports multiple languages**

------------------------------------------------------------------------

## 📂 Project Structure

    aws-lambda-examples/
    │── functions/
    │   ├── apiGateway.js
    │   ├── s3Processor.js
    │   ├── dynamoTrigger.js
    │   ├── snsHandler.js
    │   ├── sqsConsumer.js
    │   ├── cronJob.js
    │   └── stepFunction.js
    │── README.md

------------------------------------------------------------------------

## 🚀 Deployment (Terraform Example)

Here's how to create a Lambda using Terraform:

``` hcl
resource "aws_lambda_function" "example" {
  function_name = "example_lambda"
  runtime       = "nodejs18.x"
  handler       = "index.handler"
  role          = aws_iam_role.lambda_exec.arn
  filename      = "lambda.zip"
}

resource "aws_iam_role" "lambda_exec" {
  name = "lambda_exec_role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Action = "sts:AssumeRole",
      Effect = "Allow",
      Principal = {
        Service = "lambda.amazonaws.com"
      }
    }]
  })
}
```

------------------------------------------------------------------------

## ✅ Conclusion

AWS Lambda is the backbone of **serverless applications**.\
You can integrate it with **API Gateway, S3, DynamoDB, SNS, SQS,
EventBridge, Step Functions, and more** to build powerful event-driven
systems.
