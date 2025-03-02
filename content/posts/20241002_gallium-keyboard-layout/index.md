+++
title = "Gallium Keyboard Layout"
featuredImage = "20241002-gallium-1536x678.png"
date = 2024-10-02
lastmod = 2025-03-02T17:32:41+11:00
tags = ["Computers", "Keyboards", "AltKeyboardLayouts"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

I’ve been using the ISRT keyboard layout for a while now, and it is becoming more intuitive to type on. However, I’ve been hearing a lot about [Graphite](https://github.com/rdavison/graphite-layout) and [Gallium](https://github.com/GalileoBlues/Gallium) on the Alt Keyboard Layout forums, which has made me a little curious. So, I set up a keymap to see what all the fuss is about.

<!--more-->

Graphite and Gallium are very similar layouts at their core. They both put all the vowels in a block on the right hand with an “OA” stack on the middle finger. They then put the letter “H” on the vowel hand on the index finger, and the letter “N” on the pinky finger of the consonant hand. Since, in English, the letter “N” is typically preceded by a vowel and followed by a consonant, and the letter “H” is typically preceeded by consonant and followed by a vowel, this encourages a high alternation between hands with a left-to-right rolling tendency that results in very low redirects (where the rolling pattern in one hand changes direction mid-word). Redirects are a known weakness of the ISRT and other Colemak-like layouts, so I am very interested to compare them against the NRTS-HAEI family of layouts like Gallium and Graphite.

On doing some reading, it seems that Graphite is better optimised for traditional row-staggered keyboards whereas Gallium may be better suited to column-staggered keyboards like my Voyager and Corne split keyboards. I also saw some remarks that Gallium may be more “Vim-friendly”. However, the two layouts are actually fairly similar, differing mostly in the punctuation and index-finger keys. Based on this, I chose to explore the Gallium layout.

It turns out that there are actually two versions of Gallium: v1 and v2. The latter one mainly seems to be tweaked to take advantage of the reduced distance between the homerow inner column of the right hand index finger and the “OU” on the top row on a row-staggered keyboard. Since I mainly plan to use col-stag keyboards, I chose to go with v1:

```text
b l d c v   j y o u , -
n r t s g   p h a e i ;
q x m w z   k f ' / .
```

There are a few departures from the published Gallium v1 layout:

-   I swapped the semicolon (;) and forward slash (/). This is because I wanted the layout to work on my Corne Mini, which lacks an outer pinky column. Putting the slash on the base layer makes it easier to type filenames and web addresses and, more importantly, gives access to the question mark (?) without having to dive into a layer.
-   I considered swapping the “C” and “W” keys, which someone recommended to make the layout even more Vim-friendly. It would also move the potentially destructive “W” shortcut off the bottom row, but for now I think I’ll stick with the standard arrangement until I get more experience with the layout.
-   I did decide to swap the “X” and “Q” keys. The order of those two keys has very little effect on the performance of the layout, but when implementing the layout on a row-staggered keyboard using an angle mod, it makes it easier to use “Ctrl-X” in Emacs, and also makes it harder to accidentally type the destructive “Ctrl-Q” shortcut.

Here is the row-staggered angle-modded Gallium layout that I implemented on my Macbook keyboard using [Karabiner Elements](https://karabiner-elements.pqrs.org/) (the karabiner.json config file is in my [github repo](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb)):

```text
` 1 2 3 4 5 6 7 8 9 0 = [
   b l d c v j y o u , - ] \
    n r t s g p h a e i ;
     x m w z q k f ' / .
```

I also created a [layout for my ZSA Voyager](https://configure.zsa.io/voyager/layouts/KWgaz/94W5A/0) using their web configuration tool.

{{< figure src="20241002-Screenshot_2024-10-02_17-33-57-1024x589.png" >}}

Before I had the “Shift” modifier on a left thumb key and the “Control” modifier on a right thumb key. It is common at the end of a sentence to have a space followed by a capital letter. By having “Space” and “Shift” together on the left thumb cluster, this creates an awkward same-finger motion on the left thumb, so I moved the “Shift” key to the right thumb cluster.

I’m going to spend a little time building some fluency with this new layout to see if it is really worthwhile compared to ISRT, and will keep you updated here on what I discover.
