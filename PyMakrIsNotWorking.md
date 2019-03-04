# Stuff to try when PyMakr is bugging (v0.2)

Here's some possible solutions to common problems in your workflow with PyMakr and the LoPy. For each problem there are a series of steps. Maybe you don't need to do all of the steps. Just keep doing steps until it works. You will quickly learn the ways to fix these problems.

This document will be updated when new solutions or problems arise.

#### I cannot connect to the LoPy!
1. Make sure you have set the right USB port for the connection in the settings. (`More -> Get Serial Ports -> Settings`)
2. (If Linux:) Make sure your user is added to the `dialout` group (you might need to restart computer)
3. Unplug the device and plug it in again.
4. Restart Atom / VS Code

#### I can connect to the LoPy, but a lot of the buttons (upload, sync) are missing!
1. Right-click on the code in the editor and select `PyMakr -> Run Current File`. For some reason the `Alt+Ctrl+R` shortcut does not work for this.
2. Restart Atom / VS Code

#### Uploading code to the LoPy keeps failing!
When the LoPy is running blocking code, it sometimes does not respond very much, and you can't use the REPL. And sometime you can't upload code to it either.
1. Press the restart button on the LoPy.
2. Change the code in `boot.py` to the following:
```
import os
os.mkfs('/flash')
```
And run it on the LoPy. This flashes a clean file system to the device. The press the reset button and connect and try to upload the code again.
3. Unplug the device and plug it in again
4. Restart Atom / VS Code

### Other random problems and fixes that people have reported
 - (Windows) Auto-connect on Atom tried to connect to the wrong port. Turn off auto-connect and manually enter the right port name in the global settings.
