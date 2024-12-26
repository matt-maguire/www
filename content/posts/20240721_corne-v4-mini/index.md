+++
title = "Corne V4.1 Mini"
date = 2024-07-21
lastmod = 2024-12-26T17:56:42+11:00
tags = ["keyboards", "corne"]
categories = ["Blog"]
draft = false
weight = 2006
+++

I’ve had so much fun with my Corne V4 keyboard, I decided to take advantage of the sale the vendor had going, and order a second one! My idea was to keep the 46-key Corne at home connected to my Linux workstation, and acquire a 40-key Corne V4 mini to carry around with my laptop. I ordered the same choc brown switches as before, but instead of black keycaps I ordered white keycaps. This would allow me to swap the different coloured keycaps in order to better highlight the home keys. And here it is:

{{< figure src="20240721-corne_v4_mini-1024x365.jpg" >}}

It’s a lovely compact setup. I was finding the outer columns of keys on the 46-key model to be distracting, and so I tried removing the outer columns of switches. I then ran into a problem where Vial requires you to hold down two switches on the top far left to un.ock the keyboard — oops, I had removed the top left-most switch! Fortunately the firmware allows you to configure which keys to use for unlocking at build time, so I moved the security key setting inwards to avoid the far outer column.

I reflashed the left hand side of the mini with no problem to open up 10 layers in Vial. When I tried to flash the right hand side though, I had a problem — one of the keys needed to unlock the keyboard doesn’t exist on the mini — it was on the outer column that gets broken away from the PCB when building the mini. How to get the right hand half into bootloader mode if one of the unlock keys doesn’t exist?

I pulled the keyboard apart with my newly acquired Torx screwdriver set, and found some switches on the PCB that could be used to get the board into bootloader mode. A double-tap on the reset switch did the trick. I reflashed the right-hand half after tweaking the build to map the unlock keys onto keys that actually exist. This went through ok, so I put the keyboard back together and used Vial’s matrix tester to verify that all keys were working.

What I found was that only the half that was connected to the USB cable would register any key presses. The other half would not show any RGB lights nor register any key presses. Oh dear, what to do?

When I built the firmware, I assumed that it would be rev 4.0 like with the previous keyboard I had purchased from this vendor. However, I hadn’t checked to confirm this. Once again, I disassembled the board, and discovered that the mini was made from rev 4.1 boards! I built some rev 4.1 firmware and reflashed both halves, and the keyboard came back to life with full communication between the two halves. Phew, what a relief!

It has been lots of fun playing with these keyboards, and I have learned a lot. The Vial layouts I set up on the 46-key Corne can be loaded directly onto the 40-key mini, and it maps the correct keycodes onto the appropriate keys (and the two outer columns from the 46-key keyboard are silently dropped).

I have loaded the full miryoku layout into the left-hand side of both keyboards, and the Ben Vallack “sticky layer” adapted layout in the right hand side of the keyboards. By connecting either the left-hand side or the right-hand side to the computer, I can choose which layout I want to use. At the moment, I think I am finding the miryoku layout a bit easier to navigate. The other layout requires a bit more awareness of which layer you are in, and because I am still battling to learn the Colemak layout on both setups, this just adds to the cognitive load. I think I’ll stick with the miryoku layout for now until I get more comfortable, and then I’ll revisit the other layout once I have more experience.

You can find the firmware tweaks and keyboard layouts I am using in my [GitHub repository](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb) (“custom” branch has my changes; “main” branch tracks the vanilla upstream code).
