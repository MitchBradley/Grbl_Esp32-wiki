## Macro Button Feature

### Overview

Macro buttons allow you to associate a switch closure with a simple command like homing or running a file from the SD card.

### Electrical

Buttons are typically used as normally open GPIO inputs that switch to ground. They need a pull up resistor to hold the pin in a high state when not closed to ground. Most pins have an internal pull up that will be used. GPIO pins 34 through 39 do not have a pull up resistor and you will need to add them. A 10k pull up to 3.3v is typical.

**Advanced:** You can use normally closed switches. You will have to invert the logic using the **#define INVERT_CONTROL_PIN_MASK** in config.h

### Implementation

Macro buttons are similar to the control switches (reset, hold, etc). You can define up to (4) of them. You add them to your cpu_mpa like...

```#define MACRO_BUTTON_0_PIN		GPIO_NUM_13```

If it detects a macro button push, it will call a function like this.

```user_defined_macro(CONTROL_PIN_INDEX_MACRO_0);  ```

This function must be created by the user and added to the firmware. This is typically done an a new file.

The easier way to execute macro functions is via the Serial2Socket.push(...) function, like this for homing.

```inputBuffer.push("$H\r");``` 

**Note:** Make sure `IGNORE_CONTROL_PINS` is not NOT defined. You probably should also define `ENABLE_CONTROL_SW_DEBOUNCE`

### Example

The Polar Coast adds (3) macro buttons. You can look at that cpu_map. polar_coaster.cpp has the user_defined_macro function.  It has a home button , a button to run gcode file 1.nc and a button to run 2.nc for the SD card. This is a simple way to make a headless machine.