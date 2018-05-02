#  TTN & node-RED & InfluxDB & Grafana- install log

This installation follows

https://www.thethingsnetwork.org/labs/story/store-and-visualize-data-using-influxdb-and-grafana

and is largely just a collection of links and remarks where necessary.

It is meant for Debian/Ubuntu, however, the main links will provide platform-independent guides - we have had people install the complete set within an hour, following those.

For the impatient, here s a summary of all commands run in a full installation - for details, scroll down:

## Installation / commands summary (Debian/Ubuntu)

```
// Node-RED

# apt-get install nodejs     (<<< or nodejs-legacy)
# nodejs -v                  ( or: node -v)
# apt install npm
# npm install -g --unsafe-perm node-red

(start with)

# node-red

(should give result)

2 May 09:58:17 - [info] 

Welcome to Node-RED
===================

2 May 09:58:17 - [info] Node-RED version: v0.18.4
2 May 09:58:17 - [info] Node.js  version: v4.2.6
2 May 09:58:17 - [info] Linux 4.4.0-62-generic x64 LE
2 May 09:58:17 - [info] Loading palette nodes
2 May 09:58:18 - [warn] ------------------------------------------------------
2 May 09:58:18 - [warn] [node-red/rpi-gpio] Info : Ignoring Raspberry Pi specific node
2 May 09:58:18 - [warn] [node-red-node-twitter/twitter] ReferenceError: Invalid left-hand side in assignment
2 May 09:58:18 - [warn] ------------------------------------------------------
2 May 09:58:18 - [info] Settings file  : /root/.node-red/settings.js
2 May 09:58:18 - [info] User directory : /root/.node-red
2 May 09:58:18 - [warn] Projects disabled : set editorTheme.projects.enabled=true to enable
2 May 09:58:18 - [info] Flows file     : /root/.node-red/flows_poetry.json
2 May 09:58:18 - [info] Creating new flow file
2 May 09:58:18 - [info] Server now running at http://127.0.0.1:1880/
2 May 09:58:18 - [info] Starting flows
2 May 09:58:18 - [info] Started flows

// Influx

# apt-get install influxdb influxdb-client

(test with)

# influx

// Grafana

# wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_5.1.0_amd64.deb
# sudo dpkg -i grafana_5.1.0_amd64.deb# 


```


## Essential reading

To understand the ttn node-red connection, the MQTT formats etc:

https://www.thethingsnetwork.org/docs/applications/nodered/quick-start.html
https://www.thethingsnetwork.org/docs/applications/mqtt/api.html#device-events


## Installing node-RED

https://www.thethingsnetwork.org/docs/applications/nodered/

This works fine following the guide - except a few self-explaining apt-get glitches -

Note: We found it was important to install the -legacy version of the nodejs package because Node-RED's startup scripts expect your Node.js binary to be named node, but the standard package uses nodejs instead. This is due to a naming conflict with a preexisting package. Easy to fix by means of a symlink/ alias too: in your ../bin directory, do
```
ln -s nodejs ./node
```

Check your version in case of troubles!

sebastian@NewThing:~$ node -v
v4.2.6

Depending on versions, weather and time of day, you might need to install npm separately (not included in nodejs) etc - 

When done, try reaching your web interface on

http://yourServerIP:1880



### Important note to self

node-RED names its flows after the hostname! ... whenever you change hostname, you will lose your flows!

Check startup on command line to see which flows it is trying to open, and rename accordingly.


## Installing influxdb

Installed via apt - no probs.

version: InfluxDB shell 0.10.0

Test interactive connection to db:
```
$ influx
Visit https://enterprise.influxdata.com to register for updates, InfluxDB server management, and monitoring.
Connected to http://localhost:8086 version 0.10.0
InfluxDB shell 0.10.0


> help

(to see all commands)


> CREATE DATABASE pit001

> show databases
name: databases
---------------
name
_internal
pit001

> use pit001
Using database pit001
> show measurements
name: measurements
------------------
name
003-01-light
003-01-moist
003-01-temp
...

> insert Temperature,Sensor=room1 value=20.0
> select * from Temperature
name: Temperature
-----------------
time			Sensor	value
1516892087287085637	room1	20

> select * from "003-01-light"
name: 003-01-light
------------------
time			value
1517485516395063851	4
1517485592557867571	4
1517485668698762094	4
1517485745150414591	4
1517485821014042293	4

(this example taken later, after sending measurements to db, obviously)
```


## Integrating node-RED & influxDB

Sending data from Node-RED to Influx is self explanatory and easy, via the influx node:

https://www.thethingsnetwork.org/labs/story/store-and-visualize-data-using-influxdb-and-grafana

https://flows.nodered.org/node/node-red-contrib-influxdb


## Installing Grafana

http://docs.grafana.org/installation/debian/


## Getting data from TTN into influx via Node-red

Follow:

https://www.thethingsnetwork.org/docs/applications/nodered/quick-start.html

