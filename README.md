# AWS-Smart-Room-Automation
Smart room automation project being built with AWS IoT Core, Lambda, API Gateway, DynamoDB, EventBridge, CloudWatch, and an always-on edge gateway.


## Current Project Progress

I started by building and testing the AWS cloud side of the project before connecting the physical devices.

So far I have configured AWS IoT Core and created an MQTT topic structure for commands, device state, and telemetry.

For the sensor side, I used the AWS IoT MQTT test client to simulate a temperature sensor. The message went through AWS IoT Core and an IoT Rule and was successfully stored in a DynamoDB table called SmartRoomTelemetry.

I also created a Lambda function called SmartRoomCommandRouter. The function takes a device ID and an action and publishes the command to the correct MQTT topic. I tested this with curtain-left and confirmed that the MQTT test client received the open command.

I created an HTTP API called SmartRoomControlAPI using API Gateway. The POST /commands route connects to the SmartRoomCommandRouter Lambda function. I tested the API using AWS CloudShell and confirmed the command made it through API Gateway, Lambda, and AWS IoT Core.

CloudWatch logging is working for the Lambda function. I can see when the function starts, what MQTT topic it publishes to, and the Lambda execution report.

I also tested EventBridge Scheduler with a one-time curtain automation. At the scheduled time, EventBridge automatically invoked the Lambda function and the curtain-left command appeared in AWS IoT Core.

For device state, I created an AWS IoT Thing called curtain-left and added a classic Device Shadow. I tested a desired position of 100 and then simulated the device reporting position 100. Once the desired and reported states matched, the shadow delta disappeared.

The API Gateway command route is protected using AWS IAM authorization. I tested an unsigned API request and it was rejected as Forbidden. I then tested an AWS signed request and the command was successfully accepted and published to IoT Core.

## AWS Services Used So Far

- AWS IAM Identity Center
- AWS IoT Core
- AWS IoT Device Shadows
- AWS Lambda
- Amazon API Gateway
- Amazon DynamoDB
- Amazon CloudWatch
- Amazon EventBridge Scheduler
- AWS IAM
- AWS Budgets

## What Still Needs To Be Done

The cloud foundation is working, but the physical devices are not connected to AWS yet.

The next part of the project will be setting up the mini PC as the edge gateway. The mini PC will connect securely to AWS IoT Core and receive commands from the cloud.

After the edge gateway is working, I will start connecting the real smart room devices. I plan to start with a smart light or smart plug before moving on to the curtains and blinds.

The physical device integration, edge software, AWS IoT certificate setup, and real device state reporting are still in progress.
