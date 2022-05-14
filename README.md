# PiAutoDim
![image](https://github.com/dandydanny/PiAutoDim/blob/master/screenshot.gif)
Based on the original from @dandydanny

Auto-Dimming for the Official RPi and Waveshare DSI Touchscreens

Some minor code fixes and added a version for the Waveshare DSI displays, which work the same as the RPi screens, but the dimming logic is inverted. That is , 255-0 instead of 0-255 with 255 being the darket instead of the brightest setting.


### What?
Code and schematic diagram to automatically dim the backlight of the Official RPi and Waveshare DSI Touchscreens. Based on [this light-sensor example](https://gpiozero.readthedocs.io/en/stable/recipes.html#light-sensor) from [GPIO Zero](https://www.raspberrypi.org/blog/gpio-zero-a-friendly-python-api-for-physical-computing/).

### Why?
1. I don't want to be blinded by the bright LCD backlight at night and don't want my kitchen lit up like Christmas
1. Provide dynamic display brightness adjustment for optimal display contrast in all lighting conditions

### Hardware Setup
Parts needed:
* [Cds photo cell / photoresistor](https://www.adafruit.com/product/161) ([photo transistor](https://www.adafruit.com/product/2831) OK)
* Any 10µF capacitor - voltage doesn't matter. If using 1µF, edit `charge_time_limit` argument in `autobrightness.py` so that the `charge_time_limit=0.01`
* [Female jumper wires](https://www.adafruit.com/product/1951)

Connection diagram:

You can use either 3.3V pin (1 or 17) on the GPIO header, any GND (6, 9, 14, 20, 25, 30, 34 or 39) and pin 12 for the input.

The photoresistor is not polarity sensitive, so you can connect the 3.3V line to either leg. The capacitor is polarity sensitive, so the positive leg needs to be connected to the same wire as the second leg on the photoresitor and both terminate at pin 12 on the RPi. The negative leg can go to any available GND.

![image](https://github.com/morphias2004/PiAutoDim/blob/master/Wiring%20Diagram.PNG)

### Software Setup

Install GPIO Zero

`sudo pip3 install gpiozero`

For Waveshare screens, the backlight range is inverted when compared to an official RPi screen. 255-0 instead of 0-255.

To deal with this, the logic needs to be inverted. Use the `wvautobrightness.py` file instead. 

Copy the `autobrightness.py` file to `~/PiAutoDim` If using the `wvautobrightness.py` file, rename it to `autobrightness.py` before copying it over.

Permissions on the `brightness` file are not retained between reboots, so it needs to have the permissions added
at boot.

`crontab -e`

add the line for the RPi display

`@reboot sudo chmod 766 /sys/class/backlight/rpi_backlight/brightness`

or for the Waveshare display

`@reboot sudo chmod 766 /sys/waveshare/rpi_backlight/brightness`

and to make the script run in the background on boot, also add

`@reboot python3 ~/PiAutoDim/autobrightness.py`

The backlighting with adjust dynamically based on the amount of light falling on the sensor. The default update interval is 1 second.

### License

MIT

### About
http://www.ipind.com.au
