#  TTN & node-RED & InfluxDB & Grafana- install log

This installation follows

https://www.thethingsnetwork.org/labs/story/store-and-visualize-data-using-influxdb-and-grafana


## Essential reading

to understand the ttn node-red connection, the MQTT formats etc:

https://www.thethingsnetwork.org/docs/applications/nodered/quick-start.html
https://www.thethingsnetwork.org/docs/applications/mqtt/api.html#device-events


## Installing node-RED

https://www.thethingsnetwork.org/docs/applications/nodered/

Works fine following the guide - except a few self-explaining apt-get glitches -

Note: It's important to install the -legacy version of the nodejs package because Node-RED's startup scripts expect your Node.js binary to be named node, but the standard package uses nodejs instead. This is due to a naming conflict with a preexisting package.

Check your version in case of troubles!

sebastian@NewThing:~$ node -v
v4.2.6

Depending on versions, weather and time of day, you might need to install npm separately (not included in nodejs) etc - 

When done, try reaching your web interface on

http://<IP>:1880

works!

### Important notes to self

node-RED names its flows after the hostname ... whenever you change hostname, you ll lose your flows!

check startup on command line to see which flows it s trying to open, and rename accordingly.


## Installing influxdb

installed via apt - no probs.

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
003-02-light
003-02-moist
003-02-temp
003-03-light
003-03-moist
003-03-temp
003-04-light
003-04-moist
003-04-temp
003-05-temp
004-01-light
004-01-moist
004-01-temp

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
1517485897194458957	242
1517485973564120710	241
1517486049519694579	240
1517486390818333204	243
1517486465587086987	242
1517486540693590457	238
1517486615830199079	231


