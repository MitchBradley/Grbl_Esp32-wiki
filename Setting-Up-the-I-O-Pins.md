# Setting Up the I/O Pins

Most I/O setup is done in the [cpu_map.h](https://github.com/bdring/Grbl_Esp32/blob/master/Grbl_Esp32/cpu_map.h) file. Note: All I/O is 3.3V and the current capability is very low. Do not connect any I/O directly to relays, solenoids, etc.

There are more features than available I/O pins, so you need to determine what features you want.

Here are the pins you can use. Be very careful to only use each I/O pin in one place.

### Usable I/O pins

- GPIO_NUM_2 (Some dev boards have an LED on this. It works better as an output)
- GPIO_NUM_4
- GPIO_NUM_5 -  (Used by SD Card)
- GPIO_NUM_12
- GPIO_NUM_13
- GPIO_NUM_14
- GPIO_NUM_15
- GPIO_NUM_16
- GPIO_NUM_17
- GPIO_NUM_18 -  (Used by SD Card)
- GPIO_NUM_19 -  (Used by SD Card)
- GPIO_NUM_21
- GPIO_NUM_22
- GPIO_NUM_23 -  (Used by SD Card)
- GPIO_NUM_25
- GPIO_NUM_26
- GPIO_NUM_27
- GPIO_NUM_32
- GPIO_NUM_33

### Input Only (no pullup/pulldown)

- GPIO_NUM_34
- GPIO_NUM_35
- GPIO_NUM_36
- GPIO_NUM_37
- GPIO_NUM_38
- GPIO_NUM_39

### Don't Use (or not recommended)

- GPIO_NUM_0 - This is used for the bootloader
- GPIO_NUM_1 - Used for Serial Data
- GPIO_NUM_3 - Used for Serial Data
- GPIO_NUM_6 - Use for External Flash 
- GPIO_NUM_7 - Use for External Flash
- GPIO_NUM_8 - Use for External Flash
- GPIO_NUM_9 - Use for External Flash
- GPIO_NUM_10 - Use for External Flash
- GPIO_NUM_11 - Use for External Flash
- GPIO_NUM_20 - This is not available on ESP32s
- GPIO_NUM_24 - This is not available on ESP32s
- GPIO_NUM_28 - This is not available on ESP32s
- GPIO_NUM_29 - This is not available on ESP32s
- GPIO_NUM_30 - This is not available on ESP32s
- GPIO_NUM_31 - This is not available on ESP32s



### Optional Features affecting I/O Pins

#### SD Card

The SD card uses 4 GPIO pins. They are currently only supported on the pins listed above. You can free up all these pins by commenting out #define ENABLE_SD_CARD  is config.h

#### Coolant

Flood and Mist control are output signals that can be used to drive a relay circuit. They are both optional. Uncomment either or both and assign a pin to turn on the feature(s). If the feature is not turned on you will get an invalid gcode error if you try to use them. 

#### Spindle

Spindle can be disabled by commenting out the #define SPINDLE_PWM_PIN line. Grbl will still behave like it has a spindle, but will not output a spindle signal. This can help with machines like pen plotters that don't use a spindle.

Spindle Enable and direction are typically not used on most machines. The features can be enabled in firmware by assigning them a GPIO pin. If Spindle direction is not used, the M4 command is not supported.

All spindles are variable speed. If you use an on/off spindle, just set the max RPM in the $$ setting to 1. That will mean any RPM will be full on.

#### Step and Direction Pins

You can comment out any step and direction pins. This will disable the output signals and free up pins for other uses. Grbl will use those axes, but not output the signals. This could be useful for hobby servo driven axes.

#### Ganged Axes & Axis Squaring

Many CNC machines use (2) motors on one or more axes. In some cases you would gang these in hardware. In other cases you many want to be able to control the motors separately enough to allow squaring of the axes during homing.

Squaring uses the motors and homing switches to separately home each motor. To limit the number of I/O pins, this is done in a special way. Each axis will only use (1) home switch pin, (1) direction pin, and (2) step pins. Each motor has its own home switch switch, but they are wired to the same I/O pin. The homing sequence will drive both motors towards the switches. As soon as one switch is touched, each motor will will separately home. As long as the switches are square, the axis should now be square. The $1, step idle delay, should be set to 255 to prevent the motors from turning off to preserve the squareness held by the motors.

See the CPU_MAP_MPCNC as a good example of this.

## Defining a custom cpu map.

The default cpu map is defined as CPU_MAP_ESP32 in [cpu_map.h](https://github.com/bdring/Grbl_Esp32/blob/master/Grbl_Esp32/cpu_map.h). There are several others defined as well that can serve as examples of setting up a complete machine definition. 