Details on TTN Node-RED Nodes are here:

https://github.com/TheThingsNetwork/nodered-app-node

Configure node, try to deploy:

"The workspace contains some nodes that are not properly configured:

    [Flow 1] lopy2 (ttn device)
    [Flow 1] Tindiestick2 (ttn message)

Are you sure you want to deploy?"

If you get errors, it s a good idea to deploy anyway, as the cmd line output now will give you detailed error info:

> 22 Jan 15:22:19 - [error] [ttn app:c96e9f25.5c3498] No appId, accessKey or region set
> 22 Jan 15:22:19 - [error] [ttn device:lopy02] No app set

Adding missing things one by one, until ... success.
Here s an example:

### ttn device node settings

```
Name: <free form name>
App: the verbose name, NOT the hex ID!
Device ID: the hex id
Event: <check MQTT docu for possible events>
```
    
Read more about Events: https://www.thethingsnetwork.org/docs/applications/mqtt/api.html#device-events

### ttn app node settings

```
App ID: the verbose name, NOT the hex ID!
Broker: eu
Access key: hex key from bottom of this app page in TTN: https://console.thethingsnetwork.org/applications/pitlab-core
e.g. ttn-account-v2.XLXDZg4cTdT0X-4obr4F3A5StKGlt49VeqBMQl-QIRo
```

## Firewall

Note we need ports open to be able to talk to TTN:

```
1880/tcp open  http        Node.js (Express middleware)
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-title: Node-RED
3000/tcp open  ppp?
8083/tcp open  us-srv?
8086/tcp open  d-s-n?
8088/tcp open  radan-http?`

```

## Using functions

Use functions in node-red to handle your data from TTN.
Here s a very simple exmaple - it just takes array element 2 and passes it on to an output:

``` 
node.log ("function .... called")
var msg1 = {payload: msg.payload[2]};
return [ msg1];
``` 
Multiple outputs like so:
``` 
node.log ("function 5 called")
var msgAll = { payload: msg.payload};
node.log ("function 5 ALL")
var msg0 = { payload :msg.payload[0] };
var msg1 = { payload :msg.payload[1] };
var msg2 = { payload: msg.payload[2] };
return [ msg0, msg1, msg2];
``` 

These outputs we then send over as ...

## Input data to influxdb

The receiving database needs to know

```
server
database
name of measurement
```

that is all.

Server and database need to exist beforehand, while filling in a new measurement name will simply create it.


# Appendix / Examples

## Problems with receiving node messages - and SOLVED!

It turns out that you do not have to put a Device ID in the configs of ttn msgs -
removing them, and just configuring an app leads to success.
We now get message objects that look like this:

```
msg : Object
object
app_id: "dk-cph-itu-pitlab-01"
dev_id: "dk-cph-itu-pitlab-sb-01"
hardware_serial: "70B3D54990244E16"
port: 2
counter: 103
payload_raw: buffer[3]raw
0: 0xaa
1: 0x0
2: 0xfb
metadata: object
time: "2018-01-26T10:44:36.33424452Z"
frequency: 868.1
modulation: "LORA"
data_rate: "SF7BW125"
airtime: 51456000
coding_rate: "4/5"
gateways: array[1]
0: object
gtw_id: "eui-0000024b0805024b"
timestamp: 2159584427
time: "2018-01-26T10:44:36.302664Z"
channel: 0
rssi: -77
snr: 9.2
rf_chain: 1
latitude: 55.65797
longitude: 12.59292
altitude: 12
payload: buffer[3]raw
0: 0xaa
1: 0x0
2: 0xfb
_msgid: "b7951c5d.486ae"
```

Comparison between TTN backend and Node-Red:

```
==================================
ttn data
==================================



CC1F98

{
  "time": "2018-01-26T14:34:00.760049888Z",
  "frequency": 867.1,
  "modulation": "LORA",
  "data_rate": "SF7BW125",
  "coding_rate": "4/5",
  "gateways": [
    {
      "gtw_id": "eui-0000024b0805024b",
      "timestamp": 3039094131,
      "time": "2018-01-26T14:34:00.725905Z",
      "channel": 3,
      "rssi": -77,
      "snr": 9.8,
      "latitude": 55.65957,
      "longitude": 12.5912,
      "altitude": -35
    }
  ]
}

==================================

seen by node-red as
==================================

sg.metadata : Object
object
time: "2018-01-26T14:34:00.760049888Z"
frequency: 867.1
modulation: "LORA"
data_rate: "SF7BW125"
airtime: 51456000
coding_rate: "4/5"
gateways: array[1]
0: object
gtw_id: "eui-0000024b0805024b"
timestamp: 3039094131
time: "2018-01-26T14:34:00.725905Z"
channel: 3
rssi: -77
snr: 9.8
rf_chain: 0
latitude: 55.65957
longitude: 12.5912
altitude: -35


[204,31,152]

0: 0xcc
1: 0x1f
2: 0x98

```



