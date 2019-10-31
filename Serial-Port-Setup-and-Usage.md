The serial port is most basic connection. It can fully control Grbl and it also sends useful data at startup and if there is a crash. 

The default settings are **115200 baud, N-8-1**. One of the easiest serial terminals to use is the one that comes with the Arduino IDE. You get to it via the magnifying glass icon and you need to adjust the settings in the lower right per the image below.

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/serial_console.png)

If you have the serial terminal open before you compile/upload, it will connect and show you some helpful information.

It will show you the cpu map you are using and some of the features associated with it. You can send **$I** to get the version number.

If you want to reboot the ESP32 to see the startup info, send **[ESP444]RESTART** or click the boot button on your module.





