# Managing and Upgrading the ID-EEPROM

The ZeroPointModule can be equipped with a I2C EEPROM to perfomr automatic loading of device tree overlay files during boot to autoconfigure certain hardware peripherals.

Content for this eeprom can be prepared using the software tools released by the Raspberry Pi foundation and flashed using any available raspberry pi.

## Overview

In general this is a multi-step approach requiring at least some core knowledge about the linux device tree system

- Prepare dts and eeprom_settings.txt files
- compile dts file into dtbo format
- generate eeprom image using raspberry pi's eeprom util
- flash eeprom

## Prerequisites

Clone the Raspberry Pi Hats Repo and build the eepromutils accoring to their documentation:
https://github.com/raspberrypi/hats

Clone this repo 

## Building and updating the eeprom

1. move your .dts and eeprom_settings.txt over to the ./hats/eepromutils directory
2. change to the eepromtutils dir
    ```
    cd ./hat/eepromutils
    ```
3. build the devicetree overlay 
   ```
   dtc -I dts -O dtb -o zpm_overlay.dtbo zpm_overlay.dts
   ```
4. build the eep image from devicetree and configuration 
    ```
    eepmake eeprom_settings.txt zpm.eep zpm_overlay.dtbo
    ```
5. ensure that the id_i2c driver is loaded 
    ```
    sudo dtoverlay i2c-gpio i2c_gpio_sda=0 i2c_gpio_scl=1 bus=9
    ```
6. short the write-protect jumper on the zpm (add picture)
7. flash the eeprom 
    ```
    sudo ./eepflash.sh -w -t=24c32 -f=zpm.eep
    ```
8. reboot the system
