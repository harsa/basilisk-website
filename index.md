## Basilisk - Temperature logger

A DIY IoT system for logging temperature & humidity data with inexpensive hardware.

### Introduction 
Internet-of-things -things are fun and surprising cheap to get started with. This post describes the setup for a scalable system for   real-time logging  of temperate and humidity data. Sensor units are easy and cheap to put together in numbers.

### Required hardware
For each sensor unit a NodeMCU board with an ESP8266 chip and a DHT22 temperature sensor are needed. The NodeMCU’s are powered with a regular micro-usb jack and can be had for less than 6€ total.

For the server part, mine runs on a Raspberry pi model 3, but any computer running Linux or Os X should do . The server is a Node app with sqlite, mosquitto and an embedded node-red instance for REST API endpoints.

### Setting up the server

use the command
git clone https://github.com/harsa/basilisk-node
to clone the basilisk server to a directory of your choosing.

Make sure your (local) server ip is static make a note for use in the next step. 

Install js dependencies by running 
npm install 

install mosquitto as the mqtt message broker with
apt-get install mosquitto

start the server with
node index.js

### Setting up the sensors
Connect the DHT22 to NodeMCU. DHT22 Pins + o - should go to  x, x and on the NodeMCU. 

Arduino IDE is required to compile & install the Arduino sketch running the sensors. Download and open the sketch and edit the WiFi settings and server IP.
 https://github.com/harsa/basilisk-esp8266

Upload the sketch to the NodeMCU through the micro-USB port and verify that something meaningful is output in the arduino serial console. Disconnect the NodeMCU from your computer and power it with a regular micro-USB power supply.

Note that you can upload the same version of this sketch of all the sensors you have without having to make per unit changes.

### Seeing it all come together
 
Type your server’s ip address to your browser. Note that you have to be on the same local network. 

### TECHNICAL STUFF

#### Delivering measurements with MQTT
The sensors are read once a minute and values transmitted with the MQTT protocol to message broker running on the server.

#### Logging measurements and setting api endpoints
Node-red does the heavy lifting when it comes to measurements. 

A flow exists MQTT messages are listened to and values saved for corresponding sensors. Values are persisted in a sqlite database.

#### Web app
A statically served React+Redux app. Displays a temperature gauge and a line chart for each sensor. Get’s live updates from the same mqtt broker as the server that persistst it but over websockets. 
Check out the source & build instructions at [Github]

Note: A prebuilt version of this already exists in the basilisk-node and doesn’t need to be manually build 

basilisk-web in github

Relevant Git repositories 
basilisk-node
basilisk-esp8266
basilisk-web

#### Links
Nodemcu in aliexpress
DHT22 in aliexpress
Raspberry Pi Model 3B starter pack

https://github.com/harsa/basilisk-node
https://github.com/harsa/basilisk-esp8266
https://github.com/harsa/basilisk-web
