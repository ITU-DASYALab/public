# Pointers for using the BSFrance 32u4:

### Note:
This board is a clone of the Adafruit Feather 32u4 Lora, so often help found online for the feather can also be used for this board.

## Rough setup guide:

- Get an antenna plug it into the board (Maybe steal one from a LoPy)
- Get hardware zip from BSFrance and place in hardware-folder.
  - [Link to zip](http://bsfrance.fr/documentation/11355_LORA32U4II/BSFrance.zip)
  - Place in your /arduino/hardware-folder (often ~/Documents/Arduino/arduino.<VERSION>/hardware)
  - This will give you the LoRa32u4II board option.

- [If on Debian/Ubuntu:] Install Adafruit udev-rules
  - This will make you not go insane when trying to upload sketches.
  - [Link to instructions}(https://learn.adafruit.com/adafruit-arduino-ide-setup/linux-setup#udev-rules)

- Install "MCCI LoraWan LMIC Library" through Arduino library manager.
  - Normal TheThingsNetwork library does not work.

- Start from the examples bundled with the MCCI-library.
  - maybe the one called ttn-abp
     
     
### Resources for help:
- https://github.com/kersing/node-workshop/blob/master/lora32u4.md
- https://www.thethingsnetwork.org/labs/story/using-adafruit-feather-32u4-rfm95-as-an-ttn-node
