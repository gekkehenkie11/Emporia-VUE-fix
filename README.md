If you have a 3 phase system at home and you have hooked up 2 phases to 1 breaker, then you'll need this fix. 
If you have a 2 phase system and there's a 120 degrees angle between your two phases (so it's not really true two phases) you will also need this. 

Right now it's quick an dirty. I changed it so that the first mux blocks assume 2 phases to the breaker. So this means port:

10, 2, 11, 3, 12, 4, 9, 1

For the other ports it behaves as stock firmware. 

To flash, right now you'll need a Jlink adaptor or something compatible. Then hook up to the H5 port of your VUE: Vtref, GND, SWDIO, SWCLK and nReset. So that's pin 1, 5, 2, 4 and 10. 
You can just flash with Jlink Commander for example, with command "Loadfile". Don't forget to disconnect all wires when you're done flashing, the reported values will not be correct if even 1 wire is still connected!

Then in the esphome firmware you can just alternate between the 3 phases to find the right one. If you get a negative value, just reverse the clamp.
