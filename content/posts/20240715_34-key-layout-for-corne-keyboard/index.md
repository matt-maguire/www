+++
title = "34 Key Layout for Corne Keyboard"
date = 2024-07-15
lastmod = 2024-12-26T07:19:00+11:00
tags = ["keyboards", "corne"]
categories = ["Blog"]
draft = false
weight = 2008
+++

I’ve been experimenting a bit more with the Corne keyboard. I saw a [video from Ben Vallack](https://youtu.be/8wZ8FRwOzhU?si=Y1a1JSbGsN1vZizU) where he lays out a mapping for his 34-key keyboard.

In this video, he explains how holding down layer and modifier keys can cause fatigue, which he avoids by using “sticky” layer keys and “one-shot” modifiers. In the layout I am currently using, I use a similar layer scheme to Ben in which the shift key is moved to the left thumb. This is the layout I am currently working with:

{{< figure src="20240715-keymap34.png" >}}

The corresponding Vial layout file can be found in [my GitHub](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb).

Ben’s original layout was optimised for his workflows on the Mac. While I also use a Mac, I am not so familiar with some of the shortcuts he uses. He is a Vim user, whereas these days I am spending more time in Emacs. He also seems to use Apples exclusively, while I am switching frequently between Mac and Linux. The biggest headache with all this switching is the handling of modifier keys such as Ctrl and Command keys. One potential solution for this is to remap modifier keys on my Linux box to make the two systems work in a more similar way. I found the [following tool](https://github.com/rbreaves/kinto) which may (or may not) help with this.

I’m going to play around with my usual workflows on Mac and Linux to see what shortcomings there are with this layout and what improvements I might make. Some tweaks I’ve already made are:

-   re-introduced home key modifiers in the base layer. Because I am uncertain which modifiers I will be needing, I prefer to have too many rather than too few to start with. I’ve also put modifiers in a consistent way on the thumb keys — we’ll see if they get in the way of the layer toggle keys…
-   I have added a couple of tap-dance shortcuts to the base layer to make it easier to type some languages requiring special characters such as é, à, ü, ß, ĉ, ŝ, ĵ, ŭ, etc..
-   I rearranged the symbols in a similar way to a number pad to make it easier to remember. Different styles of brackets are paired on the left hand.
-   I added function keys (eg. F1, F2, …) to the numeric keypad layer, as well as modifiers and some punctuation characters that I think may come in handy when entering numbers.
-   I moved the audio/media player functions off the numeric layer onto a separate “adjustments” layer, together with some keys concerning RGB lighting.

{{< admonition type="note" title="Note" >}}
I put some backspace and delete shortcuts on the outer thumb keys for convenience; they are not actually needed, and the layout can be used just fine with only two thumb keys.
{{< /admonition >}}

As I use the keyboard more, I’ll get a better idea what else I will need to add as well as what I can strip away. I’ll post more updates here as I learn more.
