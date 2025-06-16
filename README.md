ESP32RET (Saab NG9-3 P-BUS 500kbps)
=======

ESP32RET allows you to use a ESP32 as a GVRET serial device. This can be used with the tool SavvyCAN to send/receive frames to the vehicle's 
CANBUS network.  

Setup Guide
=======

Requirements:

- SN65HVD230 transciever (the one I used is on a PCB, with pins 3V3, GND, CTX, CRX, CANH, and CANL)
- ESP32-S3 (ABSOLUTELY MUST BE THE S3 MODEL)


Wiring guide: 

| ESP32-S3    | SN65HVD230 |
| -------- | ------- |
| 3V3  | 3V3    |
| GND | GND     |
| GPIO 5    | CTX    |
| GPIO 4 | CRX |

CANH (CAN HIGH) on the SN65HVD230 goes to pin 6 (P-BUS high) on the OBD2 diagnostic connector

CANL (CAN LOW) on the SN65HVD230 goes to pin 14 (P-BUS low) on the OBD2 diagnostic connector

Open this repository in the Arduino IDE (or other), make sure you have the required libraries mentioned in the original README, and flash
to your ESP32-S3. 

Now you should be able to send/receive on the P-BUS on your NG9-3 2003-2011 vehicle, using the SavvyCAN application.
There are some good tutorials on YouTube on how to use the various features of the program.



Brief Explanation of NG9-3 CANBUS:
=======

There are two main CANBUS networks on the Saab 9-3:

2-wire P-BUS (powertrain): This is the main bus, running at 500kbps. ECU, ABS/ESP, and other important modules 
communicate through this bus. Please be careful with this side, as it controls many important safety/engine parameters.
This guide does not focus on this bus.

1-wire I-BUS (instruments): This is the slower bus, running at 33.3kbps, over a single-wire layout. Cluster (MIU), windows, seat memory, etc 
are some things on this bus. This document focuses on interfacing with this bus. 

Few potential ideas that can be achieved through sending/receiving on the I-BUS:

- Read in steering wheel button presses, use input to change song on Bluetooth-connected phone
- Read in speed value for measurements
- Automatically change source to AUX by replicating the SRC button frame

Few potential ideas that can be achieved through sending/receiving on the P-BUS:

- Get power and torque values sent out by the ECU
- Request OBD2 PIDS such as intake air temp, speed, etc.
- Get warning data sent out by ECU, or TCS/ESP/ABS module











 

 
 

Original README:
=======


ESP32RET
=======

Reverse Engineering Tool running on ESP32 based hardware. Supports both EVTV ESP32 and Macchina A0

There is a precompiled binary version of this program here:
https://www.savvycan.com/ESP32RET_Updater.zip


#### Requirements:

You will need the following to be able to compile the run this project:

- [Arduino IDE](https://www.arduino.cc/en/Main/Software) Tested on 1.8.13
- [Arduino-ESP32](https://github.com/espressif/arduino-esp32) - Allows for programming the ESP32 with the Arduino IDE
- [esp32_can](https://github.com/collin80/esp32_can) - A CAN library that supports the built-in CAN
- [esp32_mcp2517fd](https://github.com/collin80/esp32_mcp2517fd) - CAN library supporting MCP2517FD chips
- [can_common](https://github.com/collin80/can_common) - Common structures and functionality for CAN libraries

PLEASE NOTE: The Macchina A0 uses a WRover ESP32 module which includes PSRAM. But, do NOT use the WRover
board in the Arduino IDE nor try to enable PSRAM. Doing so causes a fatal crash bug.

The EVTV board has no PSRAM anyway.

This program is larger than the default partitioning scheme. You will need to use
a larger scheme. The recommended way to do this: Tools -> Partition Scheme -> Minimal SPIFFS

By default a wifi hotspot will be created by this firmware. The SSID is either ESP32RETSSID (for EVTV board) or
A0RETSSID (For Macchina A0). The default WPA2 password is "aBigSecret" (Minus the quote marks) You can configure
different settings from the serial port created when connected to USB. The serial port is 1 Megabit speed.

All libraries belong in %USERPROFILE%\Documents\Arduino\hardware\esp32\libraries (Windows) or ~/Arduino/hardware/esp32/libraries (Linux/Mac).

The canbus is supposed to be terminated on both ends of the bus. This should not be a problem as this firmware will be used to reverse engineer existing buses. However, do note that CAN buses should have a resistance from CAN_H to CAN_L of 60 ohms. This is affected by placing a 120 ohm resistor on both sides of the bus. If the bus resistance is not fairly close to 60 ohms then you may run into trouble.

#### The firmware is a work in progress. What works:
- CAN0 / CAN1 reading and writing
- Preferences are saved and loaded
- Text console is active (configuration and CAN capture display)
- Can connect as a GVRET device with SavvyCAN
- LAWICEL support (somewhat tested. Still experimental)
- Bluetooth works to create an ELM327 compatible interface (tested with Torque app)

#### What does not work:
- Digital and Analog I/O

#### License:

This software is MIT licensed:

Copyright (c) 2014-2020 Collin Kidder, Michael Neuweiler

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