(this example taken later, after sending measuremnts to db, obviously)
```
## Integrating node-RED & influxDB

https://www.thethingsnetwork.org/labs/story/store-and-visualize-data-using-influxdb-and-grafana


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

Adding missing things one by one, til ... success.
Here s an example:

### ttn device node settings

```
Name: <free form name>
App: the verbose name, NOT the hex ID!
Device ID: the hex id
Event: <check MQTT docu for possible events>
```

Events: https://www.thethingsnetwork.org/docs/applications/mqtt/api.html#device-events

### ttn app node settings

```
App ID: the verbose name, NOT the hex ID!
Broker: eu
Access key: hex key from bottom of this app page in TTN: https://console.thethingsnetwork.org/applications/pitlab-core
e.g. ttn-account-v2.XLXDZg4cTdT0X-4obr4F3A5StKGlt49VeqBMQl-QIRo
```

## Firewall

Note we need ports open to be able to talk to TTN:

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

These outputs we the send over to

## Input data to influxdb

The receiving database needs to know

server
databses
name of measurement

that s all.


# Appendix / Examples

## Problems with receiving node messages - and SOLVED!

It turns out that you do not put a Device ID in the configs of ttn msgs -
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

Comparison between TTN backend and Nod-Red:

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

## Working version - semsor to node-red to influx


lacking documentation, but this works (and has a lot of unnecessary stuff just left for future experiments):

```
[{"id":"9cea1b0e.a9c0b8","type":"ttn device","z":"e7fb9fb.21ef46","name":"dev/lopy-seb02","app":"fe383a2e.538d78","dev_id":"","event":"up","x":110.5,"y":595,"wires":[[]]},{"id":"9480d726.b8d7b8","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"metadata","x":349,"y":508,"wires":[]},{"id":"e927e84.f6b3e18","type":"ttn device","z":"e7fb9fb.21ef46","name":"dev/lopy-seb01","app":"7714594.09e8ca8","dev_id":"","event":"activations","x":109.5,"y":538,"wires":[[]]},{"id":"6e482785.6e1468","type":"influxdb out","z":"e7fb9fb.21ef46","influxdb":"2e1a5f7.ecc9aa","name":"","measurement":"tindiestick","precision":"","retentionPolicy":"","x":780,"y":160,"wires":[]},{"id":"d6ac24de.c86878","type":"ttn message","z":"e7fb9fb.21ef46","name":"msg/01","app":"7714594.09e8ca8","dev_id":"","field":"","x":90,"y":375,"wires":[["2d37e02c.830c28","6cfe8b.d3fac174"]]},{"id":"d485b324.bc27d8","type":"function","z":"e7fb9fb.21ef46","name":"function msg.payload_raw","func":"return {\n  payload: {\n    data : msg.payload_raw\n  }\n};","outputs":"1","noerr":0,"x":380,"y":100,"wires":[[]]},{"id":"860ee88c.3093d","type":"influxdb out","z":"e7fb9fb.21ef46","influxdb":"2e1a5f7.ecc9aa","name":"","measurement":"buffer","precision":"","retentionPolicy":"","x":760,"y":200,"wires":[]},{"id":"cc4dbaa1.05931","type":"function","z":"e7fb9fb.21ef46","name":"function msg.buffer","func":"return {\n  payload: {\n    data : msg.payload.buffer[2]\n  }\n};","outputs":1,"noerr":0,"x":350,"y":180,"wires":[[]]},{"id":"fefbad95.334948","type":"function","z":"e7fb9fb.21ef46","name":"function msg.payload.counter","func":"    return {\n      payload: {\n        data : msg.counter\n      }\n    };","outputs":1,"noerr":0,"x":350,"y":140,"wires":[[]]},{"id":"f055ab8.3113dd8","type":"function","z":"e7fb9fb.21ef46","name":"function msg.payload.port","func":"return {\n  payload: {\n    data : msg.port\n  }\n};","outputs":1,"noerr":0,"x":380,"y":220,"wires":[[]]},{"id":"c4cfeef7.6a0e7","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":780,"y":240,"wires":[]},{"id":"2d37e02c.830c28","type":"function","z":"e7fb9fb.21ef46","name":"payload","func":"node.log (\"function called\")\nvar msg1 = {payload: msg.payload};\nnode.log (msg1);\nvar msg2 = { payload:msg.payload[0] };\nnode.log(msg2)\nvar msg3 = { payload:msg.payload[1] };\nnode.log(msg3)\nvar msg4 = { payload:msg.payload[2] };\nnode.log(msg4)\n\nreturn [ msg1, [ msg2, msg3, msg4 ]];\n","outputs":"5","noerr":0,"x":390,"y":407,"wires":[["c21e1db7.3bb47"],["29a9c115.47e45e"],["e1c348f1.0f20c"],["1f973ef2.4fede9"],["c3c99d61.2339c"]]},{"id":"de1fd0d7.5f07a8","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":860,"y":380,"wires":[]},{"id":"323cab4f.dc8b54","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":860,"y":320,"wires":[]},{"id":"f6db85bc.3d3778","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"false","complete":"false","x":330,"y":540,"wires":[]},{"id":"c21e1db7.3bb47","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":694,"y":440,"wires":[]},{"id":"29a9c115.47e45e","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":717.5,"y":496,"wires":[]},{"id":"e1c348f1.0f20c","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":717.5,"y":551,"wires":[]},{"id":"1f973ef2.4fede9","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":714.5,"y":592,"wires":[]},{"id":"c3c99d61.2339c","type":"debug","z":"e7fb9fb.21ef46","name":"","active":true,"console":"true","complete":"payload","x":708.5,"y":635,"wires":[]},{"id":"6cfe8b.d3fac174","type":"function","z":"e7fb9fb.21ef46","name":"payload","func":"node.log (\"function called\")\nvar msg1 = {payload: msg.payload[2]};\nreturn [ msg1];\n","outputs":"1","noerr":0,"x":332,"y":298,"wires":[["6e482785.6e1468"]]},{"id":"fe383a2e.538d78","type":"ttn app","z":"","appId":"pitlab-core","region":"eu","accessKey":"ttn-account-v2.XLXDZg4cTdT0X-4obr4F3A5StKGlt49VeqBMQl-QIRo"},{"id":"7714594.09e8ca8","type":"ttn app","z":"","appId":"dk-cph-itu-pitlab-01","region":"eu","accessKey":"ttn-account-v2.2ohV653kNiCAOPsmOf9u9KIBmAjJP3Wq62Ywv6fYzdM"},{"id":"2e1a5f7.ecc9aa","type":"influxdb","z":"","hostname":"127.0.0.1","port":"8086","protocol":"http","database":"pit001","name":"","usetls":false,"tls":""}]
```


