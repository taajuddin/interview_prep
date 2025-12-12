# AWS Lambda -- Complete Guide

## Introduction

AWS Lambda is a serverless computing service that lets you run code in
response to events without managing servers. You upload your code and
AWS handles execution, scaling, and billing only for compute time
consumed.

## Event-Driven Execution

Lambda runs automatically when triggered by events such as: - S3 (file
upload) - DynamoDB (table changes) - API Gateway (HTTP requests) -
CloudWatch Events (scheduled triggers)

## Automatic Scaling

AWS Lambda scales instantly based on incoming requests with no
configuration needed.

## Pay-as-you-go

You only pay for execution time (in milliseconds). No charge for idle
time.

## Limitations

-   Max execution time: 15 minutes\
-   Stateless execution (no memory between calls)\
-   Cold start delays after inactivity

## When to Use Lambda

-   Image processing (resize, compress on upload)
-   Data transformation before saving to DB
-   Real-time notifications (email/SMS on new events)

## Architecture Diagram

            +-------------+
            |   Client    |
            +------+------+
                   |
          (HTTP Request)
                   |
            +------v------+
            | API Gateway |
            +------+------+
                   |
            +------v------+
            |  Lambda     |
            |  Function   |
            +------+------+
                   |
              (Process Data)
                   |
            +------v------+
            |   S3 / DB   |
            +-------------+

------------------------------------------------------------------------

# Node.js Lambda Example

``` javascript
exports.handler = async (event) => {
    const body = JSON.parse(event.body);
    const inputArray = body.data || [];

    const evenNumbers = [];
    const oddNumbers = [];
    const alphabets = [];

    inputArray.forEach(item => {
        if (typeof item === "number") {
            if (item % 2 === 0) evenNumbers.push(item);
            else oddNumbers.push(item);
        } else if (typeof item === "string" && /^[A-Za-z]+$/.test(item)) {
            alphabets.push(item.toUpperCase());
        }
    });

    return {
        statusCode: 200,
        body: JSON.stringify({
            status: "success",
            userId: "12345",
            email: "example@gmail.com",
            collegeRollNo: "20CS123",
            evenNumbers,
            oddNumbers,
            alphabets
        })
    };
};
```

------------------------------------------------------------------------

# Email Inbox → Lambda → GraphQL API Example

## Use Case

When a new email arrives, SES stores it in S3 and triggers Lambda.
Lambda processes the email and stores it in DynamoDB. AppSync GraphQL
API exposes email data.

## Architecture

     Incoming Email
         |
         v
     +----------------------+
     | AWS SES (Email Ingestion) |
     +------------+---------+
                  |
                  v
          +-------+--------+
          | Amazon S3      |
          +-------+--------+
                  |
                  v
          +-------+--------+
          | AWS Lambda     |
          +-------+--------+
                  |
                  v
          +-------+--------+
          | DynamoDB Table |
          +-------+--------+
                  |
                  v
          +--------------+
          | AppSync GQL  |
          +--------------+

------------------------------------------------------------------------

# Email Processor Lambda (Node.js)

``` javascript
const AWS = require("aws-sdk");
const s3 = new AWS.S3();
const dynamodb = new AWS.DynamoDB.DocumentClient();

exports.handler = async (event) => {
    const record = event.Records[0];
    const bucket = record.s3.bucket.name;
    const key = record.s3.object.key;

    const emailData = await s3.getObject({ Bucket: bucket, Key: key }).promise();
    const rawEmail = emailData.Body.toString();

    const fromMatch = rawEmail.match(/From: (.*)/);
    const subjectMatch = rawEmail.match(/Subject: (.*)/);

    const emailRecord = {
        messageId: Date.now().toString(),
        from: fromMatch ? fromMatch[1] : "Unknown",
        subject: subjectMatch ? subjectMatch[1] : "No Subject",
        receivedAt: new Date().toISOString()
    };

    await dynamodb.put({
        TableName: "EmailsTable",
        Item: emailRecord
    }).promise();

    return { status: "Email processed", emailRecord };
};
```

------------------------------------------------------------------------

# GraphQL Schema (AppSync)

``` graphql
type Email {
  messageId: ID!
  from: String!
  subject: String!
  receivedAt: String!
}

type Query {
  getEmails: [Email]
  getEmail(messageId: ID!): Email
}
```

------------------------------------------------------------------------

# Sample Queries

``` graphql
query {
  getEmails {
    messageId
    from
    subject
    receivedAt
  }
}
```

``` graphql
query {
  getEmail(messageId: "1737261829") {
    subject
    from
  }
}
```
