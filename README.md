# spartan-edge-esp32-boot
The purpose of this library is to load the FPGA bitstream from SDCard to the on-board FPGA (xc7s15) using the on-board ESP32.

The software development environment is the [Arduino IDE](https://www.arduino.cc/en/Main/Software) with [ESP32 Boards support](https://github.com/espressif/arduino-esp32).

## Spartan Edge Accelerator Board
The Spartan Edge Accelerator Board can be used independently as an Arduino compatible board, or plugged into an Arduino Board as an auxiliary accelerator board.  
![pic](extras/spartan.png)

## Arduino IDE with ESP32 Boards Support

Using these steps, you will be able to talk to the ESP32 part of your Spartan Edge Accelerator Board. This is a prerequisite for the next steps; the FPGA is not involved yet.

### using Arduino IDE Boards Manager  
Starting with 1.6.4, Arduino allows installation of third-party platform packages using Boards Manager. We have packages available for Windows, Mac OS, and Linux (32 and 64 bit).

- Install the current upstream Arduino IDE at the 1.8 level or later. The current version is at the [Arduino website](http://www.arduino.cc/en/main/software).
- Start Arduino and open Preferences window.
- Enter ```https://dl.espressif.com/dl/package_esp32_index.json``` into *Additional Board Manager URLs* field. You can add multiple URLs, separating them with commas.
  - Stable release link: `https://dl.espressif.com/dl/package_esp32_index.json`  
  - Development release link: `https://dl.espressif.com/dl/package_esp32_dev_index.json`  
  - If you want more details, you can click the [link](https://github.com/espressif/arduino-esp32)
- Open the Board Manager from Tools -> Board menu and install *esp32* platform (and don't forget to select your ESP32 board from Tools -> Board menu after installation).
- Select _tool->board->DOIT ESP32 DEVKIT_

Test your setup by building and running a test program (e.g. [SoftwareSerialExample](https://www.arduino.cc/en/tutorial/SoftwareSerialExample)). You cannot access the GPIO pins yet, as these are connected to the FPGA not the ESP32.

1. Connect the Spartan Board through the USB Type-C wire to your computer, and install USB232 driver (CP2102) for your computer's operating system if necessary.
2. Turn the power switch near the USB Type-C slot to USB side to power on the board.
3. Copy and paste some code that talks to the serial port into the editor window
4. Open the serial console , and ensure that it is set to the correct baud speed.
5. Build and upload the code using the 'Upload' button, and watch the result on the serial port.


## Library Usage

Using these steps, you will be able to tell the ESP32 on the Spartan Edge Accelerator Board to program the FPGA out of a bitstream file on the SD card.

### Library Installation

Install this Git repository as an additional library, either from a checked out Git repository or from [the Zip file](https://github.com/marsfan/spartan-edge-esp32-boot/archive/master.zip). See [Installing Additional Arduino Libraries](https://www.arduino.cc/en/Guide/Libraries)

### Prepare the SD Card
  1. Format the SDCard with FAT16/FAT32 filesystem.  
  2. Create a top level subfolder named _overlay_ in the SDCard.  
  3. Put your bitstream files (with the extension .bit) into the folder _overlay_.  
  4. If you run example 01LoadDefaultBitstream, rename the bitstream file in _overlay_ to _default.bit_.  
  5. If you run example 02LoadConfigBitstream, put [**board_config.ini**](extras/board_config.ini) into SDCard root folder.  
  6. Insert the SDCard in the Spartan Edge Accelerator Board.  

### Upload example
  1. Connect the Spartan Board through a USB Type-C cable to the PC and power it up, as described above.
  2. Open Arduino IDE's serial console, and set the speed to 115200 baud.
  3. Open one of the library examples (01LoadDefaultBitstream or 02LoadConfigBitstream) by Arduino IDE.  
  4. Press the 'BOOT' Button on Spartan Board and hold it for more than 1 second to force ESP32 enter Bootloader mode.  
  5. Press the 'Upload' button in Arduino IDE to upload the example's compiled binary to ESP32.  At this stage, a mount error is will appear in the serial console saying `Card Mount Failed,please reboot the board`.

### Run example
  1. Make sure the on-board DIP-switch K5 is set to the on Slave (ON) side, which enables FPGA programing with the ESP32.  
  2. Press 'RST' button on Spartan Board to start the example you just uploaded.  
  3. After a few seconds, the FPGA_DONE (red) LED on the board will light on.

#### Library examples
  * 01LoadDefaultBitstream  
    This example will load SDCard file /overlay/default.bit to FPGA  

  * 02LoadConfigBitstream  
    This example will read a _ini_ format file /board_config.ini in SDCard,  
    then load the bitstream spcified by the value of key **overlay_on_boot** to FPGA.  

