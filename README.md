# OSML-IoT-WNS
Wireless network of temperature, humidity, and light sensors on Raspberry Pi Zeros via Node-RED and BlueMix.

This project is a collaboration between Theo Noordeloos and Joy Lopez Cervera with help from Dan Hendricks at the Open Source Maker Labs.
The purpose of this project is to convert PiZeros into wireless network nodes that sample temperature and humidity via a DHT11 sensor, and samples light via a LDR sensor; feed the information via wireless through a Node-RED flow; visualize the data via the IBM IoT BlueMix cloud. This project adapts and integrates different recipes into this particular mix of features. I will link the addresses of the inspiration content below. For detailed instructions with pictures, visit my blog post here.

The project requires:

Hardware
3 PiZeros with internet (or internet dongles if using PiZeros or Pi 3s without internet)
3 PiZero power adaptors
1 Monitor with HDMI output
Laptop or Desktop
USB Keyboard
USB Mouse
1 HDMI cable
USB 2.0 to micorUSB hub
3 HDMI to microHDMI adaptors
Wi-fi USB dongle
40 pin Header
3 Cases (optional)
3 small bread boards
3 DHT11 sensors
3 LDR sensore
3 1 uF capacitors
Assorted wire
Solder wire
Flux
Soldering station
Sponge of wire wool
Painter's tape

Software
3 microSD cards loaded with the latest Raspbian Jessie OS
PUTTY or other SSH Client
Bluemix account
Node-RED for Raspberry Pi

Scripts
http://pastebin.com/raw/qwXLu0hu
https://pimylifeup.com/raspberry-pi-light-sensor/
Circuit diagram 1
Circuit diagram 2
Flow

Steps:

HARDWARE
Make Cases
Widen RPi holes
Assemble
Power Up
Connect to display monitor

SOFTWARE
Install Raspbian Jessie Image onto SD cards
Change keyboard settings
Connect to wi-fi
Setup SHH
Install Node-RED

CIRCUIT DIAGRAMS
Follow circuit
wire pins

PROGRAMMING
Download C Script onto RPi http://pastebin.com/raw/qwXLu0hu
Download Python Script onto RPi https://pimylifeup.com/raspberry-pi-light-sensor/
Run Node-RED
Make flow
Connect to BlueMix

let's Get Started!


HARDWARE
Solder header onto the top side of the Pi Zero boards.

Make Cases
Our lab has a laser cutter, so we custom-made cases for our PiZeros. You may find the schematics we used for our laser-cutter here https://www.thingiverse.com/make:282969. The schematics are bespoke for a non-wireless PiZero, but they can still be used for the wireless PiZeros; although the heat vent won't line up as nicely. If getting a Pi as part of a kit, like CanaKit, you can use the included case. You may order a Pi through CanaKit here: www.canakit.com.

Widen RPi holes
If using the bespoke laser-cut case like we did, I recommend enlarging the corner holes of the PiZero board. We used M3 bolts to secure the case, so we widened the holes using incrasingly larger drill bits.

Assemble
Stack the case components according to the thingiverse instructions. I recommend using tape to hold together the center of the case while screwing in the bolts onto the case.

Power Up
Plug to power cord.

Connect to display monitor
Use an HDMI to HDMImini adaptor to connect the display to the Pi Zero. Connect keyboard and mouse to a USB 2.0 hub. Connect the hub to USBmini to the board. If using a Raspberry Pi 3, use a USB wifi dongle to get wireless connectivity.


SOFTWARE
Install Raspbian Jessie Image onto SD cards
Get Raspbian Jessie with PIXEL from https://www.raspberrypi.org/downloads/raspbian/. Download onto a microSD card with an adaptor.

Change keyboard settings
If using PIXEL, click on raspberry icon on top left of GUI. Go to Preferences. Choose Mouse and Keyboard Settings, then follow the prompts for your keyboard and language. If using the command prompt, type 
  raspi-config
Choose Internationalization. Choose Keyboard setup.

Connect to wi-fi
On PIXEL, select the icon with two wires crossed out in the top right. Choose your network.

Setup SHH
Download Putty for your laptop http://www.putty.org/ or Bitvise https://www.bitvise.com/download-area. Allow SSH on Raspberry Pi by clicking on the raspberry icon on the top left of GUI. Go to Preferences. Select Raspberry Pi Configuration. Click on Preferences tab. select the on option for SSH. Enter the command 
  hostname -I
onto the command line. That is your IP address. Enter the IP address onto Putty or Bitvise to connect remotely to your raspberry pi. You should be asked for credentials. By default, your username is 'pi' and your password is 'raspberry'. Note, now you don't need a monitor connected to your pi unless you want it. You can completely control the raspberry pi wirelessly!

Install Node-RED
If you have the full pixel version of Raspbian Jessie running, Node-RED is already pre-installed and you don't need to install it. You may need to update it, however. From your secure shell, use this command to update Node-RED.
  sudo apt-get install nodered
Follow the instructions here https://nodered.org/docs/getting-started/upgradingfor upgrading npm to version 2.x. Once you have the latest npm version, download the node-red dashboard like this:
  cd ~/.node-red
  npm install node-red-dashboard
Use this command to stop Node-RED:
  node-red-stop
Use this command to start up Node-RED again:
  node-red-start


CIRCUIT DIAGRAMS
Follow circuit
From pin 2 on the Pi, connect a wire to the power pin on the DHT11.
wire pins

PROGRAMMING
Download C Script onto RPi
Download Python Script onto RPi
Run Node-RED
Make flow
Connect to BlueMix

