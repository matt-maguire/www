+++
title = "Ergonomia Emacs"
featuredImage = "20250329-hyper-meh-keys.jpg"
date = 2025-03-29
lastmod = 2026-11-16T11:39:26+11:00
tags = ["Keyboards", "AltKeyboardLayouts", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Lastatempe mi demandis min, ĉu mi devus forlasi Doom Emacs/Evil Mode kaj provi malpli opinionatan agordon bazitan sur pli tradiciaj klavkombindoj. Mi serĉis ian "komenca ilaro", kiu provizas bazajn funkciojn, kiujn mi kutimis havi en Doom Emacs. Kelkaj el la opcioj, kiujn mi rigardis, estis:

-   La [Ekzempla Agorda Dosiero](http://xahlee.info/emacs/emacs/emacs_sample_init.html) de Xah Lee el lia reta lernilo. Tiu nomo aperis multfoje dum miaj esploroj pri Emacs, kaj mi kredas, ke li ankaŭ havas grandan kolekton de YouTube-videoj pri la temo. La agordo estas tre baza kaj ne enhavas pakojn. Mi bezonus iom da tempo por mem implementi funkciojn kiel sintaksa emfazo kaj kompletigo — tre instrue, sed pli granda tempoinvesto ol mi nun povas alordigi.

    Interese, Xah havas multon da informoj pri la ergonomieco de klavkombindaj skemoj kaj pri tio, kial Vim-aj klavkombindoj ŝajnas tiom popularaj. Mi ankoraŭ ne legis ĉiujn la artikolojn tie, sed unu, kiun mi trovis interesa, estis [Kial Emacs-aj Klavoj estas Dolorigaj](http://xahlee.info/emacs/emacs/emacs_kb_shortcuts_pain.html). Tiu artikolo priparolas normajn QWERTY-klavarojn, ne la ergonomiajn disigitajn klavarojn, kiujn mi uzas lastatempe. Tamen ĝi igis min ekpensi pri klavkombinda dezajnado kaj pri kiel optimumi mian Emacs-sperton.
-   La [Racia Emacs-Agordo](https://github.com/purcell/emacs.d) de Steve Purcell. Tiu estas pli orientita al retprogramado kaj Mac OS X (kiun mi plejparte forlasis). Ĝi ne uzas la makroon `use-package`, pri kiu mi ŝatus lerni pli. Tamen ĝi aspektas kiel reala kandidato -- mi nur konvertus ĝin al org-reĝima literatura-programada stilo.
-   La agordo [crafted-emacs](https://github.com/SystemCrafters/crafted-emacs) de SystemCrafter. David Wilson kaj lia komunumo [System Crafters](https://systemcrafters.net/) estas bonega rimedo pri ampleksa gamo da teknikaj temoj, inkluzive de Emacs. La tiea agordo emas antaŭrangigi enkonstruitajn Emacs-funkciojn super la pakoj, kiujn la komunumo vaste uzas. Tio signifas, ke vi lernas multe pri enkonstruitaj Emacs-funkcioj kaj Elisp, sed ankaŭ ke la transiro de io kiel Doom Emacs povus esti pli interromoplena.
-   La agordo [minimal-emacs.d](https://github.com/jamescherti/minimal-emacs.d) de James Cherti -- kiel Doom Emacs, ĝi estas optimumigita por rapida lanĉo kaj aspektas kiel fortika kandidato. Ĝi montras al vi, kiun kodon enmeti por ebligi diversajn funkciojn, kaj al mi plaĉas ĝia uzo de la makroo `use-package`. La ĉefa bariero ĉi tie estus eltrovi, kiel plej bone integri ĝin en mian org-reĝiman agordan aron. Mi volas esplori ĉi tiun pli profunde.
-   La "distro"-agordo [Prelude Emacs](https://prelude.emacsredux.com/en/latest/). Ĝi estas glate prezentita kun bela uzanta manlibro, kaj ĝi kovras funkciojn gravajn por mi, kiel Org-reĝimo kaj \\(\\\LaTeX\\) subteno. Alio tre interesa ĉe ĉi tiu agordo estas, ke aldone al la normaj `Ctrl`-bazitaj klavkombindoj, ĝi ankaŭ enkondukas pli mallongajn alternativajn kombindojn kun la `Super`-klavo. Tio povus faciligi la transiron de Doom Emacs kun ĝiaj "Spacemacs"-stilaj spacoklavaj ŝparvojoj. Unu zorgo povus esti konfliktoj kun la klavkombindoj de mia fenestra administranto Qtile -- mi povus pripensi uzi anstataŭe la Meh-klavon por Qtile. La alia zorgo estas, ĉu saltado al alia glata distro kiel ĉi tiu vere donus al mi iun avantaĝon super restado ĉe Doom Emacs. Tamen mi volas esplori la ergonomie flankon de la klavkombindoj.

Do, estas kelkaj opcioj, kiujn mi povas esplori, precipe la lastajn du.

Dum mi pripensis ergonomiecon kaj kiel ĝi povus rilati al miaj disigitaj klavaroj, mi ricevis bultenon de la klavaro-fabrikanto ZSA. Ĝi enhavis artikolon pri nova QMK-funkcio, kiun ili subtenas, nomata [Chordal Hold](https://blog.zsa.io/chordal-hold/?mc_cid=c9a680e0af). Tiu optimumigo permesas pli fidindan funkciadon de hejmvicaj modifiloj kaj tavolŝanĝiloj, kiujn mi amplekse uzas. Mi konektis al la ZSA-reta agordilo por ebligi la novan funkcion, kaj mi rimarkis, ke mi jam estis agordinta modifilan klavon `Meh` sur mia spacoklavo. Mi estis forgesinta pri ĝin, ĉar mi ne vere uzas ĝin kun mia Linux-agordo. Ĵus leginte pri la Super-klavaj kombindoj de Prelude kaj la artikoloj de Xah Lee pri la historio de klavaroj kaj la klavo `Hyper`, mi ekpensis, ĉu mi devus pli uzi tiajn klavojn.

Do, mi faris kelkajn ĝustigojn de frapi-kaj-teni al mia agordo, ebligis la funkciojn Chordal Hold kaj Permissive Hold priskribitajn en la ZSA-bloga artikolo, kaj agordis klavojn `Hyper` kaj kroman modifilon `Meh`, kiuj bone funkcias kun Chordal Hold, preparante iom da eksperimentado. Vi povas trovi mian nunan klavaran agordon kun tiuj ŝanĝoj [tie](https://configure.zsa.io/voyager/layouts/34PvY/latest/0).

Ĉar mi iom malthavas tempon nun, mi restos ĉe Doom Emacs dummomenta kaj elsitos mian cerbon en ĉi tiun blogan afiŝon, por ke mi povu daŭrigi la esploron iom poste, kiam mi havos pli da tempo.
