## Compiling Firmware

The code should be compiled using the latest Arduino IDE and esp32 core. **Seriously...the latest!** [Follow instructions](https://github.com/espressif/arduino-esp32) here on how to setup ESP32 in the IDE. The choice was made to use the Arduino IDE over the ESP-IDF to make the code a little more accessible to novices trying to compile the code. Be sure to use the latest Arduino IDE and ESP files.

Bluetooth and all wifi options are now compiled into one large firmware. You need to select the **Minimal SPIFFS (Large APPS and OTA)** partition scheme under the Tools...Partition Scheme... menus or the firmware and WebUI will not fit.

Here are the settings you should use.

![IDE Settings](http://www.buildlog.net/blog/wp-content/uploads/2019/04/esp32_settings.png)

### Copy Libraries

There are some libraries that must be added to the Arduino IDE. Copy the folders in the [Libraries](https://github.com/bdring/Grbl_Esp32/tree/master/libraries) folder to the libraries folder in your Arduino folder on your computer. Typically some folder like "...\Documents\Arduino\libraries" (Windows computer).



### Customize

The default firmware is setup for use with the [Grbl_ESP32 Development Board](https://www.tindie.com/products/33366583/grbl_esp32-cnc-development-board-v31/). If you have a different hardware target, make sure the I/O pin mapping is correct. See [this wiki page](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-the-I-O-Pins).

### Startup Info

The USB serial port is established right away and will display a lot of useful information. It will also ride through a crash and display useful debugging information. If you want to know the Wifi settings being used, watch this port on startup.

### WebUI

When you first try to use the WebUI, you will get this screen.

***

<img src="http://www.buildlog.net/blog/wp-content/uploads/2018/11/load_webui.jpg" width="600">

***
The file, "index.html.gz". It is in [this folder](https://github.com/bdring/Grbl_Esp32/tree/master/Grbl_Esp32/data).

After loading the file, refresh your browser. If you reload firmware, check to see if "index.htnl.gz" has changed. If you need to upload a new version of "index.html.gz", use the green folder icon on the ESP32 tab of the WebUI.

## First Run

You will get an **error 7** on the first run of the code, if you happen to have a serial terminal open you will see it. It is normal behavior. It is trying to load saved settings, which don't exist yet, so it loads defaults. You should not see the error again.

## Programming Errors

It is common on some dev board to get "Connecting........_____....._____....._____..." The Arduino IDE is having trouble putting the ESP32 in bootloader mode. Try holding down the boot button until it gets past the "Connecting..." phase.

You may also see flash errors. I have found that some dev boards have trouble being programmed while prugged in. Try removing the dev board from the shield while programming. 
