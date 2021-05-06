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

## Part 2 -