# ESPHome Electric Fence Controller (README)
This ESPHome Electric Fence Controller is for my Nemtek merlin M18S Energiser, but the principles should work on most models and brands with a little logical tweaking.

## Useful Resources:
- [Dr Zzs Garage Door Opener](https://www.youtube.com/watch?v=QMepwpyjMCY&t=25s) - initial idea
- [Flashing ESPHome to Sonoff SV](https://www.youtube.com/watch?v=4Q3whVVVwYw)
- [Werner Pieterson's Centurion D5 GitHub](https://github.com/wernerhp/esphome/blob/main/centurion-d5-evo/centurion-d5-evo.yaml) - good yaml for a variety of functions
- [Connecting electric fence as alarm zone](https://www.youtube.com/watch?v=rIPFD3r421A)
- Image of GPIO options ***NOTE*** 3.3V logic! (see documentation folder)
- Image of (non-)isolated mode (see documentation folder)
- Image of energiser inside (see documentation folder)
- Image of energiser connections (see documentation folder)
- Image of my Sonoff SV (see documentation folder)

## Basic Set-up
### Flashing Firmware
- Solder header pins to the Sonoff SV (see images).
- Go to ESPHome and create a basic yaml with your wifi credentials.
- Use a USB-to-TTL serial adapter to connect the SV to your computer.
- Hold the reset button on the SV and connect/pair the board through the ESPHome interface so you can flash - follow prompts.
- Disconnect.

### Prepare Sonoff SV
- Locate the voltage supply jumper header (close to the input pads below the relay itself; see pictures and video)
- Use a small flat screwdriver to remove the resistors around the jumper header
- Connect/Solder power supply wires to the isolated power supply header (see picture/video)
- Solder wires to the relay output (see picture/video - two different approaches, but both work)

### Install Sonoff SV on the energiser
- Unplug and switch off the energiser.

  ***NOTE:***
  It really is not a fun surprise to be jabbed by the energiser so don't be a hero: unplug AND switch off (there's battery back-up for a reason!). The energiser cannot be switched on while the housing is open. There is a dead-man's switch in there to prevent it. Good for safety; frustrating if you are trying to test things.

- Use an appropriately sized Allen key to open the energiser's housing.
- Connect the Sonoff SV's power supply wires to the 12V aux dry contact.
- Connect the relay output wires to the remote.
- Connect/solder/add any other components you are interested in.
- Close up the energiser's housing.
- Plug in the energiser and switch it on.

### Programme Energiser Controller
- Go to ESPHome and open the yaml file you created earlier.
- Edit to create a momentary switch on GPIO12 (the relay) - see example yaml.
- Add any other components you need (GPIO4, 5 and 14 are exposed on the board for use).

### Other Components
1. I added an alarm to GPIO14. I decided to hook up a stand-alone 12V relay (NO) to the siren dry contact; the relay connects to GPIO14 as a binary sensor which I then configured. See limitations below.
2. When I felt more adventurous, I decided to solder wires directly to the status LEDs so I could read their states directly as a binary sensor. I connected the on/off status indicator of the energiser to an optocoupler which connects to GPIO4. See limitations below.

## Limitations/Issues
1. The alarm indicator (via the siren's dry contact) works, but it switches off after about 2 minutes even though the alarm on the energiser is still firing. I'm unsure why. It may be that the relay freaks out after a while and resets. It may also be that the siren only sounds for that amount of time (I don't know because I do not have a physical siren connected to the energiser). I want to replace this alarm on the dry contact with an alarm directly from the status LED through an optocoupler.
2. Trying to figure out the polarity and current etc. of the LEDs was torture because the unit cannot switch on while the housing is open. You will need an extra hand to keep the dead-man's switch down or jerry rig some other way to depress it so you can switch on the energiser and test the LEDs (and not choke yourself every two minutes...).
3. The "remote" switch has a slight delay to it. It actually feels like operating the physical magnetic fob on the energiser. In other words, wait half a second to see if the energiser switches off before you push the button again.
4. I wanted a smooth "status armed" / "status not armed" sensor (on/off or flashing at a regular interval). This proved to be extremely difficult for someone who doesn't know much. The LED obviously flashes at a consistent rate. I thought I would be able to read that consistent on-off click and translate that into a "status armed" / "status not armed" sensor. It turns out that the output from the optocoupler is very erratic with no consistent interval and far more on-off cycles than are visible on the LED. I tried every variation in my yaml to try and get that sensor working and added a variety and several combinations of resistors after the optocoupler to try and debounce the signal, but I failed utterly. In the end, I came up with two work-arounds in Home Assistant itself:
- template sensor to output consistent "armed"/"unarmed" binary sensor.
- css styling of icon of a custom button with if-elif <br>
  `<ha-icon icon="mdi:fence-electric" style="width: 20px; height: 20px; animation: blink 1s ease infinite; color: red"></ha-icon>`

## Future Plans
- Connect alarm directly to status LED > optocoupler > GPIO14.
- Clean up and encase electronics.
