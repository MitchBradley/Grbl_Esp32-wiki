## Trinamic Driver Support

### Basic Trinamic Driver Support

First add **#define USE_TRINAMIC** to your cpu map. This allows Trinamic drivers to be setup. For each Trinamic driver you  use, you need to add a **#define X_TRINAMIC** for each axis. This tells Grbl that the X axis driver is a Trinamic driver. Next you need to tell it what driver it is with a statement like **#define X_DRIVER_TMC2130**. You then need to assign a chip select pin like **#define X_CS_PIN GPIO_NUM_17**. 

The rest of the stuff like step. direction, etc are the same as normal motor drivers.

Here is a section of a cpu map for reference.

```C++
#define USE_TRINAMIC  
#define TRINAMIC_DAISY_CHAIN
#define USE_RMT_STEPS
	
#define X_STEP_PIN      	GPIO_NUM_12
#define X_DIRECTION_PIN   	GPIO_NUM_14
#define X_TRINAMIC   	   	// using SPI control
#define X_DRIVER_TMC2130 	// Which Driver Type?
#define X_CS_PIN    		GPIO_NUM_17  // Daisy Chain, all share same CS pin
#define X_RSENSE			0.11f   // .11 Ohm current sense resistor
```



### SPI 

Currently only the default SPI. Your PCB should use GPIO_NUM_32 for MOSI, GPIO_PIN_19 for MISO, and GPIO_NUM_18 for SCK.

This SPI can be shared with the SD card.

### SPI Daisy Chain Mode

Normally each driver requires its own chip select pin (CS_PIN). There is a mode where they can all share the same CS_PIN. In this case you wire the data out pin of the first driver to the data in pin of the next driver. You keep doing this until the last driver. Its data out goes to back to the controller.



![](http://www.buildlog.net/blog/wp-content/uploads/2019/11/daisy_chain.png)

Grbl_ESP32 supports this mode. You set this up in your cpu map. Below in an example of X and Y being daisy chained. Note that both X_CS_PIN and Y_CS_PIN are mapped to the same pin.

```C++
#define USE_TRINAMIC  
#define TRINAMIC_DAISY_CHAIN
#define USE_RMT_STEPS
	
#define X_STEP_PIN      	GPIO_NUM_12
#define X_DIRECTION_PIN   	GPIO_NUM_14
#define X_TRINAMIC   	   	// using SPI control
#define X_DRIVER_TMC2130 	// Which Driver Type?
#define X_CS_PIN    		GPIO_NUM_17  // Daisy Chain, all share same CS pin
#define X_RSENSE			0.11f   // .11 Ohm
	
#define Y_STEP_PIN      	GPIO_NUM_27
#define Y_DIRECTION_PIN   	GPIO_NUM_26
#define Y_TRINAMIC   	   	// using SPI control
#define Y_DRIVER_TMC2130 	// Which Driver Type?
#define Y_CS_PIN    		X_CS_PIN  // Daisy Chain, all share same CS pin
#define Y_RSENSE			0.11f   // .11 Ohm
```

### $$ Settings

Several $$ settings are used for Trinamic motors. Read [this section](https://github.com/bdring/Grbl_Esp32/wiki/$$-Settings-Menu) to see more details.

### StallGaurd (SG)

SG is a feature of Trinamic motors where stalls can be detected and a signal can be routed to a pin. Some people like to use this to do sensorless homing. Basic Grbl does not support this. Advanced users can implement this using [custom machine functions](https://github.com/bdring/Grbl_Esp32/wiki/Custom-Machine-Functions).

This requires good programming skills and and a thorough understanding of the SG feature. Feel free to ask question on Slack about this, but it will be a lower priority for devs to answer than standard Grbl support. Consider a donation to the project if you are asking a lot of questions :-)

Very basically...SG works by looking at changes in motor efficiency. If a motor is running under a light load it is more efficient, if it starts to stall, the efficiency lowers. The driver's ability to detect this is affected by many things. It has trouble detecting changes at very high step rates and very low step rates. You need to find the best speed to home at for it to work well. Your machine setup will also affect the baseline efficiency. Its normal load, the length and gauges of the wires, etc lower the efficiency. There are several parameters you can change to "tune" SG so it works on your machine. Finally, SG is not super accurate. At best it is accurate to a few full steps. Normal switches are typically more accurate.

### Troubleshooting

**Note:** The drivers will not response without primary motor voltage. The ESP32 can appear to be working with USB only, but the drivers will not respond.



