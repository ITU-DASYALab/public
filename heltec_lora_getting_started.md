### heltec LoRA Wifi

Using

Heltec WiFi LoRa 32 (ESP32 + LoRa + OLED 0.96") 

bought here: https://nettigo.eu/products/heltec-wifi-lora-32-esp32-lora-oled-0-96



## install / start notes


following this and finding it works 100%:

https://robotzero.one/heltec-wifi-kit-32/

however, you also have to

[https://hackaday.io/project/26991-esp32-board-wifi-lora-32]

add following lines to your setup loop

```
pinMode(16,OUTPUT);
digitalWrite(16, LOW); // set GPIO16 low to reset OLED
delay(50);
digitalWrite(16, HIGH); // while OLED is running, must set GPIO16 to high 
```
