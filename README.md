# IoT-Lab
Iot-Lab for CrowPi

# TODO:
Create installer file to make this magic happen
Add blockly to Jupyter
Add option for headless mode configuration on Jupyter

You can download the SD cardimage here and and copy it onto a 32GB SD card to getyou up and running with the following

 - Rasbian (Buster) base OS
 - Adafruit drivers for Raspberry Pi with CrowPi hard ware
 - Jupyter Notebook installedas a local sever running Python 3
 - Scrath 2
 - Scratch 3
 - Minecraft-pi
 - Idel3 (Python IDE)
 - Lab sheets are pre-installed

Note: All of the hardware configuration has been enabled for the GPIO devices.


Alternatively, you can follow the install list below to create your own image. Just swap out the parts you don't need or add to it.
 

Download Raspbian Image from # # #https://www.raspberrypi.org/downloads/raspbian/

#Install image to SD card as per https://www.raspberrypi.org/documentation/installation/installing-images/README.md

#Insert SD into RPi board, power on, follow on screen prompts.
#Suggest 'iotsoc'

#System should prompt for update and upgrade. If this fails then run the following:
sudo apt-get update && sudo apt-get upgrade

#Restart the system

#Installyour favourite text editor
sudo apt install vim

#Follow latest instructions as per https://github.com/Elecrow-RD/CrowPi
sudo apt-get install build-essential python-dev python3-dev python-smbus python3-smbus python-pip python3-pip python-pil python3-pil liblircclient-dev

#Install Jupyter and text editor
sudo pip3 install jupyter

#Make sure you have the RPi.GPIO library by executing:
sudo pip install RPi.GPIO
sudo pip3 install RPi.GPIO

#This can be done at the end if you get errors
#Need to enable hardware as per
https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c
sudo apt-get install -y python-smbus
sudo apt-get install -y i2c-tools

#Configure the hardware
sudo raspi-config

#Go to Interfacing Options -> A7 I2C -> Enable -> Finish
sudo reboot

#Likewise for SPI
#As per https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-spi

sudo raspi-config
#Go to Interfacing Options -> P4 SPI -> Enable -> Finish
sudo reboot

#Test the following
sudo i2cdetect -y 1
ls -l /dev/spidev*

#Driver Installation. You may need your git user name and password ready
cd ~
git clone https://github.com/Elecrow-RD/CrowPi.git
cd CrowPi/Drivers

cd Adafruit_Python_CharLCD
sudo python setup.py install
sudo python3 setup.py install
cd ..

cd Adafruit_Python_DHT
sudo python setup.py install
sudo python3 setup.py install
cd ..

cd Adafruit_Python_LED_Backpack
sudo python setup.py install
sudo python3 setup.py install
cd ..

cd luma.led_matrix
sudo python setup.py install
sudo python3 setup.py install
cd ..

cd RFID/SPI-Py
sudo python setup.py install
sudo python3 setup.py install
cd ..

#Setupthe IR receiver
cd /home/pi/CrowPi
sudo apt-get install lirc

#The second command produces errrors as of 17/01/2020
sudo pip install python-lirc
sudo pip3 install python-lirc

sudo cp Drivers/LIRC/* /etc/lirc

sudo vim /boot/config.txt

#Go to the following line [This may be different, see alternative]
#Uncomment this to enable the lirc-rpi module
#dtoverlay=lirc-rpi

##Alternative
#dtoverlay=gpio-ir,gpio_pin=17
#dtoverlay=gpio-ir-tx,gpio_pin=18

#Change to
#Uncomment this to enable the lirc-rpi module
dtoverlay=gpio-ir,gpio_pin=20

#Copy the config files
sudo cp /etc/lirc/lirc_options.conf.dist /etc/lirc/lirc_options.conf
sudo cp /etc/lirc/lircd.conf.dist /etc/lirc/lircd.conf

#Edit the config files
sudo vim /etc/lirc/lirc_options.conf

driver = default
device = /dev/lirc0

#Reboot the system
sudo reboot

#Run the installer to fix the previous lirc error
sudo apt-get install lirc

#Stop the lirc
sudo /etc/init.d/lircd stop

# There are also a couple of libraries that need to be installed
sudo pip3 install matplotlib
sudo pip3 install numpy
sudo pip3 install pandas

# If you want Minecraft
sudo apt install minecraft-pi

# Python IDE
sudo apt install idle3



