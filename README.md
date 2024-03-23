# rio2rgb-pcb
NI Roborio to 5V addressable LED interface.

## INTRODUCTION

This project was developed by FRC team 9234 as a fun and simple project to teach electronic design, assembly and firmware development to students. PCB converted into macarons were distributed as gifts during the “Festival de robotique Regional 2024” event.


## SIMILARITIES WITH THE REV BLINKIN MODULE

This project offers, with some extras and a few limitations, a feature set that is comparable to the REV Blinkin Module

| Feature                                         | Team 9234 rio2rgb project                                                                                                                                  | REV Blinkin                                                                                                    |
|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| Led strips support                              | Addressable 5V WS2812 based (REV-11-1198 or similar) only                                                                                                  | Addressable 5V WS2812 based (REV-11-1198 or similar),  Non addressable 12V led strips (REV-11-1197 or similar) |
| Build-in 5V converter                           | Yes - up to 3A                                                                                                                                             | Yes - up to 5A                                                                                                 |
| Support for external 5V converter               | Yes                                                                                                                                                        | Possible - requires a custom made cable and unsupported by REV                                                 |
| Pattern selection                               | PWM,  i2c (future firmware), Wi-Fi (future firmware)                                                                                                       | PWM only                                                                                                       |
| Pattern and brightness adjustment               | 1 parameter (or brightness) through PWM, 2 parameters + brightness through i2c (future firmware), 2 parameters + brightness though Wi-Fi (future firmware) | 2 parameters through physical push buttons, brightness through physical potentiometer                          |
| Team color selection                            | 2 colors through i2c (future firmware), 2 colors though Wi-Fi (future firmware)                                                                            | 2 colors through physical potentiometer                                                                        |
| Status led                                      | No                                                                                                                                                         | Yes                                                                                                            |
| Open source firmware                            | Yes                                                                                                                                                        | Yes                                                                                                            |
| PWM pulse width compatible with the REV Blinkin | Yes                                                                                                                                                        | Of course!                                                                                                     |

If you need any of the features available on the REV Blinkin but not on this project, don’t hesitate to buy the REV Blinkin, it’s a good product and you won’t be disappointed.

If your motivation is only to save $$ because you believe that you can assemble one of these for cheaper than purchasing a REV Blinkin … do yourself a favor and buy a REV Blinkin, this project was designed to learn/teach electronic/coding skills not to save $$.


## COMPONENT SOURCING

Most components can be sourced from Digikey. As a global FRC sponsor, each year, they offer a 50$ voucher to each team. This project could be a nice way to spend your voucher if you haven’t used it yet. You will be able to procure all components but the DC/DC converters for less than the voucher value.


## POWER OPTIONS

RGB led strips can be quite power hungry, at full power, while displaying white light, each light will consume 0.3 watts of power. Led strips are available in a variety of density, a 60 leds/m will exhibit a worst case power consumption of 3.6A/m.

In real world usage power consumption should be less than this. There’s also features in the fastled library to soft-limit the power consumption so that you don’t exceed your dc/dc converter capacity.

Two dc/dc converter options are possible.
LM2596S pre-built module soldered on PCB. This is the simplest and cheapest option but only works if your total led strips current draw doesn’t exceed 3A.
The part number 106990003-ND was unfortunately not available from Digi-Key at the time of writing this guide but they are widely available from Amazon, Aliexpress, Abra (PSM-2596) and other sources.
If you use this option, make sure to ADJUST THE OUTPUT VOLTAGE TO 5V BEFORE SOLDERING THE MODULE TO THE PCB.
External dc/dc. Any external dc/dc converter than can convert 8V-14V to 5V will work.
Make sure to limit the current draw to no more than 3A per LED output. Anything above this could damage the PCB and the wires from the PCB to the led strips. The project was tested successfully with Addison 500638 and Szwengao WG-1224S0505 converters.


## KNOWN ISSUES OF PCB VERSION 1
At the time of witing, there’s 2 versions of the PCB. Version 1 contains several issues:
+ SENSOR_VP, SENSOR_VN, IO34 and IO35 pins are used to drive LED … however those pins are all input only. The easiest way to fix this is to bridge pin 3 with pin 7, pin 4 with pin 8, pin 5 with pin 9 and pin 6 with 10. This can easily be done using any small gauge solid wire such as wire wrap wire.
+ TX0O and RXD0 are used respectively for PWM1 and PWM2 signals. This doesn’t create any problem if you only use PWM1 but the PWM2 signal will conflict with the output of the RS232 level shifter already present on the ESP32-DEVKIT-C that shares this pin.

Both issues were fixed with the version 2 of the PCB.

During the “Festival de robotique Regional 2024” our team distributed about 400 Version 1 PCB and 100 Version 2 PCB. The exact version of the PCB can be seen on the silkscreen of the bottom side of the PCB.

<img src="assemblyPictures/rio2pwmv2back.jpg?" width="400"> | <img src="assemblyPictures/rio2pwmv1backassembled.jpg?" width="400">


## ADDRESSABLE LED SOURCING
Addressable LED modules can be purchased from multiple sources. Our team had great success using the BFT lights WS2812Beco module purchased on both Amazon and Aliexpress. We are confident that the regular WS2812B based stripleds or the strip leds sold by REV Robotics ( (REV-11-1198) will be just as good if not better. Make sure to also purchase a XXX to dupont or bare wire cables to interface your led strip with this module.


## FABRICATION OUTPUTS
This project was design with KiCad, this repository contains all the original KiCad files as well as gerber and NC drill files. The PCBs are pre-panelized so that 4 PCBs can be made out of a single low-cost 100mm x 100mm panel. If you want a single PCB version you’ll have to modify the supply KiCad project and generate new gerber and NC drill files.

Ordering 10 panels will yield 40 PCBs and should cost less than 10 USD + shipping using common China based prototype PCB manufacturers such as JCL PCB, Elecrow or Seeed.


## ASSEMBLY
All PCBs distributed by our team have lead-free HASL surface finish. A lead free alloy such as SAC305 is recommended for assembly. We recommend starting with the surface mount components first and then proceed with the through hole components. The most difficult part to solder is the 74LS125 logical level shifter, make sure that no pins are bridged after soldering. It could be a good idea to order a few more just to practice.

<img src="assemblyPictures/rio2pwmv1frontassemblednoesp32.jpg?" width="400">  |  <img src="assemblyPictures/rio2pwmv1frontassembledesp32.jpg?" width="400">

 
## FIRMWARE
The firmware is available on Github here https://github.com/FRC9234/rio2rgb-pcb. This firmware is based on the popular FastLED library and on the original REV Blinkin firmware. When used in PWM mode, it is 100% compatible with the latter so that it can be controlled as specified by the “led pattern table” part of the REV Blinkin user manual.


## DISCLAIMER
Neither REV Robotics nor any of the Blinkin firmware contributors endorse or promote this project.
