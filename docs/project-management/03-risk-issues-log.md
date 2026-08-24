# Risks and Issues Log

This document keeps track of problems, risks, and things I still need to figure out while building the smart room project.

## Physical device integration

Status: Open

The physical smart room devices are not connected to AWS yet.

The mini PC still needs to be configured as the edge gateway between AWS IoT Core and the devices.

Plan:

Set up the mini PC at home and test one simple device first, probably a smart plug or smart light.

## Device compatibility

Status: Open

Some of the smart devices normally depend on their manufacturer's app or hub. I want the automation logic to run through my AWS project instead.

Plan:

Test how each device can be controlled from the mini PC before adding it to the full system.

## AWS IoT certificates

Status: Open

The edge gateway does not have its AWS IoT certificate and private key yet.

I did not create the permanent certificate on my work computer because the credentials should be created and stored on the mini PC.

Plan:

Create the certificate and IoT policy when I configure the mini PC.

## API security

Status: Resolved

The POST /commands API route was originally open while I was testing it.

I enabled AWS IAM authorization and tested both an unsigned and signed request.

The unsigned request was blocked and the signed request was accepted.

## MQTT endpoint connection

Status: Resolved

The AWS IoT MQTT test client initially could not connect while the IoT endpoint was being created.

I waited and tested it again later. The client connected successfully and MQTT testing continued.

## Cost risk

Status: Monitoring

AWS services can create charges if resources are left running or used more than expected.

I created an AWS Budget to help monitor project costs.

## Current project risk

The biggest remaining risk is physical device integration because I have not tested the mini PC with the real devices yet.
