There are different ways to get data from our watermeters:

## MQTT

this is how you mqtt sub to a watermeter data stream:

mosquitto_sub -h eu.thethings.network -t '+/devices/+/events/activations' -u 'watermeter-test01' -P 'ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ' -v

more doc:

https://www.thethingsnetwork.org/docs/applications/mqtt/quick-start.html

## from influxdb

you could also get data from our influxdb -
like so:

## integration on TTN

or be added to the TTN app and add your own integration - ask us.
you could for example send data to your own URL or such.
