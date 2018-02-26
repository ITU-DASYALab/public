# Setting up a MQTT broker

Following recommendation here, and to stay close to TTN,

https://www.thethingsnetwork.org/docs/applications/mqtt/

we go for Mosquitto:

https://github.com/mqtt/mqtt.github.io/wiki/libraries

starting with:

https://mosquitto.org/download/

which tell us to 

apt-get install mosquitto

The server listens on the following ports:

```
    1883 : MQTT, unencrypted
    8883 : MQTT, encrypted
    8884 : MQTT, encrypted, client certificate required
    8080 : MQTT over WebSockets, unencrypted
    8081 : MQTT over WebSockets, encrypted
```

all of these come up automagically after install - so the broker seems to be running.

```
root@NewThing:/home/sebastian# ps aux | grep mosq
mosquit+ 28508  0.0  0.0  42128  4876 ?        S    13:36   0:00 /usr/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf
```


## Test -

https://stackoverflow.com/questions/26716279/how-to-test-the-mosquitto-server

Start the command line subscriber:
mosquitto_sub -v -t 'test/topic'

Publish test message with the command line publisher:
mosquitto_pub -t 'test/topic' -m 'helloWorld'

works!

### mqtt-spy

Try mqtt-spy for some more testing:

http://kamilfb.github.io/mqtt-spy/

(depends on java jre 8+, opwnjfx - follow guide
