+++
title = "Ergonomic Emacs"
featuredImage = "20250329-hyper-meh-keys.jpg"
date = 2025-03-29
lastmod = 2025-03-29T13:42:29+11:00
tags = ["Keyboards", "AltKeyboardLayouts", "Emacs"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

Recently I was wondering whether I should move on from Doom Emacs/Evil Mode and try a less opinionated setup based on more traditional key mappings. I was looking around for some sort of "starter kit" that provides some basic features that I have grown accustomed to using Doom Emacs. Some of the options I looked at were:

-   Xah Lee's [Sample Init File](http://xahlee.info/emacs/emacs/emacs_sample_init.html) from his online tutorial. This is a name I have come across a number of times as I research Emacs, and I believe he also has a large collection of YouTube videos on the topic. The config is very basic, and doesn't include any packages. It would take some time for me to work out how to implement features such as syntax highlighting, completion, etc. which, while very educational, is a bigger time commitment than I can currently afford.

    Interestingly, Xah has a lot of information about the ergonomics of Key Binding schemes and why Vim keybindings seem so popular. I haven't yet read all the articles there, but one I found interesting was [Why Emacs Keys are Painful](http://xahlee.info/emacs/emacs/emacs_kb_shortcuts_pain.html). This article discusses standard QWERTY keyboards rather than the ergonomic split keyboards that I have been using of late. It however got me to start thinking about keybinding design and how to optimise my Emacs experience.
-   Steve Purcell's [Reasonable Emacs Config](https://github.com/purcell/emacs.d). This one is oriented more towards web development and Mac OS X (which is something I have mostly moved away from). It doesn't use the `use-package` macro, which is something I'd like to learn more about. However, it looks like a viable candidate -- I would want to convert it to a org-mode literate programming style though.
-   SystemCrafter's [crafted-emacs](https://github.com/SystemCrafters/crafted-emacs) config. David Wilson and his [System Crafters](https://systemcrafters.net/) community is a great resource on a range of technical topics including Emacs. The config proposed there tends to prioritise built-in Emacs functionality over packages that are widely used in the community. This means you learn a lot about built-in Emacs features and Elisp, but it also means that it might be more disruptive when moving from something like Doom Emacs.
-   James Cherti's [minimal-emacs.d](https://github.com/jamescherti/minimal-emacs.d) setup -- like Doom Emacs, this is optimised for fast start-up, and looks like a strong contender. It tells you which code to insert to enable various features, and I like its use of the `use-package` macro. The main barrier here would be working out how to best integrate it with an Org-Mode configutation setup. I'm keen to explore this one further.
-   The [Prelude Emacs](https://prelude.emacsredux.com/en/latest/) "distro" config. This is slickly presented with a nice user manual, and covers features that are important to me such as Org Mode and \\(\LaTeX\\) support. The other thing very interesting about this setup is that, in addition to the standard `Ctrl` based keybindings, it also introduces shorter alternative bindings with the `Super` key. This may ease the transition from Doom Emacs with its "Spacemacs"-style spacebar shortcuts. One concern may be conflicts with my Qtile window manager keybindings -- I could look into using the Meh key instead for Qtile. The other concern is whether hopping over to another polished distro like this would really give me any benefit over sticking with Doom Emacs. I want to explore the ergonomic keybinding side of setup however.

So, there are a number of options I can explore, especially the last two.

While I have been thinking about ergonomics and how it may relate to my split keyboards, I received a newsletter from the keyboard manufacturer ZSA. It included an article about a new QMK feature they support called [Chordal Hold](https://blog.zsa.io/chordal-hold/?mc_cid=c9a680e0af). It is an optimisation that allows for more reliable operation of home-row modifiers and layer switchers, of which I make extensive use. I connected to the ZSA web configurator in order to enable the new feature, and I noticed that I already had configured a `Meh` modifier key on my spacebar. I had forgotten about it, since I don't really use it with my Linux setup. Having just read about the Prelude super key bindings and Xah Lees articles about the history of keyboards and the `Hyper` key, I started to think whether I should be making more use of such keys.

So, I made some tap-and-hold tweaks to my config, enabled the Chordal Hold and Permissive Hold features described in the ZSA blog article, and set up `Hyper` key and additional `Meh` key modifiers that would play nicely with the Chordal Hold feature in preparation for some experimentation. You can find my current keyboard setup with those changes [here](https://configure.zsa.io/voyager/layouts/34PvY/latest/0).

As I am a little time-poor at the moment, I will stick with Doom Emacs for the moment and dump out my brain into this blog post so I can pick up the exploration a bit later when I have more time.
