

this is how you mqtt sub to a watwermeter data stream:

mosquitto_sub -h eu.thethings.network -t '+/devices/+/events/activations' -u 'watermeter-test01' -P 'ttn-account-v2.S6_w-wpG19AvLeUNjorYMhx4uctru7dmed_fcTwyvlQ' -v
