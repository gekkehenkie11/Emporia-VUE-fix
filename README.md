WARNING ONLY TESTED ON VUE2 !!!!

If you have a 3 phase system at home and you have hooked up 2 phases to 1 breaker OR if you have a 2 phase system and there's a 120 degrees 
angle between your two phases (so it's not really true two phases) you might want to flash this fix. It will allow you
to use only 1 CT on one of the two legs. 

Right now it's quick and dirty. I changed it so that the first mux blocks assume 2 phases to the breaker. So this means port:

10, 2, 11, 3, 12, 4, 9, 1. So, rewritten, that's port 1, 2, 3, 4 and 9, 10, 11, 12. So these ports are now reserved for 2 phases.

For the other ports it behaves as stock firmware, so that's L+N connected. 

So I now have 8 ports configured for 2 phase connection and the others should be between phase and neutral. 
If you want to change this number you can simply change line 618 where it says

if (Muxnr < 4)

to for example

if (Muxnr < 2)

So then it would only use the first 4 ports for our special treatment. So if you only have 4 devices at home that need 2 phases then you could do it like that.

The port ordering is:

10, 2, 11, 3, 12, 4, 9, 1, 13, 5, 16, 8, 14, 6, 15, 7

So if you changed it into Muxnr < 2, it would then have ports 10, 2, 11 and 3 for 2 phases.

HOW TO COMPILE:
You'll need the have gcc-arm-none-eabi installed, see here how to do that: https://askubuntu.com/questions/1243252/how-to-install-arm-none-eabi-gdb-on-ubuntu-20-04-lts-focal-fossa

HOW TO FLASH:
To flash, right now you'll need a Jlink adaptor or something compatible. I used a Jlink EDU myself but there are a lot cheaper adapters that work well too. Make sure you select ATSAMD09D14A as chip.
Then hook up to the H5 port of your VUE: Vtref, GND, SWDIO, SWCLK and nReset. So that's pin 1, 5, 2, 4 and 10. See: https://www.g3gg0.de/wordpress/wp-content/uploads/2023/06/grafik-2.png
You can just flash with Jlink Commander for example, with command "Loadfile" (after of course having issued 'connect' to connect to the chip). 
Don't forget to disconnect all wires when you're done flashing, the reported values will not be correct if even 1 wire is still connected!

Then in the esphome firmware you can just alternate between the 3 phases to find the right one. Or if you know which 2 phases are connected:
Phase a (in yaml) = L1 - L2.

Phase b (in yaml) = L1 - L3.

Phase c (in yaml) = L2 - L3.

If you get a negative value, just reverse the clamp (or multiply the result with -1)

I've also just uploaded out.bin which is a precompiled version, ready to be flashed. And I've also just uploaded "out_full_16_ports_220V.bin" which expects 220V on all 16 ports, so 
in case you want a dedicated VUE 2 phase connections entirely (so each clamp goes to one of the 2 phases of each breaker that's connected to 2 out of 3 phases in your house)
I've also just uploaded 2 other preflashed bins in case you want only 2 or 4 ports to connect to 2 phases.

For more technical information see: https://github.com/emporia-vue-local/esphome/discussions/287
