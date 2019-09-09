## Macro Button Feature

### Overview

Macro buttons allow you to associate a switch closure with a simple command like homing or running a file from the SD card.

### Implementation

Macro buttons are similar to the control switches (reset, hold, etc). You can define up to (4) of them. You add them to your cpu_mpa like...

```#define MACRO_BUTTON_0_PIN		GPIO_NUM_13```

If it detects a macro button push, it will call a function like this.

```user_defined_macro(CONTROL_PIN_INDEX_MACRO_0);  ```

This function must be created by the user and added to the firmware. This is typically done an a new file.

The easier way to execute macro functions is via the Serial2Socket.push(...) function, like this for homing.

```inputBuffer.push("$H\r");``` 

### Example

The Polar Coast adds (3) macro buttons. You can look at that cpu_map. polar_coaster.cpp has the user_defined_macro function.  It has a home button , a button to run gcode file 1.nc and a button to run 2.nc for the SD card. This is a simple way to make a headless machine.