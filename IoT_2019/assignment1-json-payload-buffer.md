## what should a full json object look like?

```
s:1427:"{"app_id":"watermeter-test01","dev_id":"watersim_sebastian_01_thingsuno15","hardware_serial":"0004A30B001FD9C9","port":1,"counter":126,"payload_raw":"BAIHAg==","metadata":{"time":"2019-02-03T20:06:33.238050358Z","frequency":867.5,"modulation":"LORA","data_rate":"SF7BW125","coding_rate":"4/5","gateways":[{"gtw_id":"eui-0000024b0803147b","timestamp":354672284,"time":"","channel":5,"rssi":-31,"snr":7.2,"rf_chain":0,"latitude":55.6591,"longitude":12.59098,"altitude":-19},{"gtw_id":"eui-0000024b0805024b","timestamp":2731368028,"time":"2019-02-03T20:06:33.216603Z","channel":5,"rssi":-89,"snr":1.8,"rf_chain":0,"latitude":55.65989,"longitude":12.59097,"altitude":30},{"gtw_id":"pitlab3-multitech","gtw_trusted":true,"timestamp":100763060,"time":"2019-02-03T20:30:15Z","channel":5,"rssi":-37,"snr":6.75,"rf_chain":0,"latitude":55.65961,"longitude":12.591463,"altitude":30,"location_source":"registry"},{"gtw_id":"eui-b827ebfffea091b4","timestamp":188201012,"time":"2019-02-03T20:06:33.227586Z","channel":5,"rssi":-33,"snr":6.8,"rf_chain":0,"latitude":58.95688,"longitude":-3.30104,"altitude":5},{"gtw_id":"pitlab-things-gateway","gtw_trusted":true,"timestamp":2757009372,"time":"2019-02-03T20:06:33Z","channel":5,"rssi":-29,"snr":6.75,"rf_chain":0}]},"downlink_url":"https://integrations.thethingsnetwork.org/ttn-eu/api/v2/down/watermeter-test01/water-sim-01-http?key=ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ"}";
```


## how do i get from buffer_raw to string in node-red

(which really should be the same in general js):

```
var msgAll = { payload: msg.payload};
var msg0 = { payload :msg.payload[0] };
var msg1 = { payload :msg.payload[1] };
var msg2 = { payload: msg.payload[2] };
var msg3 = { payload: msg.payload[3] };
return [ msg0, msg1, msg2, msg3];
```

## more than once an hour?

also, if you d like to test code on something that sends more often than once an hour -

this app sends a temperature every 2 minutes:

pitlab-ds18b20
app key:
ttn-account-v2.D7-uy5QHbEvbjOv65hV_DI4chfWPAC0tWTGNQoiGYno
