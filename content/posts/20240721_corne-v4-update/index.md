+++
title = "Corne V4 Update"
date = 2024-07-21
lastmod = 2024-12-26T17:56:42+11:00
tags = ["keyboards", "corne"]
categories = ["Blog"]
draft = false
weight = 2007
+++

I’ve been continuing my journey into split mechanical keyboards. The 46-key Corne v4 Board that I recently bought seemed to have an unreliable USB connection on the left-hand side, with it losing power if the cable was lightly depressed. If the USB cable was connected to the right-hand side then it worked reliably. I decided to disassemble the left hand side and inspect the soldering on the USB connector to see if there could be a dry joint.

I immediately ran into an issue where the case was secured with torx screws rather than phillips heads. I took a trip to the local electronics store and bought a set of torx screw drivers. Once I had the keyboard disassembled, the cause of the connection issue was quite clear: a couple of the pins hadn’t been properly soldered!

{{< figure src="20240721-connector-1024x751.jpg" >}}

After quickly touching up the dodgy joints, I had a reliable connection and reassembled the keyboard.

I was still keen to use a proper Miryoku layout implementation with this keyboard. The Vial firmware is actually very nice, allowing you to play with keymaps on the fly without constantly reflashing the keyboard, but the build supplied with the keyboard only supported 6 layers, whereas Miryoku requires 10 layers for a full implementation. I looked into how to rebuild the Vial firmware so it would support the required 10 layers. The Vial repo only seemed to support V1 of the Corne keyboard, based on the Pro Micro controller, whereas v4 seems to be based around a raspberry pi chip. This probably meant that I couldn’t use the Vial repo directly.

The keyboard vendor had provided a link to the github repo with the firmware for this keyboard. I forked the repository and followed the instructions to see if I could get it to build. Digging into the Makefile, I saw that the vanilla QMK and Vial repositories would be downloaded, and then some other files would be copied over to support the V4 keyboards. I found the place where the number of layers was hard-coded, increased it to 10, and tried to build.

Unfortunately, I saw that there were separate files for rev 4.0 and rev 4.1 boards. Which one did I have? After checking with some people on Discord, it seemed that I would have to disassemble the keyboard again so I could check what revision number was printed on the PCB. It turned out I had a rev 4.0 board, so I built the software and flashed it. Et voilà, I now had a firmware supporting 10 layers!

I went through the Miryoku source code to understand all the features used for the layout, and replicated them in my keyboard using the Vial GUI. You can find my tweaked firmware plus Miryoku layout file in [GitHub](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb). I made a couple of tweaks to the layout to improve the user experience when typing foreign languages such as Esperanto and French.

{{< admonition type="note" title="Note" >}}
I've since learned that one important difference between rev 4.0 and 4.1 is that r4.0 uses a ''TRRS'' (tip-ring-ring-sleeve) full duplex interconnecting cable, whereas r4.1 uses a ''TRS'' half-duplex interconnecting cable. I believe this change was made due to a shortage of TRRS sockets on the market, and it explains why my 4.1 TRS board couldn't communicate properly when flashed with 4.0 TRRS firmware.
{{< /admonition >}}
