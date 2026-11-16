+++
title = "Mia AI-medio por instruado, Parto 0: Vivo en pura teksto : A Life in Plain Text"
featuredImage = "20261116-ai-setup-0-plain-text.png"
date = 2026-11-16
lastmod = 2026-11-16T11:39:13+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Tio ĉi estas la unua el kelkaj artikoloj pri la AI-sistemo, kiun mi uzas por instrui matematikon. Esence, antaŭ dek monatoj mi decidis aĉeti AI-abonon por lerni pli pri la teknologio, kaj hodiaŭ mi mastrumas malgrandan floton da maŝinoj kaj agentoj, kiuj skizas miajn lecionojn, kontrolas miajn ekzamenojn, indeksas mian laŭprogramon kaj kverelas kun mi pri dezajno je noktomezo. Tiu ĉi unua artikolo temas pri la lastaj jaroj _antaŭ_ io ajn el tio, ĉar montriĝas, ke ĝi estas la parto, kiu ebligis ĉion ceteran.


## Jaroj da LaTeX {#jaroj-da-latex}

Preskaŭ ĉiun instruistan dokumenton de mia kariero mi verkis per LaTeX. Komence estis mia instruista diplomo, la Magistro pri Instruado, kiu estas postulo por fariĝi kvalifikita instruisto en mia hejmŝtato. Mi jam antaŭe uzis tipumajn pakaĵojn por akademiaj dokumentoj, ekzemple [Lout](http://jeffreykingston.id.au/lout/), kaj mi konis la potencon de koncentriĝi al la enhavo dum la iloj prizorgas la prezentaĵon. Aparte, ni devis uzi la APA-citan sistemon por ajnaj citaĵoj en niaj taskoj, kaj anstataŭ ĉieni pri ĉu mi bezonas kursivan tiparon _tie ĉi_ aŭ komon _tie_, mi decidis delegi tion al LaTeX, industria normo por tipumado de matematiko.

Post la finiĝo de miaj studoj, mi studis la lecionajn notojn, kiujn produktis aliaj instruistoj en mia lernejo, kaj decidis, ke mi volas disvolgi mian propran stilon, en kiu la ekzemplaj bildoj ne estus fiksitaj neŝanĝeblaj bitmapoj, sed priskribitaj en LaTeX-kodo, kiun mi povus pli facile ŝanĝi kaj adapti. Komence la laboro estis malrapida, sed finfine ĝi kreskis en mian propran hejman stilon: lecionaj notoj, laborfolioj, taksotaskoj — ĉio tekstdosieroj, kiuj kunmetiĝas al PDF. Tiu elekto estas la malplej brila kaj plej konsekvoplena decido en la rakonto. Iom post iom, mia tuta profesia produktaĵo — ĉiu leciono, ĉiu demando, ĉiu poentodistribuo — migradis al pura teksto en dosierujoj.

La kosto de tiu elekto estis kompreneble tempo. LaTeX estas postulema, kaj la diagramoj eĉ pli. Desegni unu nombrolinion kun etikeditaj frakcioj, aŭ cirkon dividitan por la enkonduko de frakcioj, povis postuli dudek minutojn da Ti*k*Z-ĉienado — koordinatoj, ankroj, superpoziciitaj etikedoj, la eta babilo de vektorgrafiko. Kaj la diagramoj gravas: mi forte apogas min je duala kodigo, kie la algebro kaj la bildo reciproce fortikigas unu la alian, do "preterlasu la diagramon" neniam estis opcio. La Ti*k*Z-imposto estis simple vivfakto, pagata en ĉiu lecion-plana sesio.

Parton de tio mildigis la uzo de potenca teksta redaktilo nomata Emacs, kiu ofertas diversajn klavaro-ŝparvojojn kaj aŭtomatan kompletigon por forigi parton de la skribista peno. Mi de longe fojfoje eksperimentis kun Emacs, ofte revenante al la redaktilo Vi, kiun mi pli bone konis. Estis dum mia instruista diplomo, ke mi decidis plene kaj ĝuste lerni la Emacs-redaktilon, kaj dum la lernado mi malkovris, ke Emacs estas multe pli ol simpla teksta redaktilo.


## Org-mode-cerbo {#org-mode-cerbo}

Dum mi serĉis produktivecsistemojn, kiuj helpus min organizi mian laboron, mi ekkonis diversajn sistemojn kiel [Getting Things Done](https://en.wikipedia.org/wiki/Getting_Things_Done), [Zettelkasten](https://zettelkasten.de/introduction/) kaj [Obsidian](https://obsidian.md/), kaj mi ekkomprenis, ke ĉio ĉi estas farebla en pura teksto per la [Org mode](https://orgmode.org) de Emacs: miaj notoj, taskoj, kalendaro, scibazo kaj lecionplanoj — ĉio estas tekstdosieroj, interligitaj laŭstile de zettelkasten. La instruada scio — NESA-atingoj, laŭprogramaj kapricoj, kiu klarigo funkciis por kiu temo — amasiĝis en miloj da malgrandaj ligitaj notoj.

Tiam tio estis simple mi, teksta obsedato kun Emacs-forma kutimo. Ne venis al mi, ke mi konstruas ion celonte.


## La fono, kiun mi devus konfesi {#la-fono-kiun-mi-devus-konfesi}

Antaŭ la instruado mi pasis jarojn kiel diplomita elektrika inĝeniero kaj komputoscienclististo, specialiĝinta ĉefe pri programaro, kaj mi estas komputila ŝatokupanto tiel longe kiel mi povas memori. Retoj, terminaloj, iom da neŭra-reto-teorio el miaj bakalaŭraj tagoj, kiam ĝi ankoraŭ estis nisega temo (unura elstara momento estis kiam Geoffrey Hinton mem venis prelegi en la universitato) — nenio el tio estis neformala tiam, kaj nenio el tio aparte preparis min por kio okazis en 2026. Mi mencias ĝin nur ĉar ĝi glatigis la fruan vojon, kaj ĉar la vojo iĝis eĉ pli glata, pro kialoj kiuj fariĝas la punkto de tiu ĉi tuta serio: la teknikaj partoj montriĝis la partoj, kiujn mi povis delegi.


## La unuaj reakiritaj horoj {#la-unuaj-reakiritaj-horoj}

La unua afero, kiun AI iam faris por mi, estis helpi min pri LaTeX-Ti*k*Z-diagramoj. Mi vaste uzis [Stack Exchange](https://stackexchange.com/), interretan forumon plenan de helpemaj konsiletoj. Sciante, ke la AI verŝajne englutis tiun retejon, mi provis priskribi diagramojn en unu frazo por vidi, kion ĝi redonus. Ĝi produktis diagramojn plenajn de koordinatoj, kaj per iom da plia instigo, ĝi eĉ ĝeneraligis la kodon, tiel ke mi povis enigi iujn parametrojn por tuj modifi la diagramon. Dudek minutoj fariĝis du, kaj la du kutime donis pli bonajn rezultojn, ĉar la AI memoris ankorajn trukojn, kiujn mi forgesis aŭ pri kiuj mi eĉ ne konsciis. Tiam venis la LaTeX-prozo, la vortumado de la demandoj, kaj la petoj "pli malfaciligu tiun parton, sed lasu la poentojn samaj", kiuj antaŭe estis horo da atenta kirurgio.

Kaj jen la kvieta ekkompreno, al kiu tiu ĉi serio daŭre revenas: **Mi neniam havis integrigan projekton.** Kiam la AI-iloj alvenis, ili jam povis legi mian tutan profesian vivon, ĉion metitan en puran tekston. Neniu lukto kun PDF-dosieroj, neniuj proprietaj formatoj, neniu eksporta paŝo — la lecionoj, la notoj, la cerbo mem estis tekstdosieroj, kaj teksto estas tio, kion tiuj iloj manĝas. Homoj demandas min, kiel mi enmetis mian scion en la AI. Mi ne faris. Mi jam jarojn skribis ĝin en la denaska lingvo de la AI. Mi nur devis direkti la AI-n al ĝi.

Jen la leciono de Parto 0, kaj mi ne atingis ĝin deliberate: **konstruu vian laboran vivon en pura teksto.** Ne ĉar AI venas — kvankam ĝi venas — sed ĉar teksto akumuliĝas. Ĝi ligiĝas, ĝi estas linio-post-linio komparebla, GitHub ĝin amas, kaj ĝi transvivis ĉiun vendiston. Tiam, iun tagon, montriĝas, ke ĝi estas la fundamento, sur kio ĉio alia konstruiĝas.

_Sekve: Parto 1 — Luita Inteligenteco. La abonoj, kiuj komencis ĉion, la angoro, kiu sekvis, kaj la sankta unua devo de okgigabajta grafika karto._
