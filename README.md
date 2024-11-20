# Blueborne Attack Setup
This is a guide to execute a blueborne attack on a vulnerable linux computer. 
ALL OF THE CODE IN THIS REPO IS TAKEN FROM https://github.com/ArmisSecurity/blueborne

## Dependencies for Attack
You need python2 installed, this may be a bit more difficult on newer linux distributions. You may need to build it yourself.
Once python2 is installed, you need pybluez, pwn, and scapy
```bash
sudo pip2 install pybluez pwn scapy
```
You also need libbluetooth-dev
```bash
sudo apt-get install libbluetooth-dev
```

## Setting up vulnerable system
To setup a vulnerable system you will likely need a system older than 2017. You need to have a bluetooth/wifi card that still has drivers in the kernel before Sept 2017, otherwise you will need to manually choose commits to make it vulnerable. 
For my system I decided to use Fedora 25, and just use a live image from a USB flash drive to attack. You can get a download from: [Fedora 25](https://www.google.com/url?q=https://archives.fedoraproject.org/pub/archive/fedora/linux/releases/25/Workstation/x86_64/iso/Fedora-Workstation-Live-x86_64-25-1.3.iso)

You will need to boot into this image. It's easies to just boot into a live image on a flash drive. Once booted, you need to enable you bluetooth. Run
```bash
sudo bluetoothctl
```
This should bring up a shell. Run 
```bash
power on
```
and this should enable your bluetooth.
You will need to get your bluetooth address so you can attack it from the attacking machine. To do this run
```bash
hciconfig
```
You should see an output that looks like 
```
hci0:	Type: Primary  Bus: USB
	BD Address: 2C:8D:B1:E3:87:6B  ACL MTU: 1021:4  SCO MTU: 96:6
	UP RUNNING PSCAN 
	RX bytes:2323268 acl:162 sco:0 events:331106 errors:0
	TX bytes:227216015 acl:330932 sco:0 commands:142 errors:0
```
The 2 important values are that your interface is `hci0` and your BD Address is `2C:8D:B1:E3:87:6B` (These values will differ for your machine).

## Executing the attack
On you attack computer enter your root shell
```bash
sudo su
```
then source the env.sh script. This makes sure the python scripts can see each other in the path.
```bash
source env.sh
```
Now you should be able to run the attack. Use the \<BD Address\> and \<interface\> from the "Setting up vulnerable system" section
```bash
python2 linux-bluez/amazon_echo/exploit.py <interface> <BD address>
```
And that should execute the attack. :)
