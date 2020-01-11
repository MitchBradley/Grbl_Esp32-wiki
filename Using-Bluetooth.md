### Using Bluetooth

**Note:** The USB/Serial port is always on and the connection is never lost even during restarts and crashes. Therefore, very useful messages are sent to the serial port during bootup and when other events occur. It will tell you whether Bluetooth or Wifi are on, the modes they are in and the names being used. Passwords are never shown though.

The [ESP...] commands work on any interface (Serial, Bluetooth, Wifi, WebUI, etc), but you will only see startup info on the serial port because the others will not have started yet.

- Make sure **#define ENABLE_BLUETOOTH** is not commented out in **config.h**.
- Use a USB serial port terminal to set the Bluetooth name using the **[ESP140]< Bluetooth name>**. [See this doc](https://github.com/bdring/Grbl_Esp32/blob/master/doc/Commands.txt) on how to use [ESP...] commands. I don't know all the Bluetooth naming rules, so keep the name short and simple. There is no capability to use a password yet. Example: **[ESP410]ESP32_BT**. This would set the Bluetooth name to "ESP32_BT".
- Put the radio mode into Bluetooth mode with the [ESP110]BT. The ESP32 uses the same hardware for Wifi and Bluetooth, so only one can be used at a time.
- A reboot is required to change radio modes.  Reboot the ESP32 to turn on Bluetooth with that name. You can reboot by power cycling, pushing the reset button on the ESP32 module or sending the **[ESP444]RESTART** command.
- Bluetooth is setup as a serial Bluetooth. This means when you pair it with a device, it will look like a serial port. This allow backward compatibility with existing serial port senders, like Universal GCode Sender
- **Caution:** Do not pair while running a job. The ESP32 will likely interrupt and/or watchdog issues while the stepper timer is running and the pairing process is running.
-   **Tip:** If you are in BT mode you can switch back to Wifi from your Bluetooth console with [ESP110]STA or [ESP110]AP, then reboot

### Android

- Android works great! I suggest using Grbl Controller.

![Grbl Controller](http://www.buildlog.net/blog/wp-content/uploads/2018/07/JoggingTab.png)

### iPhone

- I have no clue. Is there even a sender for iPhone?

### Windows

- Windows is difficult. When you pair, it will create 2 ports. One is an incoming port and one is an outgoing. There is little documentation on what is going on here, but the incoming is for devices initiating the connection and the outgoing is for Windows initiating the connection. Use the outgoing one. Unfortunately, it is not always easy to determine which is the outgoing. Try them both. 
- I still have often issues using Bluetooth on Windows. Test the ESP32 Bluetooth using and Android phone before you blame the ESP32 for problems.