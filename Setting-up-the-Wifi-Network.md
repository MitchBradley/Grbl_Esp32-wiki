## Setting up the Wifi Network

### Default Settings

The first time you power up the ESP32 with Grbl_ESP32 it will a create a Wifi access point with the following settings

- **SSID:** GRBL_ESP
- **Password:** 12345678
- **IP Address**: 192.168.0.1

You can connect any Wifi enabled device to this network. Most computers will automatically launch a browser to connect to Grbl_ESP32 after connecting via Wifi. If it does not, simply enter **http://192.168.0.1** for the address in your browser.

This is very convenient if you are just getting started or there is no existing network to connect to.

### Determining Current Settings

If you want to determine your current settings, you can connect a USB cable. Open the created serial port at 115200 baud in a serial terminal. Send **$I** with a carriage return to get the information that includes the network information. It will not tell you any passwords. If you loose the password, you will need to reset it or reload the firmware.

**Note:** By default authentication is off in firmware. If you have it enabled a second level of authentication is required to access the machine. The default user is "admin" with password "admin". This is control via **#define ENABLE_AUTHENTICATION** in config.h

### Changing Wifi Setting

If you want to change the name of the Wifi or connect it to an existing Wifi, there are two methods.

**Note:** If you change the settings to connect to another network and it fails to connect, it will revert to making its own network.

#### Method #1: Using the WebUI to change the network

The easiest way to change the settings is via the WebUI (Web User Interface). Go to the ESP3D tab of the WebUI. Here you will find all the network settings. We will only cover a few here. Be sure to click set after changing each one. **Note: Nothing take effect until reboot.**

- **AP SSID:** This is the name of the network (Access Point) Grbl_ESP32 creates
- **AP Password:** The password for the access point
- **AP: Static IP:** This is the adress of the access point
- **Station SSID:** This is where you would put your network name
- **Station Password:** This is the password for your network
- **Station IP Mode:** Most people would leave this at DHCP. If you want a specific one select static and set the other static settings
- **Radio Mode:** This determines if you want Access Point, Client Station (your network), Bluetooth or none. **This is important. don't forget to set this one**

When everything is set, click the red power icon on the WebUI. During the startup process, helpful information is sent to the serial port. It will tell you if the connection to the new network was successful.



## Method #2: Using a serial terminal

You can use the [ESPxxx] protocol to change the network settings. This is handy if you cannot connect and open the WebUI. [All commands are documented here](https://github.com/bdring/Grbl_Esp32/blob/master/doc/Commands.txt).

Note: If you have authentication enabled, you will need to supply a password. These examples assume it is off.

Here are the steps required to put it on your nework

- [ESP100]YourSSID
- [ESP101]YourPassword
- [ESP115]STA

Now reboot the system.



