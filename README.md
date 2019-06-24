# Smartguard

## Index

### Introductions

This project consists of three components which are a Raspberry Pi with a PIR motion sensor, another Raspberry Pi with a speaker and camera that combined with a microphone and a IOS client end based on Swift.


### Objectives

Specially, the first part is a Raspberry Pi connected with a PIR Motion sensor performing the motion detection function. As one of our project’s function is to monitor the outside in front of the door and send the notification to users when a specific motion is detected, the Raspberry Pi is chosen as a server based on the Node JS and MQTT protocol. The PIR Motion sensor is also our choice because it is easy to use and can detect the motion of a warm body within the range of five or six meters. On the Raspberry Pi, a server named Mosquitto using the MQTT protocol is setted up and it will run automatically when the the Raspberry Pi running. A JS format file including the code of controlling the motion sensor and the Mosquitto clients to publish the notification to the Mosquitto server will be executed manually. After running these file, the Mosquitto server will receive a message from the client that has subscribed a specific topic if the motion sensor is triggered and broadcast this message to all the clients that also subscribe the same topic.

The second part is another Raspberry Pi connected with a camera, a microphone and a speaker which enable the user to communicate with the visitors through our iOS application. The video stream is handled by MJPG streamer in the server (Raspberry Pi) and the voice stream is handled by Twilio framework in both client and server side. The frameworks used here will be introduced in part 3 later.



### Pre-requisites


1. Setup gcloud 

Changes in this tutorial made without python SDK are done with the Google Cloud SDK gcloud command-line tool. This tutorial assumes that you have this tool installed and authorized to work with your account per the documentation.

2. create a service account with 3 roles 
     - Cloud Scheduler admin
     - service account token creator
     - service account user
  
  `Example:`
 ![ See image below :](./images/service_account_svc_cloud_scheduler.png)
                 

### How to deploy cloud functions

- Cloud Functions can be triggered by messages published to Cloud Pub/Sub topics in the `same GCP project as the function`. 
- Cloud Pub/Sub is a `globally distributed` message bus that automatically scales as you need it and provides a foundation for building your own robust, global services.

### Event Structure
- Cloud Functions triggered from Cloud Pub/Sub topic events will be sent an event containing a `PubsubMessage object`. 
- The `format` of the `PubsubMessage` is as per the published format for `PubsubMessage objects`.

- The payload of the `PubsubMessage` (the data you published to the topic) is stored as a `base64-encoded string` in the data attribute of the PubsubMessage. To extract the payload of the PubsubMessage you need to decode the data attribute.

Option1 : Using gcloud command

```python
gcloud functions deploy stop_instances --runtime python37 --trigger-resource T
OPIC_STOP_INSTANCES --trigger-event google.pubsub.topic.publish
```

- deploy : < Name of your cloud function >
- --runtime : <You can choose from `Node.js v6` ,`Nodejs v8` & `Python3.7`>
- --trigger-resource :< Name of your pub/sub Topic >
- --trigger-event : < How do you want to trigger it eg. by publishing to pubsub >

where TOPIC_NAME is the name of the Cloud Pub/Sub topic to which the function will be subscribed. If the topic doesn't exist, it is created during deployment.

### How to check cloud function logs 

```Python
gcloud functions logs read --limit 50
```

#### Output



## How to deploy cloud scheduler using python sdk

1. Change into the directory that holds your code
```
cd cloud_scheduler
``` 
2. Activate virtual env for python so that you have an isolated environment and all the necessary modules.

```
source setup/venv/venv_cloud_scheduler/bin/activate
```
3. use python to deploy
   
   Before you deploy ,update the inputs to the below variables 

- PROJECT_ID = "the ID of your project where you want to deploy this cloud scheduler"

- LOCATION_ID = " Provide the location where you want to deplpy"
Currently only 3 regions are supported US , europe and asia...

`example`:
LOCATION_ID = "us-central1"

- JOB_NAME
Update with the name of the job
`example`:
JOB_NAME = 'start_instances_at_9am'

The command to deploy is as below.

```python cloud_scheduler_start_instances.py``` 
