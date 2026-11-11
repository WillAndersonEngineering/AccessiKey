# AccessiKey

AccessiKey is a team-built joystick keyboard prototype for people with limited fine motor control. A user can type a letter with two directional movements. The Arduino Micro reads the joystick, displays the active key group on a 2-inch SPI LCD, and sends the selected key over USB HID. The [original project](https://github.com/WillAndersonCU/AccessiKey) has prototype photos and a wiring diagram.

## How it works

1. Read both joystick axes and ignore movement near the center.
2. Divide the stick angle into eight directions.
3. Use the first direction to enter a group and update the LCD. Use the second to send a key or return to the base layer.

For example, move **up** to open **A–G**, then **down-left** to send **a**. Move **down** from that group to return to the base layer. The letter groups, number groups, and modifier behavior are implemented in the sketch.

## Hardware

| Signal | Firmware configuration |
| --- | --- |
| Joystick X / Y | `A1` / `A0` |
| LCD clock / data | Arduino Micro hardware SPI SCK / MOSI |
| LCD CS | Arduino pin `17` |
| LCD DC / reset / backlight | Arduino pins `7` / `8` / `9` |

The LCD control pins are defined in [DEV_Config.h](firmware/AccessiKey/src/display/DEV_Config.h). Check the power and logic requirements of your joystick and display modules before wiring.

## Build and try it

Open [AccessiKey.ino](firmware/AccessiKey/AccessiKey.ino) in Arduino IDE with **Arduino Micro** selected and Arduino's **Keyboard** library installed, or compile from the repository root:

```sh
arduino-cli core install arduino:avr
arduino-cli lib install Keyboard
arduino-cli compile --fqbn arduino:avr:micro firmware/AccessiKey
```

This layout compiled with Arduino AVR Boards 1.8.8 and Keyboard 1.0.7. The compile check did not include flashing the board.

Upload over USB. Open a blank text editor, center the stick, then try the **up → down-left** sequence and check that the display and typed character agree.

The main sketch contains the input logic and screen layouts. The [display driver](firmware/AccessiKey/src/display/) contains the LCD support code and font assets, with their original Waveshare and STMicroelectronics notices.
