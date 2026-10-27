# AWS Smart Room Automation Project Charter

**Project Status:** In Progress
**Project Type:** Cloud / IoT Portfolio Project
**Project Owner:** Samuel Quiroz-Rincon

## Project Overview

The goal of this project is to build a smart room automation system using AWS for the cloud side and a mini PC at home for the local side.

I already have several smart devices in my room including smart lights, smart plugs, SwitchBot curtains, and SwitchBot blinds. Instead of only using the normal apps and hubs that come with the devices, I want to see how much of the system I can build and control myself.

The mini PC will stay on at home and act as the connection between AWS and the devices in my room. AWS will handle things like sending commands, automation logic, storing data, device information, and logging.

I am also using this project to get more hands-on experience with AWS and cloud engineering. One of my main goals is to understand how the services actually work together instead of just following a tutorial and copying the steps.

## Why I Chose This Project

I wanted to make a project that combines something useful in real life with the cloud skills I am learning.

Smart home devices are normally easy to set up using their manufacturer apps, but doing that does not really teach me much about what is happening on the cloud side.

With this project I want to build more of the system myself and understand how commands and data travel between AWS, my home network, and the physical devices.

Some of the things I want to learn from this project are:

* AWS IoT
* MQTT
* Serverless applications
* APIs
* Lambda functions
* Databases
* Edge computing
* Python
* Logging and troubleshooting
* AWS security
* Git and GitHub
* Project documentation

I also want the finished project to be something I can put on my resume and explain during an interview.

## Main Project Goals

My main goals are:

* Connect a mini PC at home to AWS securely.
* Use AWS IoT Core to communicate between AWS and my home.
* Send commands from AWS to devices in my room.
* Send device information and sensor readings back to AWS.
* Store useful device information in DynamoDB.
* Create automations based on schedules and sensor readings.
* Keep track of device states.
* Create an API that can eventually be used to control the room.
* Use CloudWatch so I can troubleshoot problems.
* Practice using IAM and other AWS security features correctly.
* Document what I build and what I learn on GitHub.

I also want to make sure I understand why I am using each AWS service instead of adding services just to make the project look more complicated.

## Devices

These are the devices I currently plan to use:

* 2 SwitchBot curtain controllers
* 2 SwitchBot blind controllers
* Smart lights
* Smart plugs

Devices I plan to add later:

* Temperature sensor
* Smart clock or information display

I might add more devices later, but I want to get the main system working before making the project too big.

## Planned AWS Services

### AWS IoT Core

AWS IoT Core will be the main service used for communication between AWS and the mini PC.

I plan to use MQTT to send messages between the cloud and the edge gateway.

A basic command could follow this path:

```text
AWS
->
AWS IoT Core
->
Mini PC
->
Smart Device
```

Information coming from a sensor could go the opposite direction:

```text
Temperature Sensor
->
Mini PC
->
AWS IoT Core
->
AWS
```

### AWS Lambda

Lambda will be used for cloud-side logic.

For example, if an API request says to open the curtains, Lambda could process that request and send the correct command through AWS IoT Core.

Lambda may also be used for some automation logic.

### Amazon API Gateway

API Gateway will allow me to create API endpoints for controlling the room.

For example, I could eventually have a request for something like:

```text
POST /devices/curtain-left
```

The request could then be processed by Lambda and sent to the correct device through AWS IoT Core.

Later I may connect the API to a simple web dashboard.

### Amazon DynamoDB

DynamoDB will mainly be used to store information.

Some examples are:

* Temperature readings
* Sensor readings
* Device activity
* Device state history
* Automation events

DynamoDB will not control the devices directly. Its main purpose in this project is storing data that I may want to use or look at later.

### AWS IoT Device Shadows

Device Shadows will help keep track of the state that AWS wants a device to have and the state that the device is actually reporting.

For example:

```text
Desired State: Curtain Open

Reported State: Curtain Open
```

If the desired state and reported state do not match, it can show that the device has not completed the requested action yet.

### Amazon EventBridge Scheduler

EventBridge Scheduler will be used for automations that need to happen at certain times.

For example:

```text
7:00 AM
->
EventBridge Scheduler
->
Lambda
->
AWS IoT Core
->
Mini PC
->
Open Curtains
```

I could use this for morning routines, nighttime routines, or other scheduled actions.

### Amazon CloudWatch

CloudWatch will be used mainly for logs, monitoring, and troubleshooting.

I want to be able to see things like:

