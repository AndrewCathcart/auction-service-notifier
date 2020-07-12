# auction-service-notifier

Currently contains a Lambda function that sends emails from an SQS queue via SES for our [auction-service](https://github.com/AndrewCathcart/auction-service).

## Installation

Prerequisites

- [Node](https://nodejs.org/en/) is required. This can be installed via the website or by using [nvm](https://github.com/nvm-sh/nvm) (see their documentation).
- [AWS CLI Version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- Serverless Framework `$ npm install -g serverless`

Then install dependencies

`$ npm install`

Deploy to development in AWS

`$ sls deploy -v`

## SQS Notes

- Can help send messages between microservices using a queue, decoupling them.
- Services can produce message to the queue, messages will be retained until picked up by other microservices or deleted after the retention period.
- E.g. could have a Mail Queue for an Order Service, Auth Service & Delivery Service. Notification Service can then pick things up off the queue and send out emails at it's own pace for each of these microservices.
- Security - Granular control over who can send and receieve messages. Can also encrypt messages.
- Durability - Amazon SQS stores messages on multiple servers.
- Availability - Highly-concurrent access to messages and high availability for message production/consumption.
- Scalability - Auto-scales to handle any load increase or spike, without additional instructions.
- Reliability - Messages can be sent by multiple producers and multiple consumers at the same time.
- Customization - Can set default delays, variable messages sizes, message splitting etc.

Queue Types

- Standard Queue

  - Max throughput
  - At-least-once delivery
  - Best-effort ordering

- FIFO Queue

  - Guaranteed order
  - Exactly once
  - Limited throughput (up to 3k messages / second with batching or 300 without batching (batch 10 msgs at a time))

- Dead Letter Queue
  - Standard SQS Queue with a different purpose.
  - Can send failed messages to this queue to either reprocess or identify problem.

SQS Pricing

- Based on usage. No min fees.
- First 1m monthly requests are free.
- FIFO - \$0.50 per 1m requests
- Standard - \$0.40 per 1m requests
