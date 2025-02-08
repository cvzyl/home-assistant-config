# Solar Installion README

Immense thanks go to [Syssi](https://github.com/syssi) for their guidance and help on this project. I could not have understood any of this without your patient input.

Other useful resources:
- [Cloned Shine-Wifi dongle](https://www.reddit.com/r/Esphome/comments/18lvgsb/made_my_own_growatt_shinewifif_dongle/)
- [Stuff](https://diysolarforum.com/threads/hacking-the-new-growatt-wifi-f-modules.43231/#post-550051)

## Components
- SACOLAR 5kVa solar inverter (M5000-H) or similar (inverters that can work with the Shine-Wifi-F dongle)
- ESP device (I used a WEMOS D1 Mini clone, the Lolin)
- MAX3232 serial to TTL converter
- USB A data cable (I cannibalised an only cable I had in a box)
- Various wires to connect components

You will also need a soldering iron, solder and some basic soldering skills.

## Purpose
- read inverter statistics and data locally (not through the PVButler or other cloud service)
- integrate inverter data into Home Assistant
- control basic inverter functions on the inverter remotely (NOT YET ATTEMPTED - future project)
- read Fivestar LiFePo 10kVa battery statistics and data (NOT YET ATTEMPTED - future project)
- integrate battery data into Home Assistant (NOT YET ATTEMPTED - future project)

#
