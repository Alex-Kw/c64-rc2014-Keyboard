# c64-rc2014 Keyboard Adapter

![58rus7l](https://user-images.githubusercontent.com/20172602/207116664-d4ca4d4c-cb51-452a-bf97-7e2bc2365fd5.jpeg)

This is an Adaptor to use a Commodore 64 Keyboard with a RC2014 Serial input. It is currently using P-LAB Appledore PCB; A serial output header must be added at this time until/unless the PCB is revised. Baud is hard coded currently at 115200 but you can set it as you see fit in the arduino sketch if your system is running at a different clock speed or if you are using the dual clock module to adjust a secondary baud rate. This assumes 'defaults' of the RC2014 Pro. 

PCB and original project/firmware is for using C64 keyboards on Apple1/Apple II and can be found here:
https://p-l4b.github.io/appledore/

I am using the 1.0 version of the PCB. Photos of the modification to add a serial header:
https://i.imgur.com/wXjShyj.jpg

NOTE ABOUT RESET USING RESTORE KEY: I used a pin on the Pi serial module that is unused, but it is used on the normal / Dual SIO modules. DON'T CONNECT A RESET WIRE UNLESS YOU KNOW WHAT YOU ARE DOING. In my case, I connected the reset pin of the bus (pin 20) to an unused pin on the serial header. On the Appledore, that pin then goes to the reset output OF THE ASCII TTL header. Do -not- connect it to the Nano's RST pin. You can also in concept simply connect the ASCII TTL RST pin on the Appledore PCB straight to pin 20 of the bus (RST) and the RESTORE key on the C64 keyboard will pull the pin low to trigger a reset.

In the firmware several negative values for special keys are converted to positive values. I left these in the comments so you may move things like the ctrl-c (RUN STOP) function around. RESTORE/RESET cannot be moved on the key map, it uses an electical circuit on the appledore, not code. Version 1.1 of the Appledore PCB has a reset button present on the actual PCB but I have not used that revision for an RC2014 yet (No reason to believe it would not work).

SOFT CAPSLOCK: F1 and F2 toggle a soft CAPSLOCK mode. This was added because the SHIFT LOCK key is mechanical and affects the number row. The soft caps lock [which affects just alpha characters] is very useful for programming in BASIC in uppercase without miskeying symbols or numbers. The keyboard boots in mixed case, F1 enables this special CAPSLOCK mode, and F2 (SHIFT+F1) disables it.

Additional mappings and macros:

- Left arrow as ESC
- Up arrow mapped as ^
- RUN STOP mapped as Ctrl-C which will break out of BASIC loops.
- CLR/HOME mapped as Ctrl-L which will clear screen at CP/M Prompt
- F3 - macro to type 'CPM' and a carriage return
- F5 - macro to type 'MBASIC' and a carriage return
- F7 - macro which types out a command to clear the screen in MBASIC, and a carriage return.
- F8 - macro to type 'OUT 0 , 0' and a carriage return (turn off digital output 0 in MBASIC) 

Booting up an RC2014 with SCM and getting to BASIC is then a matter of a few keypresses. Power on the system, press 'RESTORE' to reset it, and then pressing F3, F5, F7 in order will launch CPM, MBASIC, and clear the screen (on a system with Compact flash and the necessary software). 

Full album of photos:
https://imgur.com/a/SZrAeKl

Video of functionality:
https://www.youtube.com/watch?v=GENigrzKh0U

Instructions:
Build version 1.0 of the AppleDore PCB, and connect to the Arduino Nano's serial however you see fit. This could be soldering right to TX1 and RX0 or adding some sort of port. At the imgur album above you can see how I created a 5 pin port, which also powers the Arduino NANO from the RC2014 Serial port.

Programming:
In the Arduino software simply load the sketch, select the correct port/device, and use Arduino as ISP. Nothing fancy, it should 'upload' rather quickly

Connecting:
You built a RC2014... Connect it to the serial bus ;)

hint: The NANO pins are misleadingly labelled, RX0 goes to RX not TX, at least on my Pi serial module. If you get no input from the keyboard after resetting your RC2014 try swapping RX/TX. 
