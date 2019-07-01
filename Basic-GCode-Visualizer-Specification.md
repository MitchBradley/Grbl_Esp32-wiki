## Basic GCode Visualizer Specification

### Overview

The goal is to add basic gcode visualization to the WebUI. This will allow a quick visualization of the files on the SD card. An example of a [viewer is here](http://jherrm.com/gcode-viewer/).

### Features

- 3 Axis
- Able to rotate, pan zoom with mouse
- Color code by move type (G0, G1, G2/G3) No visualization of non move gcode. No visualization of extruder.
- Can be launched from WebUI browser
- Fast as possible loading. Do not animate loading.
- It is not intended to show job progress.

### Requirements

- Pack everything into one file. ESP32 server does not like multiple files. 
- Use [CND](https://en.wikipedia.org/wiki/Content_delivery_network)  to use online resources
- The web page file will be served from the SD card
- Gcode files will be read from the SD card
- Liberal license, such as MIT

### Discussion
- Ask via issues
- Ask via Slack