# rc-fabgl
 FabGL terminal board for RC2014
![Assembled PD104](/img/assembled.jpg)

## Overview
This is my take on the FabGL dev kit, designed into the form factor of a RC2014 card with the intention of implementing a serial terminal.

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
The terminal board uses the RC2014 bus for little more than power and to connect to the bus serial ports. Jumpers are provided to disconnect the ports from the bus as needed. The Ser1 interface is provided to the bus only, while Ser2 is available on both the (Enhanced) bus and an FTDI-style header. Only the header provides the RTS & CTS pins. Since Ser2 is the *only* thing on the Enhanced bus, consider the wider connector optional - a cheaper 1x40 header can be used in place of the 2x40 one (you'll need a [6-pin Dupont cable](https://www.ebay.com/sch/i.html?_nkw=6+pin+dupont+cable) to hook up Ser2).

### Initial setup
The ESP32 module will need to be programmed before use - this is done using an FTDI adapter on the assembled board, via the *Prog* port. If you don't have an FTDI adapter, they can be [found on eBay](https://www.ebay.com/sch/i.html?_nkw=ftdi+adapter). I don't have a firmware bundle made up for this yet, so for now consider this a dev kit for hacking (as opposed to a plug-and-play terminal board). To get started with FabGL, check out Fabrizio Di Vittorio's [YouTube channel](https://www.youtube.com/user/fdivitto/videos) and the [FabGL reference](http://www.fabglib.org/). Compiling FabGL and its examples requires the [Arduino IDE](https://www.arduino.cc/en/software). Once Arduino is up and running, follow the instructions in [Fabrizio's video](https://www.youtube.com/watch?v=8OTaPQlSTas) to add the ESP32 board package and FabGL library. Be sure to select *ESP32 Wrover Module* from *Tools* → *Board* → *ESP32 Arduino*. Examples are found in *File* → *Examples* → *FabGL* → *VGA*. 

## Part selection
Bill Of Materials and part references are below. If you want the 60-pin RC2014 Enhanced bus connector, you'll need to make it from a 2x40-pin right-angle header and pull out half of the pins from the top row (needle-nose pliers work well). I recommend using gold-plated header for this - I use these ones from [Pololu](https://www.pololu.com/product/2668) or [Sparkfun](https://www.sparkfun.com/products/12792). The jumper headers can be snapped from regular (or double-row) breakaway header, for which eBay is substantially cheaper; the Mouser listings are provided for convenience.

The specified parts are just the ones I used, and can be substituted as needed - Mouser links provided for convenience and reference.

| Reference | Value | Qty | Mouser link |
| --------- | ----- | --- | ----------- |
| BZ1 | Piezo buzzer | 1 | [TDK PS1240P02BT](https://www.mouser.com/ProductDetail/810-PS1240P02BT) |
| C1 | 4.7μF ceramic | 1 | [TDK FG18X5R1E475KRT06](https://www.mouser.com/ProductDetail/FG18X5R1E475KRT06) |
| C2 | 1μF ceramic | 1 | [TDK FG18X5R1H105KRT06](https://www.mouser.com/ProductDetail/FG18X5R1H105KRT06) |
| C3 | 22μF electrolytic | 1 | [KEMET ESS226M016AC2AA](https://www.mouser.com/ProductDetail/ESS226M016AC2AA/) |
| C4 | 10μF ceramic | 1 | [TDK FG18X5R1E106MRT06](https://www.mouser.com/ProductDetail/FG18X5R1E106MRT06) |
| C5-C7 | 100nF ceramic | 3 | [KEMET C315C104M5U5TA](https://www.mouser.com/ProductDetail/C315C104M5U5TA7303) |
| J1/2 | 2x40 right-angle header | 1 | [3M 2380-5121TG](https://www.mouser.com/ProductDetail/2380-5121TG) |
| J3 | 2x6 right-angle header | 1 | [Amphenol 10129382-912002BLF](https://www.mouser.com/ProductDetail/10129382-912002BLF) |
| J4 | 6-pin Mini-DIN socket | 1 | [Kycon KMDGX-6S-BS](https://www.mouser.com/ProductDetail/806-KMDGX-6S-BS) |
| J5 | D-Sub HD-15 Female | 1 | [TE 2311763-1](https://www.mouser.com/ProductDetail/571-2311763-1) |
| JP1/2 | 2x2 header | 1 | [Amphenol 10129381-904002BLF](https://www.mouser.com/ProductDetail/10129381-904002BLF) |
| JP3/4/5/6 | 2x4 header | 1 | [Amphenol 10129381-908002BLF](https://www.mouser.com/ProductDetail/10129381-908002BLF) |
| Q1, Q2 | 2N7000 enh N-MOSFET | 2 | [ON Semi 2N7000](https://www.mouser.com/ProductDetail/2N7000) |
| R1 | 10kΩ resistor | 1 | [Yageo CFR-25JT-52-10K](https://www.mouser.com/ProductDetail/CFR-25JT-52-10K) |
| R2-R5 | 100kΩ resistor | 4 | [Yageo CFR-25JT-52-100K](https://www.mouser.com/ProductDetail/CFR-25JT-52-100K) |
| R6-R9 | 4k7Ω resistor | 4 | [Yageo CFR-25JT-52-4K7](https://www.mouser.com/ProductDetail/CFR-25JT-52-4K7) |
| R10-R14 | 120Ω resistor | 5 | [Yageo MFR-25FTF52-120R](https://www.mouser.com/ProductDetail/603-MFR-25FTF52-120R) |
| R15-R17 | 261Ω resistor | 3 | [Yageo MFR-25FRF52-261R](https://www.mouser.com/ProductDetail/603-MFR-25FRF52-261R) |
| R18-R20 | 510Ω resistor | 3 | [Yageo MFR-25FTE52-510R](https://www.mouser.com/ProductDetail/603-MFR-25FTE52-510R) |
| SW1, SW2 | Right-angle tact sw 3.9mm | 2 | [C&K PTS645VL392LFS](https://www.mouser.com/ProductDetail/611-PTS645VL392) |
| U1 | MCP1825S-330 | 1 | [Microchip MCP1825ST-3302E/DB](https://www.mouser.com/ProductDetail/579-MCP1825ST3302EDB) |
| U2 | ESP32-WROVER-E | 1 | [Espressif ESP32-WROVER-E(M213EH3264PH3Q0)](https://www.mouser.com/ProductDetail/356-ESP32WRVE23264PC) |
| U3 | 74LV125 | 1 | [TI SN74LV125AN](https://www.mouser.com/ProductDetail/595-SN74LV125AN) |