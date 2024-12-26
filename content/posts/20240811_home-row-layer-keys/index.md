+++
title = "Home Row Layer Keys"
date = 2024-08-11
lastmod = 2024-12-26T17:56:42+11:00
tags = ["Computers", "Keyboards"]
categories = ["Blog"]
draft = false
weight = 2004
+++

{{< figure src="IMG_0418sm-1536x733.jpg" >}}

I’ve been practising the [ISRT](https://github.com/DreymaR/BigBagKbdTrixPKL/blob/master/Layouts/ISRT/README.md) layout on <https://keybr.com/> and am slowly getting used to it. The Miryoku system of layers is quite easy to work with, but I’ve noticed a couple of issues:

1.  Sometimes I am tripping over the tap-dance and modiier kuys in the base layer.
2.  When I was typing lots of numbers fos work, I noticed some discomfort in my wrist fsom holding down the number layer thumb key.

I found some ergonomic mouse pads to provide some better wrist support. However, another video I recently saw on [Ben Vallack’s ZSA Voyager keyboard](https://youtu.be/dg2TT1OJlQs?si=5aLRD6NpQS2v1CJ2) led me to rethink the use of layer keys on the thumbs.

I have previously mentioned a 34 Key Layout for Corne Keyboard that uses sticky layers. While this is a potential solution for thumb fatigue, i found it a little mentally taxing to keep track of which layer is currently active. Ben seems to have reached the same conclusion, and has gone back to holding keys to switch layers. Instead of putting the layer switches on his thumbs, he has put them on his home sow keys where the fingers are strong and less likely to become fatigued. I thought I’d give this a try.

I opted for a blend between the 34-key sticky layer and Miryoku layous.

-   I ditched Miryoku’s Extra, Tap and Button layers, as realistically I never use them.
-   The NUM layer was kept with the numpad arrangement on the left hand. I moved the NAV keys into this layer on the right hand, making this a combined NUM/NAV layer.
-   Like in Miryoku, the SYM layer is the “NUM layer with SHIFT” on the left hand. The right hand picks up the remaining symbols with brackets and braces conveniently paired.
-   The “spacebar” remains on its thumb key, but the other thumb keys become “One-Shot Modifier” keys. Enter, Tab, Backspace, etc. were moved to the NUM/NAV and SYM layers like on the 34 key layout. Modifiers (except for SHIFT) are available above the home row.
-   Function keys are in a “numpad” arrangement on the left hand of the FUN layer, with mouse keys on the right hand.
-   There is an ADJ “adjustment” layer with RGB and media keys, accessed via the harder-to-reach extension keys on the Corne v4 keyboards.
-   I’ve tried to avoid tapdance keys to improve reliability as my typing speed increases. Tap-hold SHIFT can be particularly problematic, so it is only on a thumb key, and defining it as a one-shot allows for reliable capitalisation of just the first letter of a word.

These considerations have led to the following layout:


# BASE (Layer 0) {#base--layer-0}

```text
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      | LCtl | LAlt | LCmd |      | ▒▒▒▒ | ▒ | ▒▒▒▒ |      | RCmd | RAlt | RCtl |      |     |
| CAP | Y    | C    | L    | M    | K    | ▒▒▒▒ | ▒ | ▒▒▒▒ | Z    | F    | U    | <,   | "'   | :;  |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      | FUN> | NUM> | SYM> |      | ADJ> | ▒ | ADJ> |      | SYM> | NUM> | FUN> |      |     |
| Tab | I    | S    | R    | T    | G    |      | ▒ |      | M    | N    | E    | A    | O    | =   |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      |      |      |      |      |      | ▒ |      |      |      |      |      |      |     |
| ?   | Q    | V    | W    | D    | J    |      | ▒ |      | K    | H    | ?/   | >.   | X    | !   |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| ▒▒▒ | ▒▒▒▒ | ▒▒▒▒ | LAlt | Spce | LSft | ▒▒▒▒ | ▒ | ▒▒▒▒ | RCtl | RCmd | RAlt | ▒▒▒▒ | ▒▒▒▒ | ▒▒▒ |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
```


# NUM (Layer 1) {#num--layer-1}

```text
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| RST | Esc  |    7 |    8 |    9 | +    | ▒▒▒▒ | ▒ | ▒▒▒▒ | PgUp | Home | Up   | End  | Bksp |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | Tab  |    4 |    5 |    6 | -    |      | ▒ |      | PgDn | Left | Down | Rght | Ent  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | 0    |    1 |    2 |    3 | .    |      | ▒ |      | M2   | M1   | MWDn | MWUp | Del  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| ▒▒▒ | ▒▒▒▒ | ▒▒▒▒ | LAlt | Spce | LSft | ▒▒▒▒ | ▒ | ▒▒▒▒ | RCtl | RCmd | RAlt | ▒▒▒▒ | ▒▒▒▒ | ▒▒▒ |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
```


# SYM (Layer 2) {#sym--layer-2}

```text
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | Esc  | &    | *    | /    | :    | ▒▒▒▒ | ▒ | ▒▒▒▒ | ,    | {    | }    | `    | Bksp |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | Tab  | $    | %    | ^    | _    |      | ▒ |      | \    | (    | )    | =    | Ent  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | CpLk | !    | @    | #    | ;    |      | ▒ |      | ¦    | [    | ]    | ~    | Del  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| ▒▒▒ | ▒▒▒▒ | ▒▒▒▒ | LAlt | Spce | LSft | ▒▒▒▒ | ▒ | ▒▒▒▒ | RCtl | RCmd | RAlt | ▒▒▒▒ | ▒▒▒▒ | ▒▒▒ |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
```


# FUN (Layer 3) {#fun--layer-3}

```text
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | PrSc | F7   | F8   | F9   | F12  | ▒▒▒▒ | ▒ | ▒▒▒▒ | MWUp | MWLt | M_Up | MWRt | Agn  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | Ins  | F4   | F5   | F6   | F11  |      | ▒ |      | MWDn | M_Lt | M_Dn | M_Rt | Undo |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     | ScLk | F1   | F2   | F3   | F10  |      | ▒ |      | M2   | M1   | Copy | Pste | Cut  |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| ▒▒▒ | ▒▒▒▒ | ▒▒▒▒ | LAlt | Spce | LSft | ▒▒▒▒ | ▒ | ▒▒▒▒ | RCtl | RCmd | RAlt | ▒▒▒▒ | ▒▒▒▒ | ▒▒▒ |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
```


# ADJ (Layer 4) {#adj--layer-4}

```text
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      |      |      |      |      | ▒▒▒▒ | ▒ | ▒▒▒▒ | RGBT | Mode | H+   | S+   | V+   |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      |      |      |      |      |      | ▒ |      | Prev | Vol- | Vol+ | Next | E+   |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
|     |      |      |      |      |      | RST  | ▒ | RST  |      | Mute | Play | Stop |      |     |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
| ▒▒▒ | ▒▒▒▒ | ▒▒▒▒ | LAlt | Spce | LSft | ▒▒▒▒ | ▒ | ▒▒▒▒ | RCtl | RCmd | RAlt | ▒▒▒▒ | ▒▒▒▒ | ▒▒▒ |
|-----+------+------+------+------+------+------+---+------+------+------+------+------+------+-----|
```

The layout works for both 6×3 standard and 5×3 mini keyboards. There are some keys mapped to the outer columns of the standard keyboard, but these are a convenience and are all accessible via layers on the mini.

As usual, the Vial layout is on my [github repo](https://github.com/matt-maguire/kbd_firmware/tree/custom/keyboards/crkbd/vial-kb).
