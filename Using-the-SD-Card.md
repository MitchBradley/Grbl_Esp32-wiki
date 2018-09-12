![SD Card](http://www.buildlog.net/blog/wp-content/uploads/2018/08/pny_micro_sd_card-150x150.jpeg)

## Overview

This documents the current command set. The commands and responses were chosen to in a Grbl style rather than a Marlin type style. This will allow existing Grbl senders to be used without issues. The current commands have been approved by Sonny (Grbl Dev) and will be reserved for this use to prevent future conflicts.

If you have any suggestions, please post them as an issue.

## Commands

**$FM**

Mount the SD card. This must be done before listing or sending files.

**$F**

Show all the files. This is recursive and will search all subdirectories. Each file will print like this...

[FILE:/FOO.NC,SIZE:29547]

...where /FOO.NC is the filename. including the directory. In this case the directory is the root. The number following the file name is the file size. This is not case sensitive. 

There is a filter for valid file types in grbl_sd.cpp. Only these types will display.

char fileTypes[FILE_TYPE_COUNT][8] = {".NC", ".TXT", ".GCODE"}; 

*Note:* There are a few naming restrictions because of how grbl strips out characters used for real time commands. It strips out the following characters...

- ' ' (space) it removes spaces to make the buffer more efficient and easier to parse.
- '?' used to get real time status
- '!' used for feed hold
- '~' used for cycle start

**$F=/FOO.NC**

This will run file /FOO.NC 

Note: If in alarm mode, this command will fail with error 9

## Other Actions

**Pause/Restart**

Just use the normal grbl cycle start and feedhold commands

**Stop/Quit a file**

Initially this will be Grbl Reset. If that does not work out then $FQ will be added.

**Status**

I am not sure how to send status. I think adding an optional thing to the ? is probably the best. Maybe a percentage that gets added in when an SD file is running.

<Idle|WPos:195.000,144.000,19.000|Bf:15,128|FS:0.000,0.000|Pn:P|WCO:-195.000,-144.000,-19.000|SD:45.5>
 

## Errors

There will be some sort of state machine type thing to make sure mount, run, unmount, etc do not cause undesirable effects.

## Future

Adding a card detect would be nice to eliminate the card mounting command, but I am not sure what I/O to reassign.