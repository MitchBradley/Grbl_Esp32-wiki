![SD Card](http://www.buildlog.net/blog/wp-content/uploads/2018/08/pny_micro_sd_card-150x150.jpeg)

## Overview

This documents the current command set. The commands and responses were chosen to in a Grbl style rather than a Marlin type style. This will allow existing Grbl senders to be used without issues. The current commands have been approved by Sonny (Grbl Dev) and will be reserved for this use to prevent future conflicts.

If you have any suggestions, please post them as an issue.

## Commands

**$FM**

Mount the SD card. This must be done before listing or sending files.

**$F**

Show all the files. This is recursive and will search all subdirectories. Each file will print like this...

[FILE:/FOO.NC|SIZE:29547]

...where /FOO.NC is the filename. including the directory. In this case the directory is the root. The number following the file name is the file size. This is not case sensitive. 

There is no filter, all files and folders are reported. Senders, WebUI, etc should handle this from now on.

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

Initially this will be Grbl Reset. If that does not work out then $FQ will be added.The best way to do this is to do a feedhold then a Grbl Reset. The last line will be reported.

Keep in mind that the feedhold will have stopped a move in progress and there will be more moves in the buffer. Restarting and reseting all the modal things is very tricky and left to the sender.

**Errors**

Any gcode errors in the SD card file will terminate the job. The offending line number of the file will be reported.

**Status**

When an SD card job is running, the percent complete is appended to the status string. This is simply percent of bytes read from the file.

<Idle|WPos:195.000,144.000,19.000|Bf:15,128|FS:0.000,0.000|Pn:P|WCO:-195.000,-144.000,-19.000|SD:45.5> 

**Errors**

There will be some sort of state machine type thing to make sure mount, run, unmount, etc do not cause undesirable effects.

## Future

Adding a card detect would be nice to eliminate the card mounting command, but I am not sure what I/O to reassign.