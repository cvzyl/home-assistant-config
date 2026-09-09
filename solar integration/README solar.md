# Solar Inverter Monitor README

Immense thanks go to [Syssi](https://github.com/syssi) for their guidance and help on this project. I could not have understood any of this without your patient input.

Other useful resources:
- [Cloned Shine-Wifi dongle](https://www.reddit.com/r/Esphome/comments/18lvgsb/made_my_own_growatt_shinewifif_dongle/)
- [Hacking Growatt Wifi-F modules](https://diysolarforum.com/threads/hacking-the-new-growatt-wifi-f-modules.43231/#post-550051)

## Components
- SACOLAR 5kVa solar inverter (M5000-H) or similar (inverters that can work with the Shine-Wifi-F dongle)
- ESP device (I used a WEMOS D1 Mini clone, the Lolin)
- MAX3232 serial to TTL converter
- USB A data cable (I cannibalised an only cable I had in a box)
- Various wires to connect components

You also need
- a soldering iron
- solder
- multimeter
- some basic soldering skills.

## Purpose
- read inverter statistics and data locally (not through the PVButler or other cloud service)
- integrate inverter data into Home Assistant
- control basic inverter functions on the inverter remotely (NOT YET ATTEMPTED - future project)
- read Fivestar LiFePo 10kVa battery statistics and data (NOT YET ATTEMPTED - future project)
- integrate battery data into Home Assistant (NOT YET ATTEMPTED - future project)

## Included documents
(see Github folder)
- Growatt MODBus protocols
- Photograph of Shine-Wifi dongle
- Photograph of soldered MAX3232 serial to TTL converter
- Schematic of connections

## Set-up
I used an old USB A to USB B data cable to interface the inverter with the ESP device.
The USB cable's data lines and GND are connected to to the MAX3232 serial to TTL converter.
The MAX3232's positive and negative pads are connected to the ESP device's GND and 3V3.
The TX/RX lines are connected to the ESP's GPIO4 and GPIO5.
Currently, the ESP is still powered by a separate USB-C cable as the set-up is just proof of concept and needs some refinement. As soon as the testing is done, I plan to power the ESP from the inverter's USB port via the USB cable's 5V line (see below).

### MODBus Protocols
I have included a PDF in the documentation folder with the complete MODBus protocol.
Some of these codes don't work entirely as expected. This might be due to errors on the document or variations in the different inverter models using these protocols. I have found that where there are two codes for a protocol for a *high* and *low* variation, that the *low* variation code works for my inverter. It might be different for other inverters, so play around.

I have also included my full ESPHome yaml file for anyone who would like to emulate this set-up.

### USB Cable
Apparently, USB data cables are all standardised and use the same colour scheme for wires. This is not true. However, you can figure it out quite easily. Ingeneral there are 4-wire and 5-wire data cables. They are supposed to be colour-coded as follows:
- Black (GND)
- Red (5V)
- White (D-)
- Green (D+)
- Various, even unshielded, for a common connection (for the 5-wire variants)

When I cannibalised an old cable I had around the house, the wires were:
- yellow
- red
- green
- white
- unshielded

I could not find any colour scheme online like this. Luckily I noticed a detail of all the photographs online: the power wires (GND and 5V) were always significantly thicker than the data wires. So I guessed that my white and green wires were D- and D+ while the two thicker ones had to be the power supply.
- I stripped and separated the five internal wires and then plugged the USB cable into my laptop.
- I tested the yellow and red wires for DC voltage. They were indeed measuring 5.1V witth red being positive.
- I tested the voltage between GND (yellow) and the data lines (white and green).
- the white wire had zero voltage; the green had abut 2V.
- I surmised that white must the be D- and green D+

## Next steps
- I want to upgrade from the current ESP8266 device to an ESP32 devices (see below)
- I need to clean up the build, power it from the USB data cable and place it inside a proper container
- I want to add battery monitoring capabilities
- A comprehensive Home Assistant dashboard for energy usage (which will ultimately include my water and gas usage)

### ESP8266 Memory Limitations
The MODbus protocol on ESP apparently takes up a lot of memory. This has proven to be a problem for OTA on my ESP8266 device. In order for OTA to work, the updated firmware needs to be uploaded alongside the original firmware which means that neither can be larger than 50% of the memory on the device. As such, I cannot update the device over the air; instead, I have to physically connect the device to my laptop so I can flash any updated firmware. When I add battery monitoring capability to my buuild, I believe that the ESP8266's memory will simply not be sufficient. Keep this in mind if you attemp this built.
