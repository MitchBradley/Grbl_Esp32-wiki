# Custom Machine Functions

If you have a machine that needs some special features there are some hooks in Grbl_ESP32 that can call custom functions for your machine. You need to write these functions. The Atari 1020 machine is a good example. See that for reference (atari_1020.h & atari_1020.cpp) in the existing source code files.

## Code Files

You should create a custom .h and .cpp file for your machine, like atari_1020.h and atari_1020.cpp. Put these files in the same folder as all the other firmware files. Inside your cpu_map definition in cpu_map.h place an include like #include "atari_1020.h". Try to do everything using those 2 files only.  

## Machine Initialization

If your machine needs some initial setup, like defining some I/O or starting a background task you can put **#define USE_MACHINE_INIT** in your header file. It will call a function **void machine_init()** that you must provide.

## User Defined Homing

If normal homing won't work for you or you need to do something special before homing, you can write your own function. Put **#define USE_CUSTOM_HOMING** in your header file. Grbl_ESP32 will see this when it goes to home the machine. It will call a function, **bool user_defined_homing() ** that you must provide. If you return **true** from this function, it tells Grbl_ESP32 that you are handling all of the homing. If you return **false**, Grbl_ESP32 will then run its normal homing sequence.

User Defined Macro Buttons



## User tool change

Normally Grbl_ESP32 ignores tool changes. It just tracks the current tool number. If you put **#define USE_TOOL_CHANGE** in you header file, it will call a function **void user_tool_change(uint8_t new_tool)** when it sees the M6 gcode command.

## M30 Command

The M30 gcode command signals the end of a file. Normally Grbl ignores the M30 command. If you put **#define USE_M30** in your header file, it will call a function **void user_m30()** that you must provide when it sees the M30 command.