# HassanChadad

# CS 683 Computer Security

# Windows Unlocker Pi Zero

## Description

This project aims to unlock windows machine running windows 10 specifically and to crack the hash of the machine's password. I will use Raspberry Pi Zero that will act as a ducky usb and will run payloads when connected to the target machine.

## Setup

To setup your raspberry pi to be act as a ducky USB, please follow the below instructions

1. Get a [Raspberry Pi zero](https://www.amazon.com/Vilros-Raspberry-Starter-Power-Premium/dp/B0748MPQT4/ref=sr_1_3?keywords=pi+zero&qid=1557187209&s=electronics&sr=1-3)
2. Get a [micro SD card](https://www.amazon.com/Kingston-Digital-8GB-Industrial-SDCIT/dp/B01DOFCPNW/ref=sr_1_1?keywords=micro+card+sd+8gb&qid=1557187314&s=electronics&sr=1-1) - 8 GB would be more than enough
3. Download [Raspbian Stretch Lite](https://www.raspberrypi.org/downloads/raspbian/)
4. Download and Install [Etcher](https://www.balena.io/etcher/) on your machine, then use it to write the Raspbian Stretch Lite on the SD card

## Connect to Pi Zero

#### Username: pi

#### Password: raspberry

There multiple ways to connect to Pi Zero, please follow any method you want:

#### Connect using keyboard and monitor

You can connect the raspberry pi to a monitor using the HDMI port and with a [wireless keyboard](https://www.amazon.com/STEADYGAMER-Wireless-Keyboard-Touchpad-Raspberry/dp/B07MP9BGWP/ref=sr_1_8?keywords=pi+zero+keyboard&qid=1557188474&s=electronics&sr=1-8)

#### SSH/Connect to Pi zero Using Ethernet connection

1. Get a [USB-Ethernet Adapter](https://www.amazon.com/AmazonBasics-USB-Ethernet-Network-Adapter/dp/B00M77HLII/ref=sxin_2?crid=1Y7ZSRN48T405&keywords=usb+to+ethernet&pd_rd_i=B00M77HLII&pd_rd_r=14938a6e-71e6-4b86-a563-b982db669a66&pd_rd_w=UkTgN&pd_rd_wg=2610h&pf_rd_p=2f202bc2-a141-4742-bd82-9252d5485b34&pf_rd_r=Z8ME8RPB9SCEKQKH02NR&qid=1557187664&s=electronics&sprefix=USB-Ethere%2Celectronics%2C197)
2. Enable your internet network to share with Ethernet
3. Download and install [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
4. Insert the SD card to the laptop
5. Open the Software folder and create a file named "ssh" without any extension, so if you created a .txt fil rename the file to ssh and remove ".txt" **This will enable SSH to the pi**
6. Put the SD card in the raspberry Pi and connect it to the machine using the Ethernet connection
7. Open Command Prompt/Terminal: <br/>
   Run `ping -4 raspberrypi.local` to get the IPv4 of the raspberry Pi
8. Open Putty - Select SSH - Enter the IPv4 of the pi - Open

#### Connect/SSH Using Pi 3 to access Pi zero's software

If you have a pi 3, you can use it to ssh to the software of the pi zero on the SD card. Just plug the Pi zero's SD card in Pi 3 and follow the above method **SSH/Connect to Pi zero using Ethernet connection** by skipping the first step.

#### Connect to Pi zero Using USB (Windows Users only)

You can follow this great guy explaining how to connect to pi zero in his [video](https://www.youtube.com/watch?v=xj3MPmJhAPU)

## Change Pi's Password

Change the Pi's password to avoid someone SSH to your Pi with the default password

1. `sudo raspi-config`
2. Select "Change User Password"

OR

1. Run `passwd`
2. Type current password
3. Type new password
4. Retype new password

## Setup Wifi

After connecting to the pi zero, you will first enable the wireless connection. [I think this is a good tutorial that shows both ways to setup the wifi.](https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html)

## Install P4wnP1

[P4wnP1](https://github.com/mame82/P4wnP1) is a highly customizable USB attack platform, based on a low cost Raspberry Pi Zero or Raspberry Pi Zero W.

1. Install Github <br/>
   `sudo apt-get -y install git`
2. clone P4wnP1 <br/>
   `git clone --recursive https://github.com/mame82/P4wnP1`<br/>
   `cd P4wnP1`<br/>
   `./install.sh`<br/>
   This process would take some time. If you any error occured, you will get an error message.<br/>
   **You can check the Install instruction on P4wnP1 by clicking the [INSTALL.md](https://github.com/mame82/P4wnP1/blob/master/INSTALL.md)**
3. If everything went well. Shutdown Pi zero<br/>
   `sudo shutdown -h now`

## Run P4wnP1

### IMPORTANT NOTE: if you used Pi 3 to install P4wnP1, you should not use Pi 3 now. When the software boots with P4wnP1, it should be on Pi zero. So you can't use Pi 3 after installing P4wnP1.

1. If you used Pi 3, remove SD card and put it in Pi zero
2. Connect Pi zero to the machine using the Data or Power port.
3. Wait for at least 30 seconds for the pi to boot
4. You will see a new Wifi network "P4wnP1". Which means everything is fine till now
5. Connect to "P4wnP1" wifi
6. Open Putty and SSH to pi@172.16.0.1 or to pi@172.24.0.1 if the previous Ip didn't work

## Add and Edit Payloads

Create a new payload

1. `cd P4wnP1`
2. `cd payloads` - you will find all the payloads
3. duplicate a payload that is more relevant to your payload<br/>
   `cp backdoor.txt my_new_backdoor.txt`
4. open the new duplicated file `nano my_new_backdoor.txt`
5. Search for the function that contains Ducky script code and delete them to add your own ducky script

Enable the payload in setup.cfg

1. `cd P4wnP1`
2. `nano setup.cfg`
3. Go to the end of the file
4. Comment the uncommented payload to disable them
5. add on the last line<br/>
   `PAYLOAD=my_new_backdoor.txt`

## Use Pi Zero for Attacks

You need to plug the USB cable to the **DATA** port in your pi to start any attack to allow the pi to transfer data.

## Access Pi's software

Use the **Power** USB Port to disable running attacks from your pi when connecting it to the machine.

## Run Windows 10 Unlocker

For my project, I am using the "Win10_LockPicker.txt" payload to unlock the windows 10 machines.<br/>
This payload grabs the hash of the password and cracks it to get the actual password then types it in the password field and unlocks the machine.<br/>
I would just enable the "Win10_LockPicker.txt" in the "P4wnP1/setup.cfg" and the pi zero will be ready for the attack

### Pi couldn't unlock the machine

The payload adds all the collected hashes and the cracked hashes to a folder "collected"<br/>
If the payload couldn't unlock the machine, then:

1. cd "P4wnP1/collected"
2. the files will show the machine's name, date, and if cracked
3. Choose one of the file and open it
4. Copy the hash
5. Use https://www.openwall.com/john/ to crack the password hash

## Getting The Password Hash of an Unlocked Windows Machine

Another payload is collecting the hash of the unlocked Windows Machine and send it to an external server running on another machine.
You can find the tutorial to do that [here](https://shop.hak5.org/blogs/news/15-second-password-hack-mr-robot-style)

## Challenges/Problems

You would face some challenges when using payloads. If the payloads are not working.

1. Run `journalctl -u P4wnP1.service` to see the log of your Pi. If you found
   in the log "....Waiting for HID keyboard to be usable..." it means the pi is unable to use the keyboard
2. Change PID and VID of Keyboard in your payload
3. Check if the USB cable you are using with your Pi is a data cable, because it might be a power cable that doesn't transfer data

## Limitations

Both payloads are limited in their functionality

#### Windows 10 Unlocker

Cracks easy passwords because it would take a lot of time to crack strong or medium passwords

#### Getting The Password Hash of an Unlocked Windows Machine

Any anti-virus can detect this attack and disable it, so it would be better to use it on previous Windows OS versions like (Windows XP, vista, 7) where the anti-virus is not installed builtin like Windows Defender in Windows 8 and 10.

## Other kinds of Pi zero ducky attacks
[Backdoor attack by Brian Sung](https://github.com/ohbriansung/usb_rubber_ducky/tree/master/pi_zero_ducky)

## Disclaimer

This project is only for acedemic purposes and not intended for offensive and harmful attacks. It is developed as a project for one of the courses
