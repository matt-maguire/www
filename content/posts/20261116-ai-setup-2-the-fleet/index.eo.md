+++
title = "Mia AI-medio por instruado, Parto 2: La Floto : The Fleet"
featuredImage = "20261116-ai-setup-2-the-fleet.png"
date = 2026-11-16
lastmod = 2026-11-22T18:57:34+11:00
tags = ["AI", "LaTeX", "Teaching", "Hardware", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

[Parto 1](https://www.teachmaths.org/eo/20261116-ai-setup-1-rented-intelligence/) finiĝis ĉe la 8 GB-plafono kaj ĉe decido. Ĉi tio estas la hardvara parto, kaj la parto, en kiu la sistemo ĉesis esti komputilo kaj komencis esti personaro.


## La ludkomputilo, kiu neniam ludis {#la-ludkomputilo-kiu-neniam-ludis}

En julio mi konstruis hyperion: vera ludkomputilo, RTX 5090, 32 GB da VRAM. La pravigoj estis du, kaj mi estis honesta pri ambaŭ tiam. La AI-pravigo: se mi iros pli profunden en ĉi tion, mi bezonos rezervan spacon — sufiĉe da VRAM por vere bona malfermita modelo kun longa kunteksta fenestro. La ŝatokupa pravigo: mi deĉirkaŭ jardeko volis grandan ludkomputilon, kaj AI estis la unua preteksto sufiĉe forta por postvivi mian propran kontadon.

La hontinda vivkuro, tri monatojn poste: ludoj instalitaj kaj ludataj: nul. La tuta laborvivo de hyperion estas instruado. Ĝi servas la modelon, kiu skizas miajn lecionojn, gastigas la sekurkopiojn de mia familio, kaj humas sub la domo farante absolute nenion alian. La ŝatokupa preteksto neniam elspeziĝis; la labora preteksto montriĝis la vera. Mi demandas min, ĉu mi povas deklari ĝin en miaj impostoj... probable temo por interparolo kun mia kontisto, ne por ĉi tiu blogo.


## Modeloj: la honestaj malsukcesoj {#modeloj-la-honestaj-malsukcesoj}

Kiam mi skribis la aŭgustan artikolon, la ĉiutaga laborĉevalo estis Gemma — densa modelo de 31 miliardoj da parametroj servata de Ollama, kun la agento Goose stiranta ĝin. Tiu agordo vere funkciis, kaj tiam la tero ekmoviĝis sub ĝi, ĉar ĉi-jare la tero daŭre moviĝas. Gemma lasis la lokon al Qwen de 27 miliardoj da parametroj, kaj tiu al Mixture-of-Experts-modelo de 30 miliardoj da parametroj — ankoraŭ sur simpla Ollama, ankoraŭ sur la sama karto. La MoE-truko estis pruvo, ke la sekvaj salto estos softvaraj, ne hardvaraj. La modelo, kiun mi retenas hodiaŭ, estas pli granda MoE: granda biblioteko de pli malgrandaj specialistoj, kie ĉiu peto vekas nur la malmultajn fakulojn, kiujn ĝi bezonas. Tiu arkitekturo ebligas al 32 GB-karto servi ion vere saĝan: la _tuta_ modelo ne devas enspaciĝi, nur la partoj, kiuj estas konsultataj. Ĝi skizas miajn lecionajn notojn kaj miajn taksotaskojn laŭ nivelo, kiun mi subskribas, kaj ĝi funkcias per elektro, ne per ies preza paĝo.

{{< figure src="fig-5090-gemma.png" >}}

{{< figure src="fig-5090-qwen.png" >}}

{{< figure src="fig-strata-tiers.png" >}}

Intermite mi kolektis malsukcesojn, kaj ili estas la utila parto:

-   Multe laŭdata modelo alvenis kun reputacio de saĝeco. Ĝi glate trapasis la puran matematikan kontroladon kaj tiam, petita desegni nombrolinion, memfide amasis ĉiun etikedon je la sama koordinato. **Matematika cerbo, rompitaj manoj.** La reputacio de modelo ĉe benchmarkoj nenion al mi diris pri tio, ĉu ĝi povas produkti _miajn_ dokumentojn.
-   Unu benchmarka rulado mistere mortis, ĝis mi trovis la kaŭzon: kunteksta-fenestra limo agordita en malĝusta loko, silente kunpremanta la memoron de la modelo pri sia propra laboro meze de la tasko. **Duono el "la modelo malsukcesis" fakte estas "la duktilaro malsukcesis".**
-   Maŝino kuranta du modelojn samtempe ekis paĝigi pecojn de ili al disko. La modelo iĝis delikate malpli bona, je neniu aparta maniero, kio estas la plej malbona maniero por softvaro malsukcesi. **Modelo, kiu estas elŝarĝita, estas modelo kun kapdoloro.**

La malnova oficeja maŝino ne forserviĝis — ĝi nun kuras pli malgrandan, rapidan Qwen kiel la motoro de dua opinio, kaj la 2080 finfine gajnas propran salajron post siaj jaroj da roludaj deĵoroj.

{{< figure src="fig-msi-hybrid.png" >}}


## La dresilo estas la produkto {#la-dresilo-estas-la-produkto}

La plej granda leciono de la jaro, kaj tiu, pri kiu mi ricevis neniun averton: **la modelo gravas malpli ol la ĉirkaŭaĵo, kiun vi konstruas ĉirkaŭ ĝi.** Mezkvalita modelo en bona dresilo konstante batas brilan modelon en malbona dresilo, en vera laboro.

La dresilo estas ĉio inter "babili kun modelo" kaj "eltiri dokumenton": ĝi legas kaj redaktas dosierojn, rulas la LaTeX-kunmetilon, vidas la erarojn, riparas ilin, rulas la kontroladon, tenas planon, kaj konas vian hejman stilon. La nuna plej bona produktaĵfaristo en la domo estas _dirigento_: unu modelo planas kaj delegas, pli malmultekostaj modeloj plenumas la subtaskojn en puraj kuntekstoj, kaj la plananto kontrolas la rezultojn. Alivorte, komitato. Ĝi superas ajnan unuopan modelon, kiun mi povas ruli, je frakcio de la kosto de la evidenta alproksimiĝo.


## Riparante la okulojn de la agento {#riparante-la-okulojn-de-la-agento}

La [aŭgusta artikolo](https://www.teachmaths.org/eo/20260823-gemma-goose/) finiĝis per la _pdftopbm_-kurzovojo: ĉar Gemma povis vidi bildojn, Goose lernis konverti ĉiun PDF-paĝon en bildon kaj legi la paĝon tiel, kiel mi legus. Ĝi funkciis. Kion mi volis sekvance, estis la samaj okuloj direktitaj enen. Mi volis, ke la agento kontrolu la grafikojn, kiujn ĝi produktas — parabolo, kies etikedoj kolizias kun la kurbo, legendo sidanta sur ombrita regiono, aksoj tute sen etikedoj. La speco de afero, kiun homo kaptas per unu rigardo al la kunmetita PDF.

Atingi tion signifis provi aliajn dresilojn, ĉar modelo kaj dresilo havas kemion, kaj iuj paroj simple ne funkcias. Qwen2-coder-modelo, kiu skribis tute bonan kodon, rifuzis ĝuste voki ilojn ene de Goose. La cerbo en ordo; la kuplo difektita. Tio estas la sama leciono kiel ĉe la modelo paĝigita al disko: duono el "la modelo malsukcesis" fakte estas "la duktilaro malsukcesis".

Tiam mi trovis veran cimon. Goose estas liverata kun katalogo de konataj modeloj, kaj modelo, kiun mi mem servas, ne estas en tiu katalogo. Mi estis deklarinta _supports_vision_ en mia agordo; Goose ignoris la deklaron kaj silente forĵetis ĉiun bildon el la peto. La modelo neniam vidis la grafikon. Neniu eraro, neniu averto — la agento simple kritikis eligojn, kiujn ĝi ne povis vidi, memfide kaj detalege.

Mi neniam skribis Rust. Do mi petis la AI-n helpi min fliki Goose mem: legi kodbazon, kiun mi ne povis legi, trovi kie la kapabloj de la provizantoj resolviĝas, skribi la flikon kaj la testojn. Kvarcent kvardek aldonitaj linioj en dek dosieroj, senditaj kiel [tira peto 12769](https://github.com/aaif-goose/goose/pull/12769), kunfanditaj la sekvan tagon. La agento rericevis siajn okulojn, kaj nun uzas ilin je sia propra laboro: kunmeti, rigardi, ekvidi la superpoziciitajn etikedojn, ripari la aranĝon, rebildigi. Iaen meze de ĉio, la sistemo riparis siajn proprajn okulojn, kaj mi gajnis kunfanditan kontribuon al malfermfonta projekto, kiun mi neniam planis tuŝi.


## Alteriĝo ĉe Hermes {#alteriĝo-ĉe-hermes}

Ĝis aŭtuno la dresila sceno eksplodis. Al Goose, la agento kiun mi uzis en aŭgusto, aliĝis — kaj plejparte superis ĝin — tuta generacio: OpenCode, la propra agento de Qwen, la Antigravity de Google. Ĉiuj el ili brilaj kodadaj asistantoj. Mi alligis ilin ĉiujn al tiu maŝino, kiu taŭgas por ĉiu tasko, kaj mi retenis la plej bonan el ili.

Sed tio, ĉe kio mi fakte alteriĝis, estas alispeca. Hermes ne estas kodada asistanto, kiu atendas, ke mi malfermu terminalon. Ĝi ĉiam estas ŝaltita: ĝi tenas memoron inter sesioj, ĝi loĝas en WhatsApp-fadeno, por ke mi povu atingi la domon de ie ajn, kaj ĝi rulas planitajn taskojn — memorigilojn, noktajn kontroladojn de la sekurkopioj, seriojn da modelaj benchmarkoj, kiuj ruliĝas dum mi dormas. Ĝi gardas la floton: gardhundo-agentoj, kiuj ĉiumatene kontrolas la disk-spacon kaj la sekurkopiojn kaj levas la alarmon kiam io odoras malĝuste. Kaj ĝi havas kolegojn: kiam la agento de unu maŝino finas eksperimenton, ĝi skribas transdonan noton, kaj alia agento ektenas la laboron la sekvan matenon. Ili ĉiuj ankaŭ legas de la sama breto: vektora datumbazo — Qdrant — tenanta jardekon da miaj purtekstaj notoj, serĉebla laŭ signifo anstataŭ laŭ ŝlosilvorto, demandata la sammaniere de ĉiu agento en la domo. Sed tiu breto estas la temo de [Parto 3](https://www.teachmaths.org/eo/20261116-ai-setup-3-the-organs/).

La amplekso silente larĝiĝis. AI ĉesis esti io, kion mi uzas por produkti dokumentojn, kaj iĝis io, kio helpas mastrumi la etan hejmon de maŝinoj, en kion la instruista agordo kreskis. Jen kie la promeso el [Parto 1](https://www.teachmaths.org/eo/20261116-ai-setup-1-rented-intelligence/) estas plenumata: la asistanto kun personeco deliberate alvenis, kaj ĝi mastrumas ĉi tiun domon.

Kaj la rilato ŝanĝiĝis ie meze de ĉio. Mi ĉesis demandi AI-n kaj komencis disputi pri dezajnoj kun ĝi — petante la lokan modelon helpi taksi, kiun modelon instali, reviziante planojn skribitajn de unu agento por ke alia agento plenumu ilin. La sistemo nun enhavas sian propran personaron. La homa laboro iĝis direkto, gusto, kaj la rifuzo fidi je io ajn nekontrolita.


## La floto bezonas provizejon {#la-floto-bezonas-provizejon}

Dek maŝinoj kaj personaro da agentoj estas unu afero; la datumoj, kiujn ili dividas, estas alia. La spino de la tuta agordo — la Nextcloud, kiu sinkronigas miajn notojn inter hejmo kaj lernejo — sidis sur du eksteraj Seagate-porteblaj diskaroj en USB-doko, spegulitaj unu al la alia. Kaj la USB-pontaj tabuloj rifuzas raporti la sanon de la diskaroj mem: demandu al ili, kiel la diskoj fartas, kaj ili respondas per silento. Mi mastrumis la memoron de la hejmo sur diskaroj, kiujn mi ne povis monitori. Tio, krom la pura grandeco de la afero — 3.5 terabajtoj kaj kreskante — estas kio finfine puŝis min konstrui propran NAS-on.

Estis unu problemo: mi ne konstruis PC-on ekde la 1990-aj jaroj. hyperion alvenis pretmuntita en julio, kaj mi ne malfermis la flankan panelon en la monatoj post ĝi. Modernaj internaĵoj estas fremda lando — diskokagedoj kaj kovriloj, SATA-alimentado, NVMe-fendoj kaŝitaj sub varmforigiloj. Do la AI iĝis mia konstrua partnero, kaj ĝia unua ago estis legado. Ĝi elŝutis la manlibrojn de la komputilkaso kaj de la patrina tabulo, legis ilin ĝuste, kaj sendis al mi la koncernajn paĝojn kiel diagramojn: kie la 3.5-colaj fendejoj efektive troviĝas, ke la tabulo havas kvar SATA-pordojn plus unu konektilon, kiu etendiĝas al kvar pliaj, kiun fendon la ekzistanta diskaro okupas — spurite tra la PCIe-dratado sen malfermi la kason. Ĝi ankaŭ korektis siajn proprajn antaŭajn supozojn, kiuj venis el miaj fotoj. Foto montras, kio estas instalita hodiaŭ; manlibro diras, kio eblas. Mi lernis distingi ilin.

Aĉetado havis unu kaptilon indan koni: la plej malmultekostaj grandaj diskaroj registras **ŝinglite** — SMR — kaj katastrofe rekonstruiĝas, kiam dua diskaro malsukcesas. La regulo estis: nur konvencia registradmaniero, kaj tiel du Seagate IronWolf 8TB-diskoj finis en la aĉetĉaro, enirantaj mem hyperion anstataŭ nova skatolo: la ludkomputilo, kiu neniam ludis, samtempe estas la stokada spino de la hejmo. Antaŭ formati ion ajn, la agento rulis identigan sojlon — modelo, kapacito, seria numero, flago de rotacianta disko, retrolegite rekte de la diskaroj kaj kontrolite kontraŭ tio, kion ni mendis, ĉar duon-enŝovita kablo ne rajtiĝas fariĝi duono el RAID. La paro eniris kiel spegulita btrfs-volumo etikedita **ironwolf**, kun nokta gardanto, kiu legas la diskosanon kaj purigas kontraŭ silenta bitputriĝo.

Tiam venis la transloko: 3.5 TB pramitaj trans rektan 2.5-gigabitan ligilon inter la maŝinoj — unua preterpaso kontraŭ la vivanta arbo, dum la retejo daŭre servis, fina delta-preterpaso dum ĝi estis paŭzita por la transŝalto. La detaloj, kiujn la AI tenis en sia poŝo, estas tiuj, kiuj mordas: la muntpunkto devas kongrui ekzakte kun la datumoj-dosierujo de la aplikaĵo, unu nivelo maltrafe kaj la retejo revenas rompita; la nova muntlinio devas esti tute senpova bloki la ekŝaltadon; kaj la sekurkopio-ilo devas esti kontrolita, ĉu ĝi sekvas la novan muntadon, alie la provizejo restas sen sekurkopioj en silento. La retiriĝintaj porteblaj diskaroj estis reformatitaj kiel tempociferblojoj — momentkopiaj celoj, po unu por maŝino — por ke dosiero forviŝita ĉi-matene estu minuto for de restarigo, kaj unu mortinta maŝino ankoraŭ ne estu perdita jardeko.

Por la rakonto, kiun mi rakontas, ĉi tio estas la bato, kiu gravas: la sama asistanto, kiu flikis la Rust-on de Goose, legis la manlibron de komputilkaso kaj starigis min antaŭ la diskokagedo. AI ne nur loĝis en la maŝino. Ĝi kondukis min paŝo post paŝo tra malfermado de unu, la unuan fojon post dudek-kvin jaroj.

_Sekve: Parto 3 — La Organoj. La scibazo, kiu jam aĝis dek jarojn, la iloj, kiuj igas la kontroladon aŭtomata, kaj la honesta kostlisto, la lud-kolumno: nul._
