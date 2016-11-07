WebUSB for Zephyr OS
====================

This folder contains WebUSB-enabled Serial interface for Zephyr OS. Examples and JavaScript code are available in the toplevel demos directory.

This code requires a local copy of Zephyr OS source. I will upstream this module to the Zephyr OS later.

Compatible Hardware
-------------------

This code has been tested with the Arduino 101.

Getting Started
---------------

1. Make sure you are running the [latest dev-channel release of Google Chrome](https://www.google.com/chrome/browser/desktop/index.html?extra=devchannel).

2. Make sure the "Experimental Web Platform Features" flag is enabled in chrome://flags/#enable-experimental-web-platform-features. (Sorry, I can't link to it. You need to copy and paste it manually into the omnibox.)

3. Confirm the [Zephyr SDK] https://www.zephyrproject.org/doc/1.4.0/getting_started/installation_linux.html#zephyr-sdk) has been installed in your system.

4. Clone this Zephyr OS repo:
  
 ` $ git clone https://gerrit.zephyrproject.org/r/zephyr zephyr-project`


5. Copy the `zephyr/subsys/usb/webusb` directory from this repository into the `zephyr-project/subsys/usb/` folder in the zephyr-project directory.

6. Copy the `zephyr/samples/usb/webusb` directory from this repository into the `zephyr-project/samples/usb` folder in the zephyr-project directory.

7. Add WebUSB-enabled Serial driver into the build configuration options. To do this please follow the instructions below.

   Add the following line in 'zephyr-project/subsys/usb/Kconfig'

   `source "subsys/usb/webusb/Kconfig"`

   Add the following line in 'zephyr-project/subsys/usb/Makefile'

   `obj-$(CONFIG_USB_DEVICE_STACK) += webusb/`


8. Set up Zephyr OS environment variables

   `$ cd zephyr-project`

   `$ source zephyr-env.sh`

9. Build the console demo app

   `$ cd samples/usb/webusb/console`

   `$ make BOARD=arduino_101`


10. Flash the image:
To flash the device with dfu-util, first press the Master Reset button on your Arduino 101, and about three seconds later type:

    `dfu-util -a x86_app -D outdir/arduino_101/zephyr.bin`

11. When the flashing is done press the Master Reset button one more time to boot your new image. Once the device is booted, you should see a notification from Chrome: "Go to nagineni.github.io/zephyr-webusb/demos/ to connect.". Click on it and try.


Build Ashell with WebUSB
------------------------

1. Confirm the [Zephyr SDK] https://www.zephyrproject.org/doc/1.4.0/getting_started/installation_linux.html#zephyr-sdk) has been installed in your system.
    
2. Clone this JavaScript* Runtime for Zephyr* OS and make the initial setup
    
    `https://github.com/01org/zephyr.js`

3. Copy the zephyr/subsys/usb/webusb directory from this repository into the deps/zephyr/usb/ folder in the zephyr.js project directory.

4. Add WebUSB-enabled Serial driver into the build configuration options in zephyr.js repo . To do this switch to zephyr.js directory and follow the instructions below.

    `Add the following line in 'deps/zephyr/usb/Kconfig'`

    `source "usb/webusb/Kconfig"`

    `Add the following line in 'deps/zephyr/usb/Makefile'`

    `obj-$(CONFIG_USB_DEVICE_STACK) += webusb/`
    
5. Instead of default CD ACM class driver, use WebUSB-enabled Serial driver in ashell. To do this please follow the instructions below.

   ` vi prj.conf.arduino_101_dev`
    
     `replace CONFIG_USB_CDC_ACM=y with CONFIG_WEBUSB_CDC_ACM=y`
    
    `vi src/ashell/acm-uart.c`
    
    `Replace CONFIG_CDC_ACM_PORT_NAME with CONFIG_WEBUSB_PORT_NAME`

6. Noticed an issue calling __stdout_hook_install() in ashell. Please disable it for now in src/ashell/acm-uart.c.

7. Build the ahell app

    `$ make DEV=ashell MALLOC=heap`

8. Flash the image: To flash the device with dfu-util, first press the Master Reset button on your Arduino 101, and about three seconds later type:

    make dfu

9. When the flashing is done press the Master Reset button one more time to boot your new image. Once the device is booted, you should see a notification from Chrome: "Go to webusb.github.io/arduino/demos/ to connect.". Click on it and try.
