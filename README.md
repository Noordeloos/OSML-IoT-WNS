# OSML-IoT-WNS
Wireless network of temperature, humidity, and light sensors on Raspberry Pi Zeros via Node-RED and BlueMix.

This project is a collaboration between Theo Noordeloos and Joy Lopez Cervera with help from Dan Hendricks at the Open Source Maker Labs.
The purpose of this project is to convert PiZeros into wireless network nodes that sample temperature and humidity via a DHT11 sensor, and samples light via a LDR sensor; feed the information via wireless through a Node-RED flow; visualize the data via the IBM IoT BlueMix cloud. This project adapts and integrates different recipes into this particular mix of features. I will link the addresses of the inspiration content below. For detailed instructions with pictures, visit my blog post here.

The project requires:

## Hardware:
- 3 Pi Zeros with internet (or internet dongles if using Pi Zeros or Pi 3s without internet)
- 3 Pi Zero power adaptors
- 1 Monitor with HDMI output
- Laptop or Desktop
- USB Keyboard
- USB Mouse
- 1 HDMI cable
- USB 2.0 to micorUSB hub
- 3 HDMI to microHDMI adaptors
- Wi-fi USB dongle
- 40 pin Header
- 3 Cases (optional)
- 3 small bread boards
- 3 DHT11 sensors
- 3 LDR sensore
- 3 1 uF capacitors
- Assorted wire
- Solder wire
- Flux
- Soldering station
- Sponge of wire wool
- Painter's tape

## Software:
- 3 microSD cards loaded with the latest Raspbian Jessie OS
- PUTTY or other SSH Client
- Bluemix account
- Node-RED for Raspberry Pi

## Scripts/Circuits:
- http://pastebin.com/raw/qwXLu0hu
- https://pimylifeup.com/raspberry-pi-light-sensor/
- Pin Diagram
- Circuit Diagram
- Flow

let's Get Started!


## HARDWARE
### Solder Header
Solder header onto the top side of the Pi Zero boards.