* Lambda errors
* Failed commands
* AWS IoT problems
* API requests
* Automation failures

I also want to learn how to troubleshoot the system instead of only knowing how to use it when everything works correctly.

### AWS IAM

IAM will be used to control permissions between AWS services.

I want to practice using least-privilege permissions instead of just giving everything full access.

### AWS IoT Greengrass V2

I plan to test AWS IoT Greengrass V2 on my mini PC.

The idea is to use Greengrass to help the mini PC work as an AWS-connected edge device and run local software.

I have not installed or tested this yet, so for now this part of the project is still planned.

## Mini PC / Edge Gateway

The mini PC at home will be an important part of the project.

AWS cannot directly control a Bluetooth device sitting inside my room, so the mini PC will work as the bridge between AWS and the local devices.

The basic idea is:

```text
AWS Cloud
->
Internet
->
Mini PC
->
Local Smart Device
```

Things I may use on the mini PC include:

* Linux
* Python
* MQTT
* AWS IoT Greengrass V2
* Bluetooth Low Energy
* Local network APIs
* Other device-specific communication methods

I may also use Home Assistant if it helps solve a device integration problem, but I do not want to make the whole project depend on Home Assistant unless there is a good reason.

If I can communicate with a device locally using Python or another method, I would like to understand and build that part myself.

I also know that not every device is going to use the same type of connection.

## Planned System Architecture

The main cloud-to-device control path is planned to be:

```text
User
->
API Gateway
->
Lambda
->
AWS IoT Core
->
MQTT over TLS
->
Mini PC
->
Device
```

Information coming from a device or sensor would look more like:

```text
Device or Sensor
->
Mini PC
->
AWS IoT Core
->
IoT Rule
->
DynamoDB
```

For device state, I plan to test something like:

```text
Desired State
->
AWS IoT Device Shadow
->
Mini PC
->
Physical Device

Physical Device
->
Mini PC
->
Reported State
->
AWS IoT Device Shadow
```

CloudWatch will be used for monitoring and logging. It will not be used to send the actual commands to the devices.

## Example Automations

### Morning Mode

One automation I want to build is a morning routine.

It could do things like:

* Open the curtains.
* Open the blinds.
* Turn on selected lights.
* Turn on certain smart plugs.

The cloud side could look like:

```text
Scheduled Time
->
EventBridge Scheduler
->
Lambda
->
AWS IoT Core
->
Mini PC
->
Room Devices
```

### Night Mode

At night I would like to be able to:

* Close the curtains.
* Close the blinds.
* Turn off the main lights.
* Turn off unnecessary smart plugs.
* Possibly leave one light at a lower brightness.

### Temperature Automation

When I get a temperature sensor, I would like to create an automation based on the room temperature.

For example:

```text
Temperature Sensor
->
Mini PC
->
AWS IoT Core
->
Automation Logic
->
AWS IoT Core
->
Mini PC
->
Smart Plug
```

One possible use would be turning on a fan connected to a smart plug if the room gets too hot.

Until I have the real sensor, I can use fake or simulated temperature readings to build and test the AWS side.

## What Is Not Part of the First Version

There are some things I am purposely leaving out so the project does not become too complicated.

For the first version I am not planning to:

* Build my own smart hardware.
* Replace firmware on existing devices.
* Support every smart home brand.
* Build a full mobile application.
* Build the system for multiple houses.
* Make it a commercial product.
* Open unnecessary ports on my home router.
* Completely remove manufacturer apps if they are still needed for things like setup or firmware updates.

My first goal is to get one complete cloud-to-device system working correctly.

After that I can add more features.

## Security

Security is something I want to pay attention to while building this because the project involves AWS and a computer running inside my home network.

Some things I plan to use are:

* IAM permissions with least privilege where possible.
* X.509 certificates for AWS IoT authentication.
* TLS encryption for MQTT communication.
* No passwords or AWS credentials uploaded to GitHub.
* No private certificates uploaded to GitHub.
* No unnecessary inbound internet access to my home network.
* CloudWatch logging for errors and activity.

I also plan to use a `.gitignore` file so that private files do not accidentally get uploaded to GitHub.

## Project Risks

### Risk 1: A device cannot be controlled locally

**Impact:** High

Some smart devices may not allow direct local control or may use a protocol that is difficult to work with.

**Plan:** Test devices individually before depending on them. If one device does not work the way I expected, I can look at another integration or change how that part of the project works.

### Risk 2: SwitchBot integration is harder than expected

**Impact:** Medium

