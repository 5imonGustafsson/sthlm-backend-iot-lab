# sthlm-backend-iot-lab

2021 Stockholm Backend community lab. The lab will consist of 4 sessions

## Part 1 - Get started with MQTT

### Local mosquitto broker

The easiest way to get started with MQTT is to run a local mosquitto MQTT broker in [Docker](https://hub.docker.com/_/eclipse-mosquitto) or on your [local machine](https://mosquitto.org/download/).

To start the broker run: ```docker run -it -p 1883:1883 -p 9001:9001 eclipse-mosquitto:latest```
This command download and start a container with a mosquitto MQTT broker that listens on port 1883 for connections. Remember that port 1883 is unsecure and should only be used when testing locally. To run MQTT secure with TLS please use port 8883 instead. (see next step)

To test out the broker if you installed mosquitto locally you can use the mosquitto CLI. Type ```mosquitto_sub -t mqtt/test``` in your terminal to subscribe to the topic  ```mqtt/test```
To publish something to the same topic open a new terminal and enter ```mosquitto_pub -t mqtt/test -m "Hello MQTT"``` When you publish a message you should see the message in the terminal you subscribed to the topic on. If you want to try it programmatically [Eclipse paho](https://www.eclipse.org/paho/index.php?page=downloads.php) is a good option to start with.

### Mosquitto SSL Configuration

To run mosquitto secure we need to do a few steps

1. Generate self-signed keys
2. create a folder to store the certificates
3. create a mosquitto.conf file and add the generated certs
4. create a docker-compose file to setup a container with a secure mosquitto broker

Once keys and certs are generated and the MQTT broker runs in a docker container an easy way to test the flow is either by using mosquitto_sub/mosquitto_pub or by installing node red via docker  ```docker run -it -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red``` and in the user interface add mqtt nodes with the client certs generated.

### Azure IoT Hub

To get started sending data to Azure IoT Hub a device need to be registered first. This can be done programmatically or in azure portal. A good suggestion to get started with is to install the [vs code plugin](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) for IoT Hub. Azure IoT Hub can use certifications just like in the example before (self signed etc) but one of the nice things with IoT Hub is the usage of SaS (shared access signature). For a device it is possible by making a call to an API to create device unique sas to make it possible for a device to send device specific messages to IoT Hub. This is really nice when it comes to orchestrating multiple devices and automate device orchistration. By using the vs code plugin one can generate a sas within vs code and use that to configure a device to be able to send MQTT,HTTPS and AMQP messages. IoT Hub is a bit limited in how devices can send messages. To send a pure MQTT message the main topic setup needs to follow this structure: ```devices/{deviceId}/messages/events/``` and to recieve C2D (cloud to device messages) the device need to subscribe to topics with this structure: ```devices/{deviceId}/messages/devicebound/#``` where  ```#``` is a wildcard for all within that subtopic.

How to connect the MQTT client to the IoT Hub:

1. Set the server adress to ```sthlm-iot-showcase-dev-iot-hub.azure-devices.net``` 
2. Set port to 8883
3. Set the Client ID to the deviceId
4. Set username to ```sthlm-iot-showcase-dev-iot-hub.azure-devices.net/{deviceId}/?api-version=2018-06-30```
5. Set password to the full Shared access signature. It should look like this: ```SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device```

### AWS IoT Core

Compared to Azure, IoT Core is not using SaS. Instead it is using the same setup as in the Mosquitto SSL Configuration (minus the self-signed keys). Unlike Azure, to send sensor data to the cloud does not need specific endpoints etc unless one want to use specific IoT Core features like device shadows, job agents etc.

How to connect the MQTT client to the IoT Core:

1. Set the server adress to ```a2058z6ulhumcw-ats.iot.eu-north-1.amazonaws.com``` 
2. Set port to 8883
3. Set the Client ID to the deviceId
4. Set Certificate to the one generated in AWS
5. Set private key to the one generated in AWS
6. Set CA certificate to the one generated in AWS (I do not know if this is really needed)

## Part 2 - TBD