### Make Cases
Our lab has a laser cutter, so we custom-made cases for our Pi Zeros. You may find the schematics we used for our laser-cutter [here](https://www.thingiverse.com/make:282969). The schematics are bespoke for a non-wireless Pi Zero, but they can still be used for the wireless Pi Zeros; although the heat vent won't line up as nicely. If getting a Pi as part of a kit, like CanaKit, you can use the included case. You may order a Pi through CanaKit [here](www.canakit.com).

### Widen RPi holes
If using the bespoke laser-cut case like we did, I recommend enlarging the corner holes of the Pi Zero board. We used M3 bolts to secure the case, so we widened the holes using incrasingly larger drill bits.

### Assemble
Stack the case components according to the thingiverse instructions. I recommend using tape to hold together the center of the case while screwing in the bolts onto the case.

### Power Up
Plug to power cord.

### Connect to display monitor
Use an HDMI to HDMImini adaptor to connect the display to the Pi Zero. Connect keyboard and mouse to a USB 2.0 hub. Connect the hub to USBmini to the board. If using a Raspberry Pi 3, use a USB wifi dongle to get wireless connectivity.


## SOFTWARE
### Install Raspbian Jessie Image onto SD cards
Get Raspbian Jessie with PIXEL from https://www.raspberrypi.org/downloads/raspbian/. Download onto a microSD card with an adaptor.

### Change keyboard settings
If using PIXEL, click on raspberry icon on top left of GUI. Go to Preferences. Choose Mouse and Keyboard Settings, then follow the prompts for your keyboard and language. If using the command prompt, type `raspi-config`. Choose Internationalization. Choose Keyboard setup.

### Connect to wi-fi
On PIXEL, select the icon with two wires crossed out in the top right. Choose your network.

### Setup SHH
Download Putty for your laptop http://www.putty.org/ or Bitvise https://www.bitvise.com/download-area. Allow SSH on Raspberry Pi by clicking on the raspberry icon on the top left of GUI. Go to Preferences. Select Raspberry Pi Configuration. Click on Preferences tab. select the on option for SSH. Enter the command `hostname -I` onto the command line. That is your IP address. Enter the IP address onto Putty or Bitvise to connect remotely to your raspberry pi. You should be asked for credentials. By default, your username is 'pi' and your password is 'raspberry'. Note, now you don't need a monitor connected to your pi unless you want it. You can completely control the raspberry pi wirelessly!

### Install Node-RED
If you have the full pixel version of Raspbian Jessie running, Node-RED is already pre-installed and you don't need to install it. You may need to update it, however. From your secure shell, use this command to update Node-RED `update-nodejs-and-nodered`.
Follow the instructions [here](https://nodered.org/docs/hardware/raspberrypi) for upgrading Node-RED and npm to version. Once you have the latest npm version, download the node-red dashboard like this:
 ```
 cd ~/.node-red
 npm install node-red-dashboard
 ```
Use this command to stop Node-RED:
```
node-red-stop
```
Use this command to start up Node-RED again:
```
node-red-start
```


## CIRCUIT DIAGRAMS
### Follow circuit

| ![Pin Diagram](https://user-images.githubusercontent.com/9855662/27614444-a720a476-5b56-11e7-857c-1d1993ddfb51.JPG) | ![Circuit Diagram](https://user-images.githubusercontent.com/9855662/27614459-b7607c4e-5b56-11e7-9b54-99815e2fda4d.JPG) |
|:---:|:---:|
| Pin Diagram | Circuit Diagram |

1. From pin 2 on the Pi, connect a wire to the power pin on the DHT11. 
2. From pin & on the Pi, connect a wire to the data pin onthe DHT11. 
3. From the ground pin on the DHT11, connect a wire to the negative (long) end of the capacitor. 
4. Connect the positive (short) end of the capacitor to the negative end of the LDR. 
5. From pin 1 on the Pi, connect a wire to the positive end of the LDR. 
6. From pin 12 on the Pi, connect a wire to the inersect node of the positive end of the capacitor and the negative end of the LDR. This will be your data line. 
7. From pin 9 on the Pi, connect a wire to the ground pin on the DHT11.

## PROGRAMMING
### Download C Script onto RPi
From your home directory (use the command `cd ~` to get to it if you're not there yet), make a new folder called 'DHT11'. Create a file, using your favorite editor, called 'dht11.c' (here is an example using vi editor).
```
mkdir DHT11
cd DHT11
vi dht11.c
```
Then, follow the instructions [here](http://pastebin.com/raw/qwXLu0hu) to write and compile an executable file. Note that often, while pasting contents from your clipboard to vi editor, the paste function will truncate the first two characters from your file. To fix this, hit the 'Esc' key on your keyboard, use your arrow keys to travel up to the begining of your file. Compare the pasted text to the original. If characters are missing, at the start of your file, press the 'insert' key on your keyboard. Type the needed characters and hit the 'Esc' key. Type ":wq!" and hit the 'Enter' key. You are now ready to compile your file. You are now ready to continue the [compiling instructions](http://pastebin.com/raw/qwXLu0hu).

### Download Python Script onto RPi
Similarly, from your home directory, make a folder called 'LDR'. Use the command below to clone the [Python code](https://pimylifeup.com/raspberry-pi-light-sensor/).
```
mkdir LDR
cd LDR
git clone https://github.com/pimylifeup/Light_Sensor/
cd ./Light_Sensor
```
Because this code comes from a different tutorial that only uses the LDR sensor, we have to adjust out pin settings for our purposes. We will open up the new light sensor file and replace one line of code.
```
vi light_sensor.py
```
Hit the 'Esc' key. Using the arrow keys, navigate the cursor to the begining of the line 'GPIO.setmode(GPIO.BOARD)'. 

![ignore warnings](https://user-images.githubusercontent.com/9855662/27616556-6882548a-5b65-11e7-8b62-7a0373e7cf04.PNG)

Hit the 'Insert' key. Type "GPIO.setwarnings(False)" and hit 'Enter'. Hit the 'Esc' key one more time and cursor down to right before the number 7.

![pin7 to 12](https://user-images.githubusercontent.com/9855662/27615777-846708a4-5b5f-11e7-9b61-595777d0ee40.PNG)

Hit the 'Delete' key on the keyboard. Hit the 'Insert' key on the keyboard and type "12". Hit the 'Esc' key and type ":wq!". Now, your file has been modified to use the 12th pin on the Pi instead of the 7th pin.

### Run Node-RED
Type the command, `node-red-start`, to make sure Node-RED is running. Look for the line on the startup message that gives you the IP address to access from your internet browser. It should be something like '123.123.0.123:1880'. Enter the IP address you get into your web browser and you should load something like this. Note the dashboard tab.

### Make flow
1. Click on the three line button on the top right menu.
2. Click the Import tab.
3. Hover over 'Clipboard' and click.
4. Copy the flow from the file 'WSNFlow' on this repository into the box.
5. Click 'Import'.
6. You should see a flow trailing your cursor. Click on the area on the clean screen where you would like to paste it. Click the top right 'Deploy' Button.
7. Test it! Click the light blue button to the left of the 'Light, Temperaure, Humidity' bubble. See your progress on the debug tab on the right.

### Connect to BlueMix

