# Cloud Test Results

This document keeps track of the main tests I completed while building the AWS cloud portion of my smart room project.

## MQTT Telemetry Test

I used the AWS IoT MQTT test client to simulate temperature sensor data.

The message included a device ID, temperature, and humidity.

The message was received by AWS IoT Core, processed by an IoT Rule, and stored in the SmartRoomTelemetry DynamoDB table.

Result: Passed

## Lambda Command Test

I tested the SmartRoomCommandRouter Lambda function with a command for curtain-left.

The Lambda function published the command to:

smartroom/bedroom/command/curtain-left

I subscribed to the smartroom/bedroom/command/# topic in the MQTT test client and confirmed the message was received.

Result: Passed

## API Gateway Test

I created the SmartRoomControlAPI with a POST /commands route.

I sent a POST request from AWS CloudShell with a curtain-left open command.

API Gateway invoked the Lambda function and the command was successfully published to AWS IoT Core.

Result: Passed

## CloudWatch Test

I checked the CloudWatch log group for SmartRoomCommandRouter.

The logs showed the Lambda function starting, publishing the MQTT command, ending, and reporting execution information.

Result: Passed

## EventBridge Scheduler Test

I created a one-time EventBridge Scheduler automation.

The schedule automatically invoked SmartRoomCommandRouter and published a curtain-left command at the scheduled time.

I confirmed the command appeared in the MQTT test client.

Result: Passed

## Device Shadow Test

I created an AWS IoT Thing called curtain-left and gave it a classic Device Shadow.

I first set the desired position to 100.

AWS showed a delta because the reported state did not match the desired state.

I then simulated the device reporting position 100.

The delta disappeared after the desired and reported states matched.

Result: Passed

## API Security Test

I enabled AWS IAM authorization on the POST /commands API route.

I first sent an unsigned request and API Gateway returned Forbidden.

I then sent an AWS signed request using my Identity Center session and the command was accepted and published to AWS IoT Core.

Result: Passed

## Current Limitation

These tests verify the AWS cloud side of the project.

The physical smart room devices have not been connected to AWS yet. The next phase will use the mini PC as the edge gateway between AWS IoT Core and the physical devices.
