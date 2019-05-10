## There is basic SPI mode TMC2130 support in Grbl_ESP32

### Library

You must add the [TMC2130Stepper library](https://github.com/teemuatlut/TMC2130Stepper) to your Arduino IDE.

### Configuration

- See the CPU_MAP_TMC2130_PEN in [cpu_map.h](https://github.com/bdring/Grbl_Esp32/blob/master/Grbl_Esp32/cpu_map.h#L839) as an example.
- You need to put #define USE_TMC2130 in your CPU Map.
- Currently you will need to manually edit [TMC2130.cpp](https://github.com/bdring/Grbl_Esp32/blob/master/Grbl_Esp32/TMC2130.cpp) to change any values sent to the drivers
- See the documentation at the [TMC2130Stepper library](https://github.com/teemuatlut/TMC2130Stepper) for more details.
- See the [TMC2130 driver datasheet](https://www.trinamic.com/products/integrated-circuits/details/tmc2130/) too.

