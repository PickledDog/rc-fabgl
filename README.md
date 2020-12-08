# rc-fabgl
 FabGL terminal board for RC2014

## Overview
This is my take on the [FabGL](http://www.fabglib.org/) dev kit, designed into the form factor of a RC2014 card with the intention of implementing a serial terminal.

## FabGL
[FabGL](http://www.fabglib.org/) is an excellent graphics/UI/Human Interface library for the Espressif ESP32. It provides VGA display generation, PS2 keyboard/mouse support, audio, and terminal emulation abilities. This gives us everything we need to build a serial terminal (and even provides examples to do just this). In addition, the ESP32 is first and foremost a WiFi module, opening up the possibility of "dial" in/out cabibilities.

## The ESP32
The common ESP32-WROOM module starts to run a bit short on memory when implementing a terminal, so the ESP32-WROVER is used instead - this adds an extra 8MB of RAM for us to use. The module is hooked up as described in Espressif's datasheet, with RC reset circuit and appropriate bypass caps.

## Functions provided
The standard dev kit schematic provides VGA, PS/2 keyboard, PS/2 mouse and line-level audio. The mouse port is removed, audio is sent directly into a piezo buzzer, and level-shifted serial ports are added to connect to the 5V serial ports of RC2014 systems. In addition, improvements have been made to the other peripheral connections.

### VGA
120Ω resistors are added to terminate the VGA connection (and limit the HSync and VSync pins). The design is similar to the Parallax Propeller VGA breakout. The resistor values were picked to stay within the drive capabilities of the ESP32 (which is why the correct 75Ω resistors weren't an option).

### PS/2 keyboard
The resistor PS/2 level-shifters are replaced with MOSFET shifters.

### Audio
Audio won't be used for much more than a terminal bell - as such, it's just connected to a buzzer. Make sure a piezo buzzer is used - the ESP32 can't drive a dynamic one without help.

### Serial ports
Serial ports are connected to the remaining pins on the ESP32. There are enough pins to implement two serial ports, with RTS/CTS flow control on the second port. Incoming 5V signals are shifted down to 3.3V with a 74LV125.

## Programming port
The standard RX/TX pins of the ESP32 are broken out to a standard "FTDI-style" header for programming. RESET and BOOT are connected to buttons on the top of the card.

## Bus connection
The terminal board uses the RC2014 bus for little more than power and to connect to the bus serial ports. Jumpers are provided to disconnect the ports from the bus as needed. The Ser1 interface is provided to the bus only, while Ser2 is available on both the bus and an FTDI-style header. Only the header provides the RTS & CTS pins.

## TODO
BOM, component references