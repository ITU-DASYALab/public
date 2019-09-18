# Hookup-notes for STM32 Black Pill (and Blue Pill)

This fuide was in part inspired by [this youtube video](https://www.youtube.com/watch?v=MLEQk73zJoU), which can be used as reference for the connection between FTDI and Pill, and for the parameter setup in ARduino IDE. But it leaves out a couple of important steps, so do the following beforehand:

1. Download [Roger Clarks Arduino_STM32 repo](https://github.com/rogerclarkmelbourne/Arduino_STM32) as a  ZIP and unpack it in your arduino /harware folder
2. Install the "Arduino SAM Boards (32-bits ARM Cortex-M3)" boards through the Board manager in Arduino IDE
3. In arduino IDE go to File > Preferences and under "Additonal Board Manager URLs" add `http://dan.drown.org/stm32duino/package_STM32duino_index.json`  
4. Install the "STM32F1xx/GD32F1xx boards" in the board manager
5. Connect the pill via a 3.3v FTDI (as shown in the video) (Maybe its 5v for the blue pill)
6. Setup the board parameters as shown: <br/>
  - Board: “Generic STM32F103C series”
  - Variant: “STM32F103C8 (20k SRAM, 64k Flash)”
  - CPU Speed (MHz): “72MHz (Normal)”
  - Upload method: “Serial”
  - Optimize: “Smallest (default)”
7. Set the internal connector in the correct mode: `B0+` & `B1-` <br/> ![](https://blog.hobbycomponents.com/wp-content/uploads/2018/03/6.png)
8. UPLOAD!
9. After upload, move the B0 internal connector back to the `B0-` connection.
10. Restart and deploy.
