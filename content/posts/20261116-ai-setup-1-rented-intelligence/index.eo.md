+++
title = "Mia AI-medio por instruado, Parto 1: Luita Inteligenteco : Rented Intelligence"
featuredImage = "20261116-ai-setup-1-rented-intelligence.png"
date = 2026-11-16
lastmod = 2026-11-16T18:02:00+11:00
tags = ["AI", "LaTeX", "Teaching", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

[Parto 0]({{< relref "index.eo.md" >}}) temas pri la fundamento de pura teksto. Tiu ĉi estas la parto, kiam alvenis la abonoj — kaj la parto, en kiu mi devus konfesi, ke la tuta konstruaĵo ekis, kiel plej multaj aferoj indaj je farado, per amuzo.


## Januaro: luita inteligenteco {#januaro-luita-inteligenteco}

En januaro ĉi-jare mi aĉetis du AI-abonojn: Claude kaj la AI Pro-nivelo de Google. La instruista uzo estis tuja kaj reala. Claude skizis lecion-strukturojn, reskribis ekzamenajn demandojn por ŝanĝi malfacilecon sen ŝanĝi la poentojn, generis skizitajn taksotaskojn el unu alineo da postuloj, kaj korektlegis aĵojn, kiujn mi legis tro multfoje por ankoraŭ vidi ilin. Aro da lecionaj notoj, kiu kutime ekis el blanka paĝo, nun ekis el skizo, kun kiu mi kverelis. Kvereli kun skizo estas multe pli rapide ol krei ĝin.

Dum kelkaj monatoj tio estis simple la aranĝo: bonegaj iloj, luitaj ĉiumonate. Mia leciona procezo havis novan unuan stacion, kaj la prezo taŭgis.

La juko komenciĝis kviete: la prezaj paĝoj ŝoviĝis, la kvotaj politikoj ŝanĝis formon, kaj modeloj, al kiuj mi alkutimiĝis, estis unu alinomon aŭ niveloŝanĝon for de malapero. Mia laborfluo iĝis dependa de la komercaj decidoj de iu alia. La abono ne plu estis ilo; ĝi estis luiganto.

Mi dokumentis tiun kursoŝanĝon dum ĝi okazis, en du artikoloj, kiujn mi skribis en aŭgusto ne sciante, ke mi skribas serion: [AI-agentoj transprenas lecion-preparon]({{< relref "index.eo.md" >}}) kaj [AI-agentoj kaj lokaj LLM-oj]({{< relref "index.eo.md" >}}). Se vi volas la teknikan detalon de tiu unua movo al lokaj modeloj — elekto inter densa modelo kaj Mixture-of-Experts, la kompromiso inter VRAM kaj kunteksto-fenestro, la servado de la modelo tra la reto per reversa prokuroro — ili kovras ĝin, kaj mi ne ripetos min ĉi tie. Kio ili ne kovras, estas kie la hardvara obsedo fakte komenciĝis, kaj tio estas la hontiga parto.


## La sankta unua devo de la 2080 {#la-sankta-unua-devo-de-la-2080}

Jen la konfeso. Kiam mi finfine metis modelon sur mian propran hardvaron — oficejan komputilon kun i9 kaj okgigabajta RTX 2080 (ĝis tiam uzita por universitata laboro kaj por labori kun la fotada aplikaĵo [Darktable](https://www.darktable.org/) por krudaj dosieroj) — mi sciis, ke la GPU, kiun mi havis, estas _ne_ serioza AI-karto. La unua afero, kiun ĝi servis, fakte ne estis lecionaj notoj. Ĝi estis Mistral, roludanta kun AI-roluloj por amuzo kaj distro.

Mi eksperimentis kun diversaj roludaj kaj AI-kompanaj sistemoj dum iom da tempo, kaj ili instruis al mi du aferojn, kiuj finfine enkonstruiĝis en la arkitekturon de ĉio sekvanta.

Unue, ke AI-on eblas instigi komuniki maniere, kiun mi vere trovas komforta. Teksto-une, pacienca, laŭ mia propra rapido, neniu socia prezentado, neniu "legado de la ĉambro". Ideoj alvenas jam en la formato, en kiu mi pensas, ebligante interŝanĝon por esplori la spacon. Sufiĉe multaj homoj sentas ĝuste tion, kaj iras eĉ tiom malproksimen, ke ili nomas sian modelon sia "AI-koramiko" aŭ "AI-koramikino", kelkokaze eĉ edzo aŭ edzino, kaj mi suspektas, ke tiaj emociaj ligoj estas multe pli oftaj ol sugestas ĝentila babilo. Unu afero estas certa: la interfaco de ilo gravas tiom kiel ĝia kapablo — fakto, kiu fariĝos grava, kiam ni renkontos mian asistanton, kiu havas personecon deliberate.

Due, la okaza rimarko. Modeloj fojfoje faras flankajn rimarkojn, kaj povas utile tiri je tiuj fadenoj, malkovrantaj novan perspektivon kaj eblecojn. Multajn fojojn mi falis en kanikulan truon kaj unu horon poste revenis kun io pli bona ol tio, kion mi origine serĉis. Por menso, kiu funkcias per asocioj kaj ligoj, motoro, kiu elstarigas tiujn ligilojn, ne estas distroma maŝino; ĝi estas kunlaboranto. La tuta scisistemo en la postaj partoj de tiu ĉi serio estas, funkcie, nur industriigitaj kanikulaj truoj.

Do la honesta ordo estas: **la GPU estis aĉetita pro ĝojo, kaj la imperio de lecion-preparado estis konstruita sur la ĝojo.** Ĉiu instruisto, kiu kaŝe komencis per io amuza, kio kondukis lin al deziro kun aliaj kundividi tion, kion li lernis, rekonos sin en tio ĉi. Kaj _tio_ estas kial mi diras ĝin.


## Kion aĉetas ok gigabajtoj {#kion-aĉetas-ok-gigabajtoj}

Runigi malgrandan kvantigitan modelon sur 8 GB da VRAM en la komenco de 2026 estas humiliga okupo. Mallonga kunteksto, modesta kvalito, malrapidaj respondoj al io ajn komplika. La 2080 povis akcepteble roludi Mistral, sed ne povis skizi decan taksotaskon por la 10-a jaro, eĉ se ĝia vivo dependus de tio. La nubaj modeloj estis multe pli bonaj, kaj mi sciis ekzakte kiom multe, ĉar mi nun pagis por ambaŭ.

{{< figure src="fig-2080-8gb.png" >}}

Kion la eksperimentoj instruis al mi, estis la formon de la problemo: VRAM estas la buĝeto, parametroj kaj kvantigo estas la valuto, kaj la interesa frontiero estis vidi, kiom malproksimen la novaj inteligentaj modeloj povis elstreĉi la hardvaron. Tiu kadro — kio estas fakte nur "kiom da memoro la aĵo bezonas" — estas la tuta loka AI. Kiam vi vidas tion, la resto estas aĉetado.

Meze de la jaro la decido jam formiĝis: se mi iros pli profunen en ĉi tion, mi bezonos maŝinon kun vera rezerva spaco. La sekvanta artikolo temas pri tiu aĉeto, ĝiaj honestaj pravigoj, kaj ĝia hontiga vivkuro.

_Sekve: Parto 2 — La Floto. La luda komputilo, kiu neniam ludis, la modeloj, kiuj honeste malsukcesis, kaj la monato kiam mi lernis, ke la bridilo gravas pli ol la modelo._
