# OSML-IoT-WNS
Wireless network of temperature, humidity, and light sensors on Raspberry Pi Zeros via Node-RED and BlueMix.

This project is a collaboration between Theo Noordeloos and Joy Lopez Cervera with help from Dan Hendricks at the Open Source Maker Labs.
The purpose of this project is to convert PiZeros into wireless network nodes that sample temperature and humidity via a DHT11 sensor, and samples light via a LDR sensor; feed the information via wireless through a Node-RED flow; visualize the data via the IBM IoT BlueMix cloud. This project adapts and integrates different recipes into this particular mix of features. I will link the addresses of the inspiration content below. For detailed instructions with pictures, visit my blog post here.

| ![img_2986](https://user-images.githubusercontent.com/9855662/27617527-c1665ee2-5b6b-11e7-8389-21d448db42c1.JPG) | ![flow](https://user-images.githubusercontent.com/9855662/27617614-59f90e5c-5b6c-11e7-9875-db418fb1d27b.PNG) | ![bluemix](https://user-images.githubusercontent.com/9855662/27617466-51bb4102-5b6b-11e7-85f8-b602979f30d9.PNG) |
|:---:|:---:|:---:|
|Pi units|Node-RED Flow| Bluemix dashboard |

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

![img_3005](https://user-images.githubusercontent.com/9855662/27617415-03ac0a46-5b6b-11e7-91ac-7a8b1bfbd80c.JPG)


### Widen RPi holes
If using the bespoke laser-cut case like we did, I recommend enlarging the corner holes of the Pi Zero board. We used M3 bolts to secure the case, so we widened the holes using incrasingly larger drill bits.

### Assemble
Stack the case components according to the thingiverse instructions. I recommend using tape to hold together the center of the case while screwing in the bolts onto the case.

### Power Up
Plug to power cord.

### Connect to display monitor
Use an HDMI to HDMImini adaptor to connect the display to the Pi Zero. Connect keyboard and mouse to a USB 2.0 hub. Connect the hub to USBmini to the board. If using a Raspberry Pi 3, use a USB wifi dongle to get wireless connectivity.

![img_2956](https://user-images.githubusercontent.com/9855662/27617395-e6033d34-5b6a-11e7-957e-18fbd086e8e4.JPG)


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
Similarly, from your home directory, make a folder and a file in it.
```
mkdir LDR
cd LDR
vi light_sensor.py
```
Paste this code onto your file
```python
import RPi.GPIO as GPIO
import time

__author__ = 'Gus (Adapted from Adafruit)'
__license__ = "GPL"
__maintainer__ = "pimylifeup.com"


GPIO.setwarnings(False)

GPIO.setmode(GPIO.BOARD)

#define the pin that goes to the circuit
pin_to_circuit = 12

def rc_time (pin_to_circuit):
    count = 0

    #Output on the pin for
    GPIO.setup(pin_to_circuit, GPIO.OUT)
    GPIO.output(pin_to_circuit, GPIO.LOW)
    time.sleep(0.1)

    #Change the pin back to input
    GPIO.setup(pin_to_circuit, GPIO.IN)

    #Count until the pin goes high
    while (GPIO.input(pin_to_circuit) == GPIO.LOW):
        count += 1

    return count

#Catch when script is interupted, cleanup correctly
try:
    # Main loop
print rc_time(pin_to_circuit)
except KeyboardInterrupt:
    pass
finally:
    GPIO.cleanup()
```
Again, while pasting contents from your clipboard to vi editor, the paste function will truncate the first two characters from your file. To fix this, hit the 'Esc' key on your keyboard, use your arrow keys to travel up to the begining of your file. Compare the pasted text to the original. If characters are missing, at the start of your file, press the 'insert' key on your keyboard. Type the needed characters and hit the 'Esc' key. Type ":wq!" and hit the 'Enter' key. Now, your file is ready to test! On your command line, type:
```
python light_sensor.py
```
If you get a number, it works!

### Run Node-RED
Type the command, `node-red-start`, to make sure Node-RED is running. Look for the line on the startup message that gives you the IP address to access from your internet browser. It should be something like '123.123.0.123:1880'. Enter the IP address you get into your web browser and you should load something like this. Note the dashboard tab.

### Make flow
1. Click on the three line button on the top right menu.
2. Click the Import tab.
3. Hover over 'Clipboard' and click.
4. Copy this text into the box:
> [{"id":"f436c50d.3367","type":"inject","z":"9649156.b4a2f68","name":"Temperature, Pressure, Light","topic":"","payload":"","payloadType":"date","repeat":"10","crontab":"","once":false,"x":179,"y":210,"wires":[["fd6f5e3e.c677c8","81429d4e.bb2f9"]]},{"id":"fd6f5e3e.c677c8","type":"exec","z":"9649156.b4a2f68","command":"./DHT11/dht11","addpay":true,"append":"","useSpawn":"","timer":"","name":"READ DHT SENSOR","x":448,"y":126,"wires":[["64046801.849fd"],[],[]]},{"id":"81429d4e.bb2f9","type":"exec","z":"9649156.b4a2f68","command":"python ~/LDR/Light_sensor-master/light_sensor.py","addpay":true,"append":"","useSpawn":false,"timer":"","name":"READ LDR SENSOR","x":283,"y":351,"wires":[["22764180.bcccf6"],[],[]]},{"id":"64046801.849fd","type":"function","z":"9649156.b4a2f68","name":"","func":"if(msg.payload.length === 0)\n	{\n	var msgExit = {payload: \"No input\"};\n	return msgExit;\n	}\nvar Data = msg.payload.split(\",\");\nvar Humidity = parseFloat(Data[0]);\nvar Temperature = parseFloat(Data[1])*9/5 + 32;\nvar msg ={temp:Temperature,humid:Humidity};\nvar jimmy ={payload:msg};\nreturn jimmy;","outputs":1,"noerr":0,"x":690,"y":190,"wires":[["894c9eea.deb318","f1b5333d.472248"]]},{"id":"894c9eea.deb318","type":"debug","z":"9649156.b4a2f68","name":"","active":true,"console":"false","complete":"payload","x":867,"y":141,"wires":[]},{"id":"22764180.bcccf6","type":"range","z":"9649156.b4a2f68","minin":"0","maxin":"30000","minout":"100","maxout":"0","action":"scale","round":true,"name":"","x":503,"y":343,"wires":[["3bea7104.66c596"]]},{"id":"1cc234e6.e49c63","type":"debug","z":"9649156.b4a2f68","name":"","active":true,"console":"false","complete":"payload","x":879,"y":413,"wires":[]},{"id":"3bea7104.66c596","type":"function","z":"9649156.b4a2f68","name":"","func":"var measurement=msg.payload;\nvar tony={brightness:measurement};\nvar newmsg = {payload:tony};\nreturn newmsg;","outputs":1,"noerr":0,"x":683,"y":340,"wires":[["1cc234e6.e49c63","f1b5333d.472248"]]},{"id":"f1b5333d.472248","type":"wiotp out","z":"9649156.b4a2f68","authType":"d","qs":"false","qsDeviceId":"","deviceKey":"ee3313fb.de1f1","deviceType":"","deviceId":"","event":"Pi","format":"json","name":"","x":990,"y":257,"wires":[]},{"id":"ee3313fb.de1f1","type":"wiotp-credentials","z":"","name":"Watson IoT","org":"6suqu4","devType":"Pi","devId":"1"}]
5. Click 'Import'.
6. You should see a flow trailing your cursor. Click on the area on the clean screen where you would like to paste it. Click the top right 'Deploy' Button.
7. Test it! Click the light blue button to the left of the 'Light, Temperaure, Humidity' bubble. See your progress on the debug tab on the right.


## Repeat!
Now that you know how to setup one Pi, you can repeat this process as many times as necesary for you to incorporate more nodes into your wireless sensor network.

### Connect to BlueMix
1. Create a account with IBM bluemix.
2. Launch an IoT platform service.

![watson plateform](https://user-images.githubusercontent.com/29682535/27618319-62598e9c-5b70-11e7-88c2-d33f6226b8f3.PNG)

3. Follow this recipe to create and register a device (Pi) with Bluemix. Be sure to copy the device credentials.  https://developer.ibm.com/recipes/tutorials/how-to-register-devices-in-ibm-iot-foundation/
6. In Node-RED, double click the 'Watson IoT' node. An 'Edit' window will appear.
7. Select the 'Registered' circle, then click the 'pencil' icon. 
8. Fill in the device credentials, then click 'update' and 'done'.
9. Click 'Deploy' and the 'Watson IoT' node will display 'connected'.
10. Now that the Pi is connected to Bluemix, follow this recipe to create Boards and cards that display real-time data. https://developer.ibm.com/recipes/tutorials/configuring-the-cards-in-the-new-watson-iot-dashboard/
11. Repeat for remaining two Pis

