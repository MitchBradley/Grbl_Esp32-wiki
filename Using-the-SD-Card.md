![SD Card](http://www.buildlog.net/blog/wp-content/uploads/2018/08/pny_micro_sd_card-150x150.jpeg)

## Overview

All commands are in the [ESP...] format. They are defined [in this document](https://github.com/bdring/Grbl_Esp32/blob/master/doc/Commands.txt).

Note: If you have authentication enabled (which is not the default), you will need to supply a password for some of the commands. Example [ESP210]pwd=admin  (this assume you are using the default password of "admin")

## Commands

### Get SD Card Status
[ESP200] pwd=<user/admin password>

### Get SD Card Content
[ESP210] pwd=<user/admin password>

Show all the files. This is recursive and will search all subdirectories. Each file will print like this...

[FILE:/FOO.NC|SIZE:29547]

...where /FOO.NC is the filename. including the directory. In this case the directory is the root. The number following the file name is the file size.

There is no filter, all files and folders are reported. Senders, WebUI, etc should handle this from now on.

### Print SD file
[ESP220] <Filename> pwd=<user/admin password>

This will run file /FOO.NC 

Note: If in alarm mode, this command will fail with error 9



## Other Actions

**Pause/Restart**

Just use the normal grbl cycle start and feedhold commands

**Stop/Quit a file**

Use Grbl Reset. The best way to do this is to do a feedhold then a Grbl Reset. The last line will be reported.

Keep in mind that the feedhold will have stopped a move in progress and there will be more moves in the buffer. Restarting and reseting all the modal things is very tricky and left to the sender.

**Errors**

Any gcode errors in the SD card file will terminate the job. The offending line number of the file will be reported.

**Status**

When an SD card job is running, the percent complete is appended to the status string. This is simply percent of bytes read from the file.

<Idle|WPos:195.000,144.000,19.000|Bf:15,128|FS:0.000,0.000|Pn:P|WCO:-195.000,-144.000,-19.000|SD:45.5> 

### Card Formatting ###

Some people have trouble when SD cards have been formatted by Windows, but were able to solve the problem by formatting with [SD Card Formater](https://sd-card-formatter.en.uptodown.com/windows)