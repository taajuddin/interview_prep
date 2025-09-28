# AWS Lambda + SNS Example with Node.js and Terraform

This guide explains how to integrate **AWS Lambda** with **SNS (Simple Notification Service)** using Node.js and Terraform.  
We’ll create:
- An SNS topic
- A Lambda function subscribed to that topic
- An email subscription to the same topic (multi-subscriber)
- Terraform code to provision everything

---

## 📌 1. Concepts

- **AWS SNS (Simple Notification Service):** Pub/Sub messaging service that allows you to send messages to multiple subscribers (e.g., Lambda, Email, SQS).  
- **AWS Lambda:** Serverless compute service that runs code in response to events.  

👉 In this setup:
- An **SNS topic** is created.  
- A **Lambda function** subscribes to that topic.  
- An **Email subscription** is added.  
- Whenever a message is published, both the Lambda and Email are triggered.  

---

## 📌 2. Node.js Lambda Function

`index.js`:

```javascript
exports.handler = async (event) => {
  console.log("Received SNS event:", JSON.stringify(event, null, 2));

  for (const record of event.Records) {
    const snsMessage = record.Sns.Message;
    console.log("Message from SNS:", snsMessage);

    if (snsMessage.includes("ALERT")) {
      console.log("⚠️ ALERT received! Take immediate action.");
    }
  }

  return {
    statusCode: 200,
    body: "Message processed successfully",
  };
};
```

Zip the file before deploying (if uploading manually):

```sh
zip function.zip index.js
```

---

## 📌 3. Terraform Code

`main.tf`:

```hcl
provider "aws" {
  region = "us-east-1"
}

# 1. Create SNS topic
resource "aws_sns_topic" "demo_topic" {
  name = "demo-sns-topic"
}

# 2. Create IAM role for Lambda
resource "aws_iam_role" "lambda_role" {
  name = "lambda_sns_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "lambda.amazonaws.com"
      }
    }]
  })
}

# 3. Attach policy for Lambda logging
resource "aws_iam_role_policy_attachment" "lambda_logging" {
  role       = aws_iam_role.lambda_role.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
}

# 4. Create Lambda function
resource "aws_lambda_function" "sns_lambda" {
  function_name = "snsLambdaHandler"
  runtime       = "nodejs18.x"
  handler       = "index.handler"
  role          = aws_iam_role.lambda_role.arn
  filename      = "function.zip" # Pre-zipped lambda package
}

# 5. Allow SNS to invoke Lambda
resource "aws_lambda_permission" "allow_sns" {
  statement_id  = "AllowExecutionFromSNS"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.sns_lambda.function_name
  principal     = "sns.amazonaws.com"
  source_arn    = aws_sns_topic.demo_topic.arn
}

# 6. Subscribe Lambda to SNS topic
resource "aws_sns_topic_subscription" "lambda_subscription" {
  topic_arn = aws_sns_topic.demo_topic.arn
  protocol  = "lambda"
  endpoint  = aws_lambda_function.sns_lambda.arn
}

# 7. Subscribe Email to SNS topic
resource "aws_sns_topic_subscription" "email_subscription" {
  topic_arn = aws_sns_topic.demo_topic.arn
  protocol  = "email"
  endpoint  = "your-email@example.com" # Change to your email
}
```

---

## 📌 4. How It Works

1. Deploy with Terraform:

```sh
terraform init
terraform apply
```

2. Terraform provisions:
   - SNS Topic (`demo-sns-topic`)
   - Lambda function (`snsLambdaHandler`)
   - Lambda + Email subscriptions

3. Confirm email subscription:  
   Check your email inbox and **confirm** the subscription link.

4. Publish a message:

```sh
aws sns publish   --topic-arn arn:aws:sns:us-east-1:123456789012:demo-sns-topic   --message "Hello from SNS!"
```

5. Results:
   - **Lambda** logs:  
     ```
     Message from SNS: Hello from SNS!
     ```
   - **Email** received:  
     ```
     Subject: AWS Notification - demo-sns-topic
     Message: Hello from SNS!
     ```

---

## ✅ Summary

With this setup:
- An **SNS topic** broadcasts messages.  
- Both **Lambda** and **Email** subscribers receive the message.  
- Easily extendable to **SQS, HTTP, SMS**.  

This is a powerful serverless integration pattern for event-driven architectures.
