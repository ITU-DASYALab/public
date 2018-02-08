## Hardware / Parts

You are receiving three parts:

 - a LoPy - https://pycom.io/product/lopy/
 - an Expansion Board, to provide a USB micro port, connectivity to your computer) - https://pycom.io/product/expansion-board-2-0/
 - an Antenna kit / Antenna and "pigtail" (cable) - https://pycom.io/product/lora-antenna-kit/
 
 ![alt text](https://github.com/ITU-PITLab/F2018_PervasiveComputing/blob/master/img/pycom_lopy.jpg "LoPy")
  ![alt text](https://github.com/ITU-PITLab/F2018_PervasiveComputing/blob/master/img/pycom_expBoard.jpg "ExpansionBoard")
   ![alt text](https://github.com/ITU-PITLab/F2018_PervasiveComputing/blob/master/img/pycom_antenna.jpg "Pycom Antenna")
 

 
 ### Remarks on hardware
 
 - Never run a radio chip (such as WiFi or LoRa radio) without an antenna or terminator on its output! the reflected wave might damage the chip.
 - Keep an eye on those little black jumpers on the expansion board! if they come off (and they do!), things will not work.
 - Dont push the micro USB connector too hard - this part of the board loves to break.
 - Carefully check the orientation of the LoPy board relative to the Expansion Board - different versions of the LoPy board have the print turning different ways! Check pin alignment!
 
 
## Software 


### python and micropython

We will be prograamming the LoPy in MicroPython,
a Python 3.5 implementation that is optimised to run on microcontrollers.

About micropython:

 - MicroPython is a software implementation of the Python 3 programming language, written in C, that is optimized to run on a microcontroller, without an operating system underneath, directly "on hardware".
 - Knows only two files - when booting, these two files are executed automatically: first boot.py and then main.py.
 

If you are not familiar with python, here is some start helpers:

https://python.swaroopch.com/

https://www.codecademy.com/learn/learn-python

https://docs.pycom.io/chapter/gettingstarted/micropython.html




### Documentation for pycom devices, MicroPython etc

https://docs.pycom.io

### Atom 

We will be using the Atom pymakr plugin to communicate with the pycom device. So, first

Install the Atom editor on your laptop:

https://atom.io/

### Atom Pymakr plugin

Install the Atom Pymakr plugin:

https://atom.io/packages/pymakr


### Connecting to the LoPY

## Serial connection through USB micro

Serial connection via USB cable is our preferred choice - rather than WiFi.

- On some OSs (Windows 7), you might need to install a serial driver.
- Find out what your serial port is called (e.g. via our OSs Device Manager or such). On Linux, it s typically ttyUSB0.
- Follow pymakr instructions within Atom to connect from within Atom/pymakr





### Hello World, LEDs and other initial fun
 
 - Try the interactive prompt (the REPL) -
 
 Note: here s some useful shortcuts for interacting with the MicroPython REPL:

  ```
    Ctrl-A on a blank line will enter raw REPL mode. 
            This is similar to permanent paste mode, except that characters are not echoed back.
    Ctrl-B on a blank like goes to normal REPL mode.
    Ctrl-C cancels any input, or interrupts the currently running code.
    Ctrl-D on a blank line will do a soft reset.
    Ctrl-E enters ‘paste mode’ that allows you to copy and paste chunks of text. Exit this mode using Ctrl-D.
  ```
 Now, let s say "hello world":
 
 ```
 >>> print 'hello world!'
  ```
  
  - Stop WiFi AP - per default, the LoPys start running a WiFi Access Point - that is a bad idea in many ways (in what ways?)
  This is how you stop it:
  
  ```
from network import WLAN
  # turn off Wifi
wlan.deinit()
print('Turned off WiFi')
 ```


 
Open a new window, and paste code into it, then run that code:

 This script will let you find your device's LoRa device EUI (identifier) - we will need that later:

 ``` 
#get your device EUI
from network import LoRa
import binascii
lora = LoRa(mode=LoRa.LORAWAN)
print(binascii.hexlify(lora.mac()).upper().decode('utf-8'))
 ```
 
 If you really need a WiFi AP for short periods of time - this is how you start it
 
  ``` 
 from network import WLAN
wlan.init(mode=WLAN.AP, ssid='aTemporarySSID', auth=(WLAN.WPA2,'yourPassword'), channel=7, antenna=WLAN.INT_ANT)
 ``` 
 
 From the pycom API examples, https://docs.pycom.io/chapter/tutorials/all/
 
find the LED example and make the LED blink.

You should now be familiar with the basics of LoPy and pymakr -
and ready to take on more challenging code examples, such as sensors and networking.
