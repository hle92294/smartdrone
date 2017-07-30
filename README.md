## Setup
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



