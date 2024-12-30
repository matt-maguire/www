+++
title = "ISRT Keyboard Layout"
featuredImage = "20240803-isrt-anglemod.en_.ansi_-1024x312.jpg"
date = 2024-08-03
lastmod = 2024-12-30T13:56:16+11:00
tags = ["Computers", "Keyboards", "Corne", "AltKeyboardLayouts"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

I have been training on the Colemak-DH layout with my Corne keyboards, and I am getting used to it, even though my typing speed is still slow. I found another [video from Ben Vallack](https://youtu.be/dg2TT1OJlQs?si=ZiAzPIMkbCAMG02X) about the [ZSA Voyager](https://www.zsa.io/voyager) keyboard, and looked into his layout on that keyboard. He seems to have dropped the idea of layer toggles due to the increased cognitive load of keeping track of which layer you are in. Instead, he now holds his home row keys to select layers. I might look into this approach at some point, but for now I am quite happy with the Miryoku setup.

I did notice that he is still using the ISRT layout even after so many iterations of his setup. Some of the Colemak-DH sequences involving my ring and pinkie fingers are not feeling so great, so I thought I’d look a bit into this ISRT setup before Colemak-DH becomes too ingrained.


## ISRT Layout {#isrt-layout}

The original creator of the ISRT layout is no longer promoting it, and has taken down his web page. However, thanks to the magic of Wayback Machine, a copy has been archived [here](https://web.archive.org/web/20230203194545/https://notgate.github.io/layout/).

The layout that he finally settled on is as follows:

{{< figure src="20240803-Zilfkpz.png" >}}

This is for an ortholinear (matrix) keyboard (like my Cornes). He also proposed some mappings onto an ANSI keyboard, with and without angle mods:

```text
Ortholinear (Matrix) Keyboards:
y c l m k    z f u , '
i s r t g    p n e a o ;
q v w d j    b h / . x
```

```text
ANSI Keyboard (no angle mod):
y c l m k z f u , '
 i s r t g p n e a o ;
  q v w d j b h / . x
```

```text
ANSI Keyboard (angle mod):
y c l m k z f u , '
 i s r t g p n e a o ;
  v w d j q b h / . x
```

The angle mod version makes it more comfortable on a staggered keyboard, but due to the limitations of the ANSI keys the “Q” is moved from the far left to the middle of the bottom row of the keyboard. I also experimented by defining a “wide” mapping:

```text
ANSI Keyboard (wide angle mod):
` 1 2 3 4 5 6 \ 7 8 9 0 =  ⌫
↹  y c l m k [ z f u , ' - ;
⇧⇧  i s r t g ] p n e a o  ⏎
⇧⇧⇧  v w d j q x b h / . ⇧⇧⇧
```

I set up the wide and regular angle mod layouts in Karabiner on my Mac. In the end, I didn’t like the wide angle mod, as you have to move both the X as well as the Q to the centre of the bottom row. If I use ISRT on my Macbook’s keyboard, I’ll just use the regular angle mod version. I imagine though that mostly I’ll be using the Corne keyboard instead.


## Pros and Cons of the Layout {#pros-and-cons-of-the-layout}

So, are there any benefits to the ISRT layout compared to Colemak-DH? A bit of a search on the Internet turned up an interesting [Keyboard Layouts](https://bit.ly/layout-doc-v2) document (that one is second edition; the [first edition](https://docs.google.com/document/d/1_a5Nzbkwyk1o0bvTctZrtgsee9jSP-6I0q3A0_9Mzm0/edit?usp=sharing) is more graphics-heavy). Some other information I found was from a Reddit post and some analysis on <https://keyboard-design.com>.

To summarise, the key points about ISRT are:

the IY column is moved to the consonant (left) hand, and A is moved to where I was. Consequently, rolls increase while redirects decrease (the **YOU** and **ION** trigrams are not redirects anymore).

The drawback is that Y is now on top row pinky, which makes it harder to reach. There is also potential that the alternation of hands that you normally get from putting vowels and consonants on separate hands becomes worse (if you consider that Y is a semi-vowel)

Punctuation no longer causes SFBs (where the same finger used to type two letters in succession), as the right ring finger has “,A.”. (ie. this avoid the SFBs on Colemak where words ending in “Y” are followed by a fullstop and words ending in “E” are followed by a comma).

Movement on the right index is drastically reduced, thanks to using FNHPB over HNLM.

Different ring + middle setup: ring finger is **CSV** and middle finger is **LRW**. Although **LRW** is a high movement column, it allows us to get the letter L off the ring finger.

So, what was it actually like to type on?


## Trying it out {#trying-it-out}

Because I wasn’t sure if I would want to commit to a layout that has effectively been abondoned by its creator, I decided to write the keymap to the right hand side fontrollers of my Corne keyboards. This means that if I plug the left half of the keyboard to my computer’s USB port, I get Colemak-DH, and if I connect the right half to the computer I get ISRT. Because I am using 10 layer Miryoku setup with both layouts, I needed to reflash the Vial firmware on the right hand controller so it could support all 10 layers. I then took my [Vial layout file](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb) for my Colemak-DH setup, copied it to the right hand controller, and used the Vial GUI to remap the letters. I created Vial layouts for both my full size and mini Corne v4 keyboards.

I found it a bit confusing that the A and I keys were swapped, as were the R and S keys. Even with the limited time I have been using Colemak-DH, those letters had already started to find their way into my muscle memory, and as a result my typing speed dropped somewhat.

I have to say though, I do like the feel of this ISRT layout better than Colemak-DH. I don’t feel so much tension in my ring and little finger typing the ION and YOU trigrams. I think this is the layout I am going to move forward with.


## Getting up to speed on ISRT {#getting-up-to-speed-on-isrt}

So, how can I get better at typing on the ISRT layout?

I am currently using two websites to help with this:

-   <https://keybr.com>: this website doesn’t support the ISRT layout. However, since ISRT can be considered to be an optimisation of the Colemak layout, I have left the keyboard layout sent to Colemak-DH. My graphs have all taken a dive, but after one day they are no longer falling, and are slowly starting to climb again. The keyboard layout on the screen shows the “next key” in the wrong place, which can be confusing, and the heatmap is also showing keys in the wrong place, but it still tracks my accuracy wit hthe different keys, and the order of unlocking keys still makes sense even for an ISRT layout.

-   <https://monkeytype.com>: this website DOES support ISRT! I was quite surprised. It is possible to set up a custom wordset, where one of the english language corpuses is filtered to include only letters from the ISRT home row. As my typing improves, I can gradually move letters from the excluded list to the list of allowed letters, and then the site behaves in a similar way to keybr.com (albeit without the per-letter statistics and adaptive letter focus).