The SwitchBot devices may require more work than the smart lights or plugs.

**Plan:** Start with an easier device first so I can prove that the AWS-to-device architecture works before working on the harder devices.

### Risk 3: AWS costs are higher than expected

**Impact:** Medium

I am still learning AWS, so I need to make sure I do not accidentally leave something running that costs more than expected.

**Plan:** Set up AWS budget alerts and keep track of what resources I create.

### Risk 4: Private information gets uploaded to GitHub

**Impact:** High

AWS credentials, private certificates, passwords, and other secrets should never be uploaded to the public repository.

**Plan:** Use `.gitignore`, keep secrets separate from the project code, and check my files before committing them.

### Risk 5: The mini PC stops working

**Impact:** High

Since the mini PC will be the connection between AWS and the room, the system may lose cloud control if it goes offline.

**Plan:** Add logging and health checks later and document how the edge system can be restarted.

### Risk 6: The internet goes down

**Impact:** Medium

Cloud-based commands would not work normally during an internet outage.

**Plan:** Look into keeping some important automations local on the mini PC where it makes sense.

### Risk 7: The project becomes too complicated

**Impact:** Medium

There are a lot of things I could add to this project, which could make it difficult to actually finish.

**Plan:** Get the main AWS-to-mini-PC-to-device path working before adding extra features.

### Risk 8: I do not have the temperature sensor yet

**Impact:** Low

I cannot test the real sensor until I get one.

**Plan:** Use simulated temperature values while I build the cloud side.

I expect the risks to change as I start working with the real AWS services and devices.

## Project Milestones

### Milestone 1: Planning

* Create GitHub repository.
* Create project charter.
* Finish the first architecture diagram.
* Write project requirements.
* Set up project tasks.

### Milestone 2: AWS Cloud Setup

* Configure AWS IoT Core.
* Decide on MQTT topic names.
* Test MQTT messages.
* Create DynamoDB table.
* Create IoT Rules.
* Create Lambda function.
* Create API Gateway.
* Configure CloudWatch.
* Configure EventBridge Scheduler.

During this part I can use simulated devices instead of waiting until all of the real devices are connected.

### Milestone 3: Mini PC Setup

* Prepare the mini PC.
* Install the software I need.
* Test AWS IoT Greengrass V2.
* Connect the mini PC to AWS IoT Core.
* Test MQTT communication in both directions.
* Start building the Python edge controller.

### Milestone 4: Physical Devices

I want to add the devices one at a time instead of trying to connect everything at once.

My current plan is:

1. Smart light or smart plug
2. SwitchBot curtain
3. Second curtain
4. SwitchBot blinds
5. Temperature sensor
6. Other devices

This order may change depending on what I learn while testing the devices.

### Milestone 5: Automation

Some of the automations I want to test are:

* Morning routine
* Night routine
* Scheduled curtains or blinds
* Temperature automation
* Device state synchronization
* What happens when a device does not respond
* What happens when the mini PC reconnects after losing connection

### Milestone 6: Testing and Final Documentation

* Test the complete system.
* Document problems I run into and how I fixed them.
* Add screenshots.
* Update the architecture diagram if the actual system changes.
* Review the security setup.
* Finish the main GitHub README.
* Add final project results.
* Prepare a short demonstration.
* Write resume bullet points.

## How I Will Track Progress

Because this is a project I am still building, I do not want to say that something works before I have actually tested it.

I will use these statuses:

**Planned**

I want to build it, but I have not started it yet.

**In Progress**

I am currently working on it.

**Implemented**

The code or AWS configuration has been created.

**Verified**

I tested it and confirmed that it works.

This will help me keep the GitHub project accurate while I am building and learning.

## Success Criteria

I will consider the first version of the project successful when:

1. My mini PC can securely connect to AWS IoT Core.
2. The mini PC can send information to AWS.
3. AWS can send commands back to the mini PC.
4. At least one real smart device can be controlled through the full system.
5. Device or sensor data can be stored in DynamoDB.
6. Device Shadows work for at least one device.
7. At least one scheduled automation works.
8. At least one sensor or event-based automation works.
9. CloudWatch logs help me understand what happened when something fails.
10. Private credentials and certificates are not exposed on GitHub.
11. My GitHub repository explains what I actually built.
12. I can explain the project and why I used the AWS services that I chose.

The project does not have to control every device in my room to be successful. The most important thing is getting a complete cloud-to-edge system working, understanding how the different parts connect, and being able to explain what I learned while building it.

