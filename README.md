# TashTrio

Firmware that emulates a trio of devices for Apple's ADB bus: a keyboard, a mouse, and a Global Village TelePort A300 ADB modem.  The emulated keyboard and mouse are controlled by a PS/2 keyboard and mouse, the emulated modem is connected to a two-wire UART.

## Special Keys

The menu key on the PS/2 keyboard is used as a modifier key which can be held and combined with the following keys:

| Key   | Function                       |
| ----- | ------------------------------ |
| F1    | Change UART baud rate to 300.  |
| F2    | Change UART baud rate to 1200. |
| F3    | Change UART baud rate to 2400. |
| F4    | Change UART baud rate to 4800.  Note that this may cause the device connected to the UART to send faster than the Macintosh can receive, which will result in undefined behavior. |
| Pause | Send power key keypress.  Note that this cannot be used to start up the Macintosh, as the microcontroller requires power to be able to read the PS/2 keyboard. |

## Caveats

PS/2 mice require their host to send a command byte to start the flow of mouse movement data.  TashTrio firmware sends this byte on startup only, meaning that the PS/2 mouse must be plugged in at powerup time or it will appear dead.  Because some newer Macintosh computers are known to supply power to the ADB port even when powered down, it is necessary to connect the PS/2 mouse before connecting to the ADB.

## Building Firmware

Building the firmware requires Microchip MPASM, which is included with their development environment, MPLAB.  Note that you **must** use MPLAB X version 5.35 or earlier or MPLAB 8 as later versions of MPLAB X have removed MPASM.

### Download links

[MPLAB Archives](https://www.microchip.com/en-us/tools-resources/archives/mplab-ecosystem)

[MPLABX-v5.35-windows](https://ww1.microchip.com/downloads/en/DeviceDoc/MPLABX-v5.35-windows-installer.exe)



## PS2toADB board
Just a quick, minimal schematic.
![YO](/images/board1.PNG)

![Y1](/images/top1.PNG)

## Writing the firmware to the PIC12
ICSP LVP with just an FT232R adapter appears to be one fast and cheap way. 

## Info on LVP limitations
There certain limitations on PIC12F1840 when programmed the LVP way. In this method, the MCLR pin must remain MCLR and cannot take GPIO functions. Therefore, RA3 cannot be used if the firmware is programmed in LVP mode.
RA3 is the PS/2 Keyboard Data signal, therefore the keyboard will not work if the firmware is programmed in LVP mode. 
When firmware is programmed in HVP mode (like with PICkit3), the RA3 pin can take GPIO function and the keyboard will work.

## PS/2 Keyboard workaround
Simply change input pin. RA1 (UART RX) can be used instead for PS2 keyboard data input.
The control on which pin is the keyboard connected could be established with a 2 pole switch.

## PICPgm on Windows
One option on Windows is PICPgm. For FT232R, you need to use the USB LVP programmer preset and remap the pins.
Attached a one-pager PICPgm hints for FT23.
![ICSP_LVP_FTDI](/images/PIC12_Programming_All-in-One.png)


## References
[Microchip PIC12F1840 Product page](https://www.microchip.com/en-us/product/pic12f1840)

[Microchip PIC12F1840 Datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/41441B.pdf)

[8-bit PIC design recommendations](https://developerhelp.microchip.com/xwiki/bin/view/products/mcu-mpu/8bit-pic/design-recommendations/)

[PICPgm with FT232 adapter](https://www.franksteinberg.de/FT232-PIC-Programmer.htm)

[TMK ADB Keyboard](https://github.com/tmk/tmk_keyboard/wiki/Apple-Desktop-Bus)

[Topic: TMK ADB to USB keyboard converter](https://geekhack.org/index.php?topic=14290.msg277407#msg277407)

[Microchip Application Note AN591 - Apple® Desktop Bus](https://ww1.microchip.com/downloads/en/AppNotes/00591b.pdf)

[PIC12F1840 Programming](https://www.northernsoftware.com/dev/pic12f/pic12f1840.htm)

[PIC12F1840 HVP Circuit](https://www.northernsoftware.com/nsdsp/hvp.htm)

[Microchip Application Note 589 - HVP parallel programmer](https://ww1.microchip.com/downloads/en/appnotes/00589a.pdf)