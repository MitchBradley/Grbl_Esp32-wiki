# The is currently a proposal only.

## Overview

The command style is temporary and subject to change. The commands and responses were chosen to in a Grbl style rather than a Marlin type style. This will allow Grbl senders to be used without issues. If a Marlin style is preferred, changes can be made after the system is fully tested.

## Commands

**$F**

Show all the files. This is recursive and will search all subdirectories. Each file will print like this...

[FILE: \FOO.NC 29547]

...where \FOO.NC is the filename. including the directory. In this case the directory is the root. The number following the file name is the file size.

There is a filter for valid file types in grbl_sd.cpp

char fileTypes[FILE_TYPE_COUNT][8] = {".nc", ".txt", ".gcode"}; 

There are a few naming restrictions because of how grbl strips out characters. I'll need to post those rules or figure out a way around that. Using **line_flags** like **LINE_FLAG_COMMENT_PARENTHESES** might work.

**$FM**

Mount the SD card


**$F=\FOO.NC**

Run file \FOO.NC 

## Other Actions

**Pause/Restart**

Just use the normal grbl cycle start and feedhold commands

**Stop/Quit a file**

Initially this will be Grbl Reset. If that does not work out then $FQ will be added.

**Status**

I am not sure how to send status. I think adding an optional thing to the ? is probably the best. Maybe a percentage that gets added in when an SD file is running.

<Idle|WPos:195.000,144.000,19.000|Bf:15,128|FS:0.000,0.000|Pn:P|WCO:-195.000,-144.000,-19.000|SD:45.5%>
 

## Errors

There will be some sort of state machine type thing to make sure mount, run, unmount, etc do not cause undesirable effects.

