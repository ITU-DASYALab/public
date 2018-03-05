# Setting up a MQTT broker

(The whole thing takes about 30 minutes max - straight forward)


Following recommendation here, and to stay close to TTN,

https://www.thethingsnetwork.org/docs/applications/mqtt/

we go for Mosquitto:

https://github.com/mqtt/mqtt.github.io/wiki/libraries

starting with:

https://mosquitto.org/download/

which tell us to 
```
apt-get install mosquitto
```
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

```
mosquitto_sub -v -t 'test/topic'
```

Publish test message with the command line publisher:
```
mosquitto_pub -t 'test/topic' -m 'helloWorld'
```

works!

### mqtt-spy

Try mqtt-spy for some more testing:

http://kamilfb.github.io/mqtt-spy/

(depends on java jre 8+, opwnjfx - follow guide

Behaves a bit beta-ish but works.

MQTT.fx seems a good GUI client across platforms.

## Securing MQTT (somewhat)

### TLS/SSL

MQTT with TLS/SSL works very much the same as https (which people typically are familiar with from web usage).

Configuration also reminds of apache setup.

The fastes way to tets it might be letsencrypt certificates, skipping the more complicated path of doing your own CA, and requesting from that one:

https://mosquitto.org/man/mosquitto-tls-7.html


Example with letsencrypt certificates:

/etc/mosquitto/conf.d/ssl.conf
```
listener 8883
certfile /etc/letsencrypt/live/influx.itu.dk/cert.pem
cafile /etc/letsencrypt/live/influx.itu.dk/chain.pem
keyfile /etc/letsencrypt/live/influx.itu.dk/privkey.pem```
```

A mosquitto_pub client would publish like this:

```
mosquitto_pub -h influx.itu.dk -p 8883 --capath /etc/ssl/certs/ -t topic/test -m "encrypted msg"
```

### Users, passwords, ACL

A good guide is at

http://www.steves-internet-guide.com/mqtt-username-password-example/

This resembles htwpasswd quite a bit:

```
You create the password file using the command

mosquitto_passwd -c passwordfile user


to add additional users to the file:

mosquitto_passwd -b passwordfile user password


```

ACLs: take the example file that comes with the installation, in our case
```
/usr/share/doc/mosquitto/examples/aclfile.example
```

The manpage is here:

https://mosquitto.org/man/mosquitto-conf-5.html


```
cat /usr/share/doc/mosquitto/examples/aclfile.example
# This affects access control for clients with no username.
topic read $SYS/#

# This only affects clients with username "roger".
user roger
topic foo/bar

# This affects all clients.
pattern write $SYS/broker/connection/%c/state
```


