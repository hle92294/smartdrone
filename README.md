## Ar-drone Setup
Connect the Drone to my network:
``` 
 1. Git clone https://github.com/daraosn/ardrone-wpa2
 2. cd to ardrone-wpa2
 3. Connect to the drone network	
 4. $ script/install
 5. $ script/connect "NETWORK_ID" -p "NETWORK_PASSWORD" -a 192.168.1.50 -d 192.168.1.1

To verify, after 10s, connect to your network, then ping the ip 192.168.1.50 or telnet 
```

Install OpenCV
```
 0. Make sure node and npm already installed
 1. Download & install Xcode 
 2. Download & Install Opencv from the website
 3. npm install Opencv --build-from-source
```

# AlexaPi Setup 

## Format your SD card: 

```
https://www.sdcard.org/downloads/formatter_4/
```

Download the latest Raspbian image
NOTE: The current version may not work great with Alexa, try older one
```
https://www.raspberrypi.org/downloads/
```

## Configure Pi
Login with default id/password:

```
username:pi, password:raspberry
```
To change password:

```
$ passwd
```

Update/Upgrade:
```
$ sudo apt-get update
$ sudo apt-get upgrade
```

Disable Bluetooth:
```
$ sudo nano /boot/config.txt
```
Add this line at the end of the file
```
“dtoverlay=pi3-disable-bt” 
```

Enable SSH
```
$ sudo raspi-config
	Interface -> SSH -> Enable -> Yes -> Finish
```

Connect to your network with command
```
$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
	NOTE: use (SHIFT + @) to get double quote ("") 
	Add this line at the bottom of the file:
		network={
				ssid="NETWORK_ID"
				psk="NETWORK_PASSWORD"
		}
```

Register Amazon Alexa 
```
First you need to obtain a set of credentials from Amazon to use the Alexa Voice service. Make a note of these credentials as you will be asked for them during the install process.

Login at https://developer.amazon.com and go to ALEXA, then Alexa Voice Service.
Register a Product Type > Device.
For the Device Type ID and Display Name use something like AlexaPi or whatever you want.
You are at Security Profile left tab.
From the drop-down menu choose Create a new profile.
Choose whatever for Security Profile Name and Security Profile Description. Hit Next.
Under Web Settings horizontal tab hit Edit and:
Allowed Origins - put there https://localhost:3000 and http://ALEXA.DEVICE.IP.ADDRESS:3000
Allowed Return URLs put https://localhost:3000/authresponse and http://ALEXA.DEVICE.IP.ADDRESS:3000/authresponse. You have to replace ALEXA.DEVICE.IP.ADDRESS with the IP (for example 192.168.1.123) of your AlexaPi device (for example Raspberry Pi). This is especially necessary when you are installing from another computer than AlexaPi is gonna run on.
```

Clone the sample app
```
cd Desktop
$ sudo git clone https://github.com/alexa/alexa-avs-sample-app
```

Configure the setup script.
Before you run the install script, you need to update the script with the credentials - ProductID, ClientID, ClientSecret
```
cd Desktop/alexa-avs-sample-app
nano automated_install.sh

Example: 
	ProductID=raspberry2
	ClientID=amzn1.appl
	ClientSecret=2b0a
```
Save & Exit

Run the setup script
Note: this step take about 30 minutes
```
cd ~/Desktop/alexa-avs-sample-app
. automated_install.sh
```

Create a start script
```
nano startalexa.sh
```

Copy the code below into startalexa.sh
```
#!/bin/bash

#Start companion service
cd /home/pi/Desktop/alexa-avs-sample-app/samples
cd companionService && npm start&

#Run the sample app
echo "Starting sample app."
cd /home/pi/Desktop/alexa-avs-sample-app/samples
cd javaclient && mvn exec:exec&
echo "When finished "
read -n1 -r -p "Press space to continue..." key

#Run the Wake Word Engine
cd /home/pi/Desktop/alexa-avs-sample-app/samples
cd wakeWordAgent/src && ./wakeWordAgent -e kitt_ai &

```

Run the start alexa script 

```
. startalexa.sh
```
This will show you a webbrowser and sign in with your developer account, you will get some thing said "token is ready"
, go back to your terminal and hit the space bar to start up the wake word






