### Using Bluetooth

- Make sure **#define ENABLE_BLUETOOTH** is not commented out in **config.h**.
- Use a serial port terminal to set the Bluetooth name using **$I=NAME**, where NAME is the Bluetooth name you want. I don't know all the naming rules, so keep it short and simple. There is no capability to use a password yet. Grbl converts all input to capital letters, so lowercase will cannot be used. 
- Reboot the ESP32 to turn on Bluetooth with that name. Grbl will now respond on either Bluetooth or Serial data. All Bluetooth sends are echo'd on the Serial port if you want to watch the data.
- Bluetooth is setup as a serial Bluetooth. This means when you pair it with a device, it will look like a serial port. This allow backward compatibility with existing serial port senders, like Universal GCode Sender
- **Caution:** Do not pair while running a job. The ESP32 will likely interrupt and/or watchdog issues while the stepper timer is running and the pairing process is running.

### Android

- Android works great! I suggest using Grbl Controller.

![Grbl Controller](http://www.buildlog.net/blog/wp-content/uploads/2018/07/JoggingTab.png)

### iPhone

- I have no clue. Is there even a sender for iPhone?

### Windows

- Windows is difficult. When you pair, it will create 2 ports. One is an incoming port and one is an outgoing. There is little documentation on what is going on here, but the incoming is for devices initiating the connection and the outgoing is for Windows initiating the connection. Use the outgoing one. Unfortunately, it is not always easy to determine which is the outgoing. Try them both. 
- I still have often issues using Bluetooth on Windows. Test the ESP32 Bluetooth using and Android phone before you blame the ESP32 for problems.