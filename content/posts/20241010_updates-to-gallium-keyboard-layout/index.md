+++
title = "Updates to Gallium Keyboard Layout"
featuredImage = "20241010-Screenshot_2024-10-10_18-16-04-1024x234.png"
date = 2024-10-10
lastmod = 2025-01-23T18:19:59+11:00
tags = ["Computers", "Keyboards", "AltKeyboardLayouts", "MechKeyboard", "SplitKeyboard", "Gallium", "Graphite", "ISRT", "Vial"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

I’ve been practising on the Gallium layout that I mentioned [last time]({{< relref "20241002_gallium-keyboard-layout" >}}), and while I am still fighting the muscle memory that I built up with the ISRT layout, I am gradually getting used to it and am currently at around the 25 wpm mark (similar to my maximum speed in Morse Code).

<!--more-->

Today I noticed that Ben Vallack has released [another video](https://youtu.be/DKQ4pOoFh5I?si=0OaoCPKk2vhdDGT7) on keyboard layouts, and I was interested to learn that he has moved on from the ISRT layout, instead adopting the [Graphite layout](https://github.com/rdavison/graphite-layout). That layout is actually very similar to the Gallium layout that I have been learning. It has the same NRTS-HAEI home keys, making it a high-alternating layout like Gallium. He said that as his speed increased on the ISRT layout, he noticed that some of the scissors and same-finger skipgrams were starting to bother him (see the [Alt Keyboard Layout Guide](https://bit.ly/layout-doc-v2) for definitions of these terms). While my own typing speed is still too low to encounter these problems, it is nice to have some external validation for my decision to move from ISRT to [Gallium](https://github.com/GalileoBlues/Gallium).

One interesting thing about Graphite is the punctuation — the shifted form of some of the punctuation keys differs from the usual setup. When I looked into Graphite earlier, I investigated how one might implement that on my various keyboards.

For my Corne keyboards which run the Vial QMK-based firmware, it is actually pretty simple. QMK has a feature called \`\`[Key Overrides](https://docs.qmk.fm/features/key_overrides)'' which allow you to remap the outputted scan code of a shifted key combo to whatever you want, and Vial exposes this feature in its configuration GUI. Similarly, it is trivial to remap keycodes for shifted keys in OS-based software like [Karabiner Elements](https://karabiner-elements.pqrs.org/) (for macOS) or [KMonad](https://github.com/kmonad/kmonad) (multi-platform).

For the ZSA Voyager keyboard however, the story is a bit more complicated. The Key Override feature is not exposed in ZSA’s Oryx web configuration tool. This means that if you want to implement Graphite’s punctuation mappings using Oryx, you would probably need to use some awkward setup involving layers for shifted characters. You could of course program a keymap in QMK that uses key overrides and flash that firmware directly to the Voyager, but you would then forego the benefits of using Oryx to tweak and share your layout with others. I could see from Ben Vallack’s video that he had implemented his Graphite layout using Oryx, and so I was very interested to see how he had managed to navigate this problem.

A quick search in the Oryx tool turned up his [Graphite configuration](https://configure.zsa.io/voyager/layouts/XgZ46/latest/0). As you can see, he didn’t bother to fully implement Graphite’s punctuation setup at all. I shouldn’t have been too surprised by this; Graphite was really designed for standard row-staggered keyboards without layers, and so of course an experienced alt keyboard layout hacker like Ben would be very comfortable using layers for his symbols like in his preceding layouts. It was nevertheless illuminating to study the layout of his base layer to see if there were any optimisations I might want to consider for my own [Gallium-based layout](https://configure.zsa.io/voyager/layouts/KWgaz/latest/0).

Some other discoveries I made recently from Ben’s new video and from other sources:

-   ZSA have released a new typing training tool outside the one they have on their Oryx page. The new tool works with non-ZSA keyboards, and is somewhat reminiscent of MonkeyType. You can find the new tool at the easy-to-remember URL: <https://typ.ing/>
-   If you want to get a “feel” for a new layout without going to the full effort of learning it, there is a web tool you can use to translate a target text into the equivalent letters you would need to press on your existing layout. The tool can be found at: <https://keyboard-layout-try-out.pages.dev/>
-   The Gallium layout has just been released in DreymaR’s [EPKL key mapping tool](https://github.com/DreymaR/BigBagKbdTrixPKL/tree/master/Layouts/Gallium). This tool is Windows-based, which means that I have very little use for it myself. One of the cool features it has though is the Extend layer which converts the mostly useless CAPSLOCK key into a layer-switching key. This is such a useful feature, which I have implemented on my macbook using [Karabiner Elements](https://karabiner-elements.pqrs.org/). The config file I use is available in [my github](https://github.com/matt-maguire/kbd_firmware/blob/custom/keyboards/crkbd/vial-kb/karabiner.json).
