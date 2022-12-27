# ZeroPointModule
 A Raspberry Pi Zero 2W Carrier for 3D-printing and autonomous driving
 
The ZeroPointModule is a carrier board for the Raspberry Pi Zero 2W with some application specific expansion making it suitable for applications such as 3D-printing and prototyping of perception algorithms.

First revision came with options to mount the CAN-Bus related components either as SMD or THT as well as with some nice hardware bugs.
Revision two has those possibility removed as well as the hardware bugs fixes, additionally some more features were added.

!(Early prototypes of the ZPM)[DOC/P1010022.JPG]

## Features
- 5V 5A DCDC converter powering the Pi and added peripherals
- MCP2515 CAN-Bus Controller and corresponding transceiver
- Two hardware PWM channels with low-side mosfet switches
- SPI, Uart and I2C connections broken out to dedicated connectors
- Two connectors for 4-pin PWM Fans (5V/24V selectable)
- low profile form-factor possible
- eeprom based autoconfiguration of IOs and kernel modules


## Assembly options
To deal with specific requirements and availability of parts, several choices for different key components are available

### 1 - main filter caps
Choose between SMD and THT capacitors based on preference and availability

### 2 - DCDC Converter IC
Choose between Texas Instruments TPS54531 and TPS54331, based on availability or requriments. *531 can handle up to 5A continouus, *331 can handle up to 3A.

### 3 - Low or high profile assembly

Choose between the low-profile assembly based on the Samtec HLE-Series Through hole socket combined with Würth Electronic WA-SMSI SMD spacers and a high profile variant with a generic 40-pin socket and 11mm standoffs.

High Profile
![High Profile Variant](/DOC/PCB-HighV2_2022-Dec-26_01-16-51PM-000_CustomizedView3909409397.png)

Low Profile
![Low Profile Variant](/DOC/PCB-Low_2022-Dec-26_01-30-59PM-000_CustomizedView15816249176.png)

### 4 - CAN-Bus Transceiver
Choose between different Canbus transceivers based on availability - options: MCP2562T or TJA1050

### 5 - Noise suppression
Based on availability, the ecmf common mode choke and esd suppressor chip can be shorted with two 0R resistors if said chip is not at hand.

### 6 - EEPROM Autoconfiguration
This board supports eeprom based gpio autoconfiguration, if this is not required, a subset of components can be left unpopolated to save cost

# Ressources

DFM Folder contains all files necessary for assembly (Gerber Files, BOM and SMT placement). SMD assembly can be done by JLCPCB when using the corresponding BOM and CPL files.

SRC Folder contains schematic and board design files as exported from Fusion360 as well as stepfiles for the low- and the high-variant.
For easier viewing the schematic is also included as PDF file.

DOC Folder contains some more images than what has been shown here.

# Kernel configuration

To use the hardware pwm feature of the BCM SoC connected to the two mosfet stages, add this line to your /boot/config.txt
> dtoverlay=pwm-2chan,pin=12,func=4,pin2=13,func2=4

Activating the CAN-Bus Interfaces requires two steps, first add these line to your /boot/config.txt

dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=8000000,interrupt=25 

After that create the file '/etc/network/interfaces.d/can0' with following contents
...
auto can0
iface can0 can static
    bitrate 500000
    up ifconfig $IFACE txqueuelen 128
...

# Upcoming

Although this project was initially planned to be utilized in 3D-printers other applications arose. For usage of the ZeroPointModule in applications concerning autonomous driving and corresponding development with [RTMaps from Intempora](https://intempora.com/products/rtmaps/) a blockset for convenient gpio access has been developed as well as some example diagrams. These components are currently in an alpha phase and will be released subsequently.


# Special thanks

Many thanks to my betatesters, [meteyou](https://github.com/meteyou), linky007 and florian
This project is (partially) sponsored by [JLCPCB](https://jlcpcb.com/).



