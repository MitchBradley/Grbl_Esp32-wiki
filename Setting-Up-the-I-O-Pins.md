# Setting Up the I/O Pins

Most I/O setup is done in the [cpu_map.h](cpu_map.h) file. Note: All I/O is 3.3V and the current is very low. Do not connect anything directly to relays, solenoids, etc.

There are more features than available I/O pins, so you need to determine what features you want.

Here are the pins you can use. Be very careful to only use each I/O pin in one place.

### Usable I/O pins

- GPIO_NUM_2
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

### Input Only

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

All spindles are variable speed. If you use an on/off spindle, just set the max RPM in the $$ setting to 1. That will mean any RPM will be full on.

Spindle Enable and direction are typically not used on most machines. The features can be enabled in firmware by assigning them a GPIO pin. If Spindle direction is not used, the M4 command is not supported.









