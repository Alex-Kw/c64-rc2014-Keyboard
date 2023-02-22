# c64-rc2014 Keyboard Adapter

![gOofLT3](https://user-images.githubusercontent.com/20172602/211600565-83e920cd-84e0-4166-a966-7464574937f7.jpeg)
![9QHPLzM](https://user-images.githubusercontent.com/20172602/211600421-cd21f051-5454-4a4b-aee5-e41381c1cbc0.jpeg)

This is an Adaptor to use a Commodore 64 Keyboard with a RC2014 Serial input. It's intended to make using BASIC, CP/M, and RomWBW from a physical terminal a good experience.

# Features:

**- SOFT CAPSLOCK: F1 and F2 toggle a soft CAPSLOCK mode.** This was added because the SHIFT LOCK key is mechanical and affects the number row. The soft caps lock affects just alpha characters, and is very useful for programming in BASIC in uppercase. The keyboard boots in mixed case, F1 enables this special CAPSLOCK mode, and F2 (SHIFT+F1) disables it.
**- CTRL key codes for CTRL-[A-Z]** use the Commodore key as a functional CTRL key. This is quite useful for RomWBW and advanced CP/M usage.
**- CTRL key codes work with SOFT CAPSLOCK enabled or disabled.** Note: if shift lock or a shift key is held down, it becomes a different key value.

# Additional mappings and macros:

- Left arrow as ESC
- Up arrow mapped as ^
- RUN STOP mapped as Ctrl-C which will break out of BASIC loops.
- CLR/HOME mapped as Ctrl-L which will clear screen at CP/M Prompt
- F3 - macro to type 'CPM' and a carriage return
- F4 - Type escape char for qterm (ctrl-y). Doesn't display, press ? after for help.
- F5 - macro to type 'MBASIC' and a carriage return
- F7 - macro which types out a command to clear the screen in MBASIC, and a carriage return.
- F8 - macro to type 'OUT 0 , 0' and a carriage return (turn off digital output 0 in MBASIC) 

Booting an RC2014 with SCM and getting to MBASIC is then a matter of a few keypresses. Power on the system, press 'RESTORE' to reset it, and then pressing F3, F5, F7 in order will launch CPM, MBASIC, and clear the screen (on a system with Compact flash and the necessary software). These macros are in clear text strings and easily changed.

# PCB INFO
This project is currently using P-LAB Appledore PCB; A serial output header matching the RC2014 is present on Ver 1.1a of this PCB. Serial Baud is hard coded currently at 115200 but you can set it as you see fit in the arduino sketch if your system is running at a different clock speed or if you are using the dual clock module to adjust a secondary baud rate. This baud rate of 115200 assumes 'defaults' of the RC2014 Pro. 

PCB and original project/firmware is for using C64 keyboards on Apple1/Apple II and can be found here:
https://p-l4b.github.io/appledore/

If using the 1.0 version of the PCB, you could add a serial header like I did in prototyping. Photos of the modification to add a serial header:
https://i.imgur.com/wXjShyj.jpg

The above mod is not needed on the version 1.1a Appledore PCB. 

# RESETTING THE RC2014, AND A NOTE ABOUT RESET USING RESTORE KEY: 
DON'T CONNECT A RESET WIRE UNLESS YOU KNOW WHAT YOU ARE DOING. When you connect the Appledore 1.1a to your RC2014, the Appledore will have reset on pin 6, but the RC2014 will not have anything on its side wired to that pin. To install a reset wire, connect the reset pin of the bus (pin 20 on a RC2014 Pro) to an unused pin on the serial header (pin 6) of the RC2014 PCB you use for serial input. This could be a Dual SIO or Pi terminal for example. On the Appledore, the reset pin goes to the reset output OF THE ASCII TTL header. If you are wiring things yourself, Do -not- connect the bus pin to the Nano's RST pin. You can also in concept simply connect the ASCII TTL RST pin on the Appledore PCB straight to pin 20 of the bus (RST) of your RC2014, and the RESTORE key on the C64 keyboard will pull the pin low to trigger a reset. Ver 1.1a of the Appledore PCB provides the correct RESET signal on pin 6 of the serial header, an unused pin on both the pi and dual sio modules as far as the schematics report. 

In the firmware, several negative values for special keys are converted to positive values. I left these in the comments so you may move things like the ctrl-c (RUN STOP) function around. RESTORE/RESET cannot be moved on the key map, it uses an electical circuit on the appledore, not code. Version 1.1 of the Appledore PCB has a reset button present on the actual PCB. This works in addition to the RESTORE key to reset the system.

**The "Apple II" problem:**

When Apple updated the Apple II keyboard for the IIe, the reset button was moved away from an area where it could cause accidental resets. If you don't connect a reset wire from the Appledore to your RC2014 in the first place, this doesn't matter. However, if you do want reset functionality on the Appledore PCB pusbutton, but not on the commodore keyboard, cut/remove pin 3 of the keyboard connector (the first pin AFTER the key slot) where it attaches to the appledore, or cut the trace. Then, the reset pushbutton on the Appledore will work, but you won't accidentally reset your system with the RESTORE key. 

# NOTES ABOUT CTRL KEY USAGE:
As noted above, CTRL-[A-Z] can be sent with Commodore key + [A-Z]. How this actually works is the keypress of the COMMODORE key sets a flag for ctrlkey in the key scanning loop. It's OK to hold the key down, as regardless, the next key pressed is offset in decimal value to make it a ctrl keycode. You can technically tap COMMODORE and then your Alpha key, but holding it as a combo and releasing both as you would do on a normal keyboard / modern computer will work. I only note this here in case you bump the commodore key, know that your next [KEY] will be CTRL+[KEY]. If you press COMMODORE and you do not want to submit a CTRL-[KEY] press, You can cancel this (counter-intuitively) by pressing the ctrl button itself to unset ctrlkey mode. Otherwise ctrlkey mode is exited automatically after sending a keypress. 

Full album of photos (prototype):
https://imgur.com/a/SZrAeKl

Photos of Finished 1.1a Ver:
https://imgur.com/a/DErxG2J

Video of functionality:
https://www.youtube.com/watch?v=GENigrzKh0U

# Instructions:
Build version 1.1a of the AppleDore PCB and install the firmware from this repo. Keep in mind this intends for the Arduino NANO to get power from the RC2014 Serial port. On the Pi serial module +5 is present by default, on the dual SIO you must install a jumper to enable its presence. Specifically For the RC2014 Pro with a Pi serial module, reset functionality is added by a jumper wire on the Pi terminal card. Wire bus pin 20 to pin 6 of the serial header on the pi terminal PCB.  

# Programming:
In the Arduino software simply load the sketch, select the correct port/device, and use Arduino as ISP. Nothing fancy, it should 'upload' rather quickly. If it doesn't work, try the 'old bootloader' option for the NANO's processor, located in the board select menus of the Arduino IDE.

# Connecting:
You built a RC2014... Connect it to the serial bus using all 6 pins of the serial headers (in order).

hint: The NANO pins are misleadingly labelled, RX0 goes to RX not TX, at least on my Pi serial module. If you get no input from the keyboard after resetting your RC2014 try swapping RX/TX. 

**RC2014 is a trademark belonging to RFC2795 Ltd, this project is not affiliated with RFC2795 and is not an RC2014 product; only RC2014 compatible.**
