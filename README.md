# Ender-3-V2-Neo-Marlin-Firmware-Setup-Notes

Hi everyone! 

I'm going to go through some of my notes, struggles, etc while setting up my V2 Neo this year. I'd like to preface by saying that this is **NOT** the best printer to buy nowadays, as many better options are available even at a similar price. 

To begin, I ran into an issue almost instantly after starting my printer. I checked and **made sure to be on 120V beforehand using the switch on the back**, then turned it on. Everything seemed fine at first, but when I started a print, nothing happened at all. The preview showed 0 time, 0 layers, 0% complete, and so forth. With the default firmware, it wouldn't even print.

Luckily, I had planned to upgrade my printer firmware before even getting the printer. Using this guide: https://lash-l.github.io/ender3_v2_neo/firmware_update , I was able to do so with some struggles. I began by attempting to do this as instructed, building my own version of Marlin (the firmware) and compiling it to the microSD card for the printer. I did everything as best I could, but it would not compile onto the card for me. I am not particularly skilled with this sort of development, so it's likely my own error, but nonetheless I had to find another way.

Instead, I used a different guide that was for a different printer (Ender 3 V2) but it worked for mine as well. Here is the link: https://github.com/mriscoc/Ender3V2S1/wiki/How-to-install-the-firmware . After downloading the appropriate binary, I put it on my SD card and into the printer. This time, it seemed to turn on properly, but the screen was still off. Going back to the old guide, I opened it up and found out that it was a TJC screen, naturally the only one that needs to have firmware flashed, I downloaded it and used the same SD card to install it, then went back to the main machine where FINALLY it booted up properly. Although this doens't sound like much, all of this took about 2 hours total. 

Now that it was finally functioning, I continued with the first guide section called "Fine Tuning", which led me through setting up my slicer properly with good presets for different speeds and qualities, leveling the bed, getting the Z offset, etc. These all took about an hour total, and after I was able to start printing! However, there were still significant issues that bothered me, there were strange sections in prints that resembled elephants foot partway through but consistent across prints, seams, some small gaps on the first layer, and more. Let's go into detail on those.

### Slicer
Using the profiles in that guide, I had few issues getting my slicer configured and working for my printer. Cura is quite user friendly, so I had no trouble there.

### Leveling the bed
Now this is where I got stuck for a while. I used the bed tramming feature in Marlin, which probes each corner to tell you the offset and you adjust them to fix it. I adjusted all of mine to just about 0, which seemed great to me. I re-homed the printer, just to be sure, and suddenly they were all around .2mm again. I put them back to 0, did it again, and they were back to .2mm. I realized after a bit that instead of targeting 0, I had to **target about .20mm** for it to be consistent across prints. After, I had few issues with the tramming and didn't notice many issues in the prints due to it.

### Getting my Z offset
Just about everything online recommends using paper, which I did, and setting the probe off that test. This setting seemed to work fine and I didn't change it until today, where I began experimenting further. Using this model: https://www.printables.com/model/78307-z-offset-calibration-print , I was able to check for how to adjust my Z offset (Note: I already tuned my e-steps for the extruder, so over-under extrusion shouldn't be a factor. You can learn about this in the lash-l guide linked above). In doing so, I ended up realizing that it was about .1mm too high, and adjusting that helped with accuracy in my prints. Check out the image **probeoffsetcalibrationlayers.png** above to see it. It shows four single layer infill tests I printed to find my optimal Z probe offset. From left to right, the numbers are (in millimeters): -1.89,-1.94,-1.99,-2.04. Out of the options, the best looking one to me was -1.99, but a bit extra seems to help so I set mine at -2.01 going forward.

To elaborate, you can clearly see the gaps in my original setting of 1.89, and they remain in 1.94 to a lesser degree. At 1.99 they become smaller and more uniform, and at 2.04 the entire piece was stuck to the bed and bent when removed, showing some possible squish even with the gaps. For now, I'm sticking with that sweet spot of 2.01 that should work well.

### Print quality
In many of my prints, there was a significant band about 3/4" above the surface plate on every print. I tried flipping the lead screw, but the issue remained. After some research, I adjusted the tension on both the lead screw nut and the eccentric nuts on the Z axis. For the lead screw, **I tightened both bolts hand tight with the allen wrench, then released each by half a turn.** This added some slack that assisted with the binding in spots. I also **loosened and then lightly snugged up the nuts by the V wheels on each side**, to provide some tension but not so much as to resist the movement of the axis. These changes seemed to greatly decrease the band, making for much nicer prints. It also, alongside the Z offset change, made it possible to print at a .2mm layer height, instead of only .28mm before (it wouldn't stick to the bed at all, which in hindsight makes a lot of sense). You can see this huge difference in the image **bandingcomparison.jpg** above: left is before fixing Z axis binding (you can see the very obvious band) right is after these adjustments were made.

### Mesh leveling
As far as I can tell, it does it's job. One thing to note is to be sure to **preheat the bed, then home, then level.** I personally added a custom preheat setting to the toolbar on the main screen of the printer that heats the bed to 55°C and the nozzle to 100°C to make the startup for printing faster. If you only press the mesh level without pre-heating, it will home before heating the bed, throwing your entire mesh completely off. 

### Useful tips
If you use Cura, there is also a plug-in you can add to give you a picture preview of the part on the screen of the printer, which I have installed. Learn more here: https://github.com/mriscoc/Ender3V2S1/wiki/How-to-generate-a-gcode-preview 

### Personal Preference

I also printed a couple things to make my printer work a bit better. First, I found a screenholder that moves it closer in front and within the footprint of the printer (I can't find the source anymore, but hopefully mine is better anyways). However, it uses additional hardware to mount, which I didn't have. I adjusted the model to have slots built in that slide into the aluminum extrusion of the printer. You can find my model here or uploaded to this repo: https://cad.onshape.com/documents/af33bdea7fdd3ea30d09401f/w/1ec14669289cab5e2b495bab/e/6b54fc4dcec8c699b5a798f2 . Secondly, I printed a side spool holder to reduce strain on the filament when printing (https://www.thingiverse.com/thing:4748139). I also attempted to create a 3D printed bearing for the filament roller, as I noticed it getting stuck constantly when feeding. My printer probably wasn't tuned well enough to make it work, so it had too much friction and didn't actually spin. Luckily, the PLA on the surface of the spool holder itself had much less friction, making for a smoother turning action. My recommendation is to just print a thin cylinder to hold it up and give it a bit less friction.

Also, I customized my toolbar to have the things I find most useful. You can see what it looks like in the image **printerscreen.jpg** above. My config is auto home, cooldown, preheat Custom (100 nozzle, 54 bed), Auto build mesh, and preheat PLA (180 nozzle, 60 bed).

### I hope this helped, thanks for reading!
