## Example

```
Real time 8.50
"metadata":{"time":"2019-02-18T07:50:47.507802742Z", ...}
Counter 328
```

(yes, we have a 1 hour offset - server time is different from real time)

Houses used

```
69 34 13 78
```

Liters of water, respectivley

Payload in hex, one byte for each reading

```
45 22 0d 4e
```

Binary

```
01000101 00100010 00001101 01001110
```
base64-encoded as 4 bytes, resulting in
```
"RSINTg=="
```


## What MQTT and http integrations receive
```
"payload_raw":"RSINTg=="
```


== What Node-red ttn-node shows
```
buffer[4]
[69,34,13,78]
```

This is an array of decimal values


## on the linux cmd line
```
echo `echo AAgAFQ== | base64 --decode`
(something unreadable) - why?
```

(it is a buffer, not a string!)


## why do we always end with "=="?

read

https://en.wikipedia.org/wiki/Base64

## Online encoders/decoders

https://cryptii.com/

(thanks to course students for showing me this)



