
## Assignment 1 - Water-measurement based services

Haveforeningen (HF) Sundbyvester is planning to set up metering for water, electricity, and potentially many other "smart" things for their group of about 163 houses, physically located close to ITU, https://www.openstreetmap.org/way/92161107

In this assignment, we focus on services based on water measurement. Consider that Lora-equipped meters are deployed in each house and made available through a Things Network gateway. See https://www.thethingsnetwork.org/docs/applications/

You are provided with a simulated stream of water meter data measured hourly. More details on the format of the data and how it is accessible available on the class github repo (see Sebastian's post).

Your goal is to design and implement (i) a billing service , and (ii) leak detection alert system. You should hand in a report that (i) precises the requirements and specifies the services you develop, (ii) describe the design of the services with a focus on your choice of architecture and components and (iii) describe your implementation and (iv) open issues that are raised and should be addressed in future  work.

We ask you to be inventive! Course lecturers and TA will assist you in your access to data, and deploying your backend.

Last modified: Monday, 4 February 2019, 11:39

## Getting data from our watermeters

We provide four simulated "houses", respectively their watermeters' hourly water consumption.

We send values once an hour, all four houses in one payload.

You can see them here:

http://130.226.140.2:3000/d/1V60Yo_mz/watermeters-simulations?orgId=1&from=now-24h&to=now



### MQTT

this is how you subscribe to a mqtt topic - giving you data for a specified application:

```
mosquitto_sub -h eu.thethings.network -t '+/devices/+/up' -u 'watermeter-test01' -P 'ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ' -v
```
An earlier version had this, by mistake - it only gives you device activations:

```
mosquitto_sub -h eu.thethings.network -t '+/devices/+/events/activations' -u 'watermeter-test01' -P 'ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ' -v
```

you will recive json objects.



more doc:

https://www.thethingsnetwork.org/docs/applications/mqtt/quick-start.html



### Integrations via TTN

Please see

https://www.thethingsnetwork.org/docs/applications/

for an overview of available integrations, APIs, SDKs.

In case you d like to add your own, e.g. an http integration, pls ask us.

### Example json

The full json contains a lot more than you ll need - a lot of metadata.

The actual data is in the payload_raw field, and needs to be decoded.

```
s:1448:"{"app_id":"watermeter-test01","dev_id":"watersim_sebastian_01_thingsuno15","hardware_serial":"0004A30B001FD9C9","port":1,"counter":125,"payload_raw":"CxIKDg==","metadata":{"time":"2019-02-03T19:06:31.01525255Z","frequency":868.1,"modulation":"LORA","data_rate":"SF7BW125","coding_rate":"4/5","gateways":[{"gtw_id":"eui-b827ebfffea091b4","timestamp":880941587,"time":"2019-02-03T19:06:30.993943Z","channel":0,"rssi":-33,"snr":10.8,"rf_chain":1,"latitude":58.95688,"longitude":-3.30104,"altitude":5},{"gtw_id":"eui-0000024b0803147b","timestamp":1047411707,"time":"2019-02-03T19:06:30.989912Z","channel":0,"rssi":-33,"snr":10.5,"rf_chain":1,"latitude":55.66,"longitude":12.5916,"altitude":2},{"gtw_id":"eui-0000024b0805024b","timestamp":3424111539,"time":"2019-02-03T19:06:30.989908Z","channel":0,"rssi":-91,"snr":10.2,"rf_chain":1,"latitude":55.65988,"longitude":12.59104,"altitude":33},{"gtw_id":"pitlab-things-gateway","gtw_trusted":true,"timestamp":3449752075,"time":"2019-02-03T19:06:30Z","channel":0,"rssi":-33,"snr":11,"rf_chain":1},{"gtw_id":"pitlab3-multitech","gtw_trusted":true,"timestamp":793509683,"time":"2019-02-03T19:30:12Z","channel":0,"rssi":-37,"snr":10,"rf_chain":1,"latitude":55.65961,"longitude":12.591463,"altitude":30,"location_source":"registry"}]},"downlink_url":"https://integrations.thethingsnetwork.org/ttn-eu/api/v2/down/watermeter-test01/water-sim-01-http?key=ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ"}";
```
### Encding / decoding

Some more info:

https://www.thethingsnetwork.org/docs/devices/bytes.html

## InfluxDB

We have an InfluxDB available on the server training.itu.dk -

and you may request accounts on that server (just give us your public ssh key).

```
sebastian@training:~$ influx
Visit https://enterprise.influxdata.com to register for updates, InfluxDB server management, and monitoring.
Connected to http://localhost:8086 version 1.1.1
InfluxDB shell version: 1.1.1
> help
Usage:
        connect <host:port>   connects to another node specified by host:port
        auth                  prompts for username and password
        pretty                toggles pretty print for the json format
        use <db_name>         sets current database
        format <format>       specifies the format of the server responses: json, csv, or column
        precision <format>    specifies the format of the timestamp: rfc3339, h, m, s, ms, u or ns
        consistency <level>   sets write consistency level: any, one, quorum, or all
        history               displays command history
        settings              outputs the current settings for the shell
        exit/quit/ctrl+d      quits the influx shell

        show databases        show database names
        show series           show series information
        show measurements     show measurement information
        show tag keys         show tag key information
        show field keys       show field key information

        A full list of influxql commands can be found at:
        https://docs.influxdata.com/influxdb/latest/query_language/spec/

> 
```

