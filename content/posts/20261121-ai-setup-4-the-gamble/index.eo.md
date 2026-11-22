+++
title = "Mia AI-medio por instruado, Parto 4: La 108-GB-a Veteto : The 108-Gigabyte Gamble"
featuredImage = "20261121-ai-setup-4-the-gamble.png"
date = 2026-11-21
lastmod = 2026-11-22T18:52:32+11:00
tags = ["AI", "LaTeX", "Hardware", "Teaching", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Partoj [du]({{< relref "index.eo.md" >}}) kaj [tri]({{< relref "index.eo.md" >}}) de ĉi tiu serio temis pri la ordo: la floto, la loĝanta modelo kaj la scibazo, funkciantaj kiel la arkitekturaj diagramoj promesis. Ĉi tio estas la parto, en kiu mi provas ion ambician sabato kaj pasigas la tagon rigardante, kiel miaj propraj supozoj perdas argumenton kontraŭ la fiziko. En konstrua taglibro, la malsukcesintaj eksperimentoj kutime estas la indaj je konservado.

---


## Kio jam funkcis {#kio-jam-funkcis}

Antaŭ la veteto, indas memori, kion la floto jam faris sen ŝvito. [Parto 3]({{< relref "index.eo.md" >}}) priskribis Strata-n detale --- Qwen-Next-stila MoE, loĝanta sur `hyperion` kaj respondanta je ĉirkaŭ 250 tokenoj/sekunde, kun vidado --- do jen la mallonga versio de _kial_ ĝi kuras tiel rapide.

Strata estas miksaĵo-de-ekspertoj (MoE) modelo: 24,576 malgrandaj specialistaj subretoj, kaj nur kelkaj aktiviĝas por iu ajn tokeno. La truko estis intence tavoligi ĝin sur unu maŝino. La 10,720 plej varmajn ekspertojn, elektitajn el la propra profilo de vojo-elektaj frekvencoj de la modelo, oni alligas al la VRAM de la 5090 je ŝalto. Ĉiuj 24,576 sidas en alligita 47-GB-RAM-areno, unu paŝon malantaŭe. Laŭ la dezajno ne ekzistas malvarma tavolo de SSD --- la raraj malvarmaj vojo-elektoj anstataŭe prenas la PCIe-buson. Ĉiu tavolo intence funkcias ĝisplene, kaj ne videblas retkablo: tokeno neniam forlasas la maŝinon, kiu ĝin posedas.

Tenu tiun dezajnon en menso, ĉar la MiniMax-eksperimento aliris ĉiun pecon de ĝi kaj faris la kontraŭon.


## Kial veti entute {#kial-veti-entute}

La malferm-peza modelo MiniMax M2.7 faras ion malfacile ignoreblan: ĝi sidas sur la podio de SWE-bench apud fermitaj frontieraj modeloj, kaj la industrio traktas ĝin proksimume kiel „Sonnet-klasa” --- ĉirkaŭ ok ĝis dek poentoj malantaŭ la nuna Opus ĉe Terminal-Bench (57.0% vs 65.4%). Antaŭ nelonge tiu interspaco estis abismo; nun ĝi estas mezurebla fendo.

Por mia agordo, la tento ne estis abstrakta scivolemo. Ĝi estis tri tre specifaj limigoj:

-   **Studentaj datumoj restas hejme.** Nomoj, poentoj kaj klasregistroj neniam forlasas la domon --- tio estas fera regulo, ne prefero. Loka modelo povas prilabori kontrolitajn studentajn laborojn; nuba modelo, laŭ la sama regulo, ne povas.
-   **La marĝena kosto estas la prezo de elektrumo.** Mi jam faris ĉi tiun argumenton por [Strata]({{< relref "index.eo.md" >}}). Havi la saman ekonomion je preskaŭ-flaŭgŝipa kvalito estas la evidenta sekva skrapo.
-   **Neniu vosta risko de maldisponebleco.** Mi vidis nuban kaskadon, kiu halbigis kontinenton dum semajno; cerbo sur mia propra drato ne povas esti forŝaltita de ies alia konfigura ĝisdatigo.

Do la sabato-demando estis sufiĉe mallarĝa por aspekti prudenta: _ĉu Sonnet-klasa modelo, kuranta loke je nulaj marĝenaj kostoj, povas tute preni la noktan generadon de dokumentoj --- ekzamen-kandidatoj, lecionaj skizoj --- el la nubo?_


## La plano {#la-plano}

M2.7 estas 230-miliard-parametra miksaĵo-de-ekspertoj (MoE) modelo; en IQ4_XS-kvantigo ĝiaj pezoj estas dosiero de 108 GB. Neniu unuopa maŝino en la floto tenas tion memstare. Kion mi havis, kiel [Parto 2]({{< relref "index.eo.md" >}}) klarigis, estis du maŝinoj, kiuj kune tenis multe pli ol sufiĉan krudan memoron: `hyperion` (RTX 5090 kun 32 GB da VRAM kaj 64 GB da DDR5, kutime la hejmo de Strata) kaj `msi` (64 GB da DDR4 kaj 8-GB 2080 SUPER), kabitaj kune per dediĉita 2.5 GbE-etareta linio. Tio estas 168 GB da memoro por 108 GB da pezoj --- la kapaco neniam estis la problemo. La kuriozaĵo estas, ke ĝi venis kiel kvar apartaj memorujoj, kaj la RPC-reĝimo de llama.cpp, konstruita por disigi modelon trans maŝinoj, devus elspezi ilin kiel unu laborantan aron. Sur papero, tio aspektis sufiĉe proksima por provi.

La sabato-horaro estis fiksita: paŭzigi Strata-n, ŝargi M2.7 disigitan trans la linio, ruli mian fiksan testaron (tasko pri Ti*k*Z-diagramo, tasko pri ekzamen-generado, tasko pri leciono, kaj tasko pri pura matematiko), kaj tiam redoni la maŝinon al Strata.


## Kiu rulis la testaron {#kiu-rulis-la-testaron}

Indas diri anticipe, ĉar tio ŝanĝas, kio estas tiu rakonto: la manoj ĉe la benko estis la manoj de la asistanto. La elŝuto estis skripto, kiun la asistanto skribis --- `wget`, kvar eroj, rapidlimigita al 8 MB/s por ke la hejma linio travivu la posttagmezon, 108.4 GB sur disko je la 14:48. La instaloj, la RPC-serviloj sur ambaŭ maŝinoj, la kaptado de ĵurnaloj, la tabelo de tempoj: ĉion rulis la asistanto trans ssh, dum mi rigardis la ĵurnalojn alveni.

Eĉ la testaro estas agenta maŝinaro. `run.sh` enigas unu frostigitan prompton en agentan dresilon --- OpenCode stiranta Strata-n, Claude Code stiranta la nuban enirejon --- ĉiu rulo en freŝa labordosierujo el semo, neniu improviza promptado, la tempoj alskribataj al CSV. La renderitajn paĝojn intence juĝas alia asistanto ol la testata: la gardanto kaj la kontestanto ne kunhavas poenttabulon.

Kio restis mia, estis la plano, la aritmetiko, kaj la decidoj halti. Tenu tiun aranĝon en menso tra la malsukceso sube; ĉi tiu afiŝo estas tiom da testo pri la laboratoria kunulanto kiom pri la modelo.


## Unua rondo: la GPU, kiu forestis {#unua-rondo-la-gpu-kiu-forestis}

La modelo ŝargiĝis. Neniu plendo, neniu averto, ĉio aspektis bone. Tiam mi kontrolis `nvidia-smi`-n, kiu raportis, ke la 5090 uzas 18 megabajtojn.

La antaŭkompilitaj llama.cpp-programoj alvenis sen siaj CUDA-bibliotekoj, do mia grandioza „GPU”-rulo silente ŝultrotuŝis kaj retpaŝis al la procesoroj. Neniu eraro, neniu protesto --- nur silenta retpaŝo, kio estus malŝparinta la tutan tagon, se mi estus fidinta la ŝarg-mesaĝon anstataŭ kontroli, kion la aparataro efektene tenas. Leĝono unua, lernita sufiĉe malmultekoste: _kontrolu, ke la aparataro efektive partoprenas, antaŭ ol kredi la vorton de la programaro._


## Dua rondo: la RAM-aritmetiko {#dua-rondo-la-ram-aritmetiko}

Kun la ĝustaj CUDA 13.5-bibliotekoj instalitaj, la 5090 finfine alvenis al laboro --- 25 el ĝiaj 32 GB uzataj. Por momento tio aspektis kiel progreso. Tiam mi rigardis la alian maŝinon.

La disigo metis ĉirkaŭ 25 GB sur la 5090-n kaj transdonis la restajn 80 GB da pezoj al la RPC-partnero --- `msi`, kiu havas 64 GB da RAM kaj ĉirkaŭ 62 el ĝi uzeblaj. Farante kion ajna laŭvorte-mensanta maŝino farus, ĝi metis ĉirkaŭ 39 GB da modelaj pezoj en swap-memorion kaj ekbatis la diskon: 120 MB/s da lego, 127 MB/s da skribo, 40% da la tempo de la maŝino pasis atendante I/O. Dekvorta testa prompto daŭris pli ol tridek sekundojn kaj neniam finiĝis. La antaŭenira paso ne estis malrapida; ĝi estis haltigita, nedifinite.

Leĝono dua, kiu vere devus esti paŝo nul: _faru la RAM-aritmetikon por la disloko, kiun vi efektive rulis_. Tiu disigo verŝis 108 GB da pezoj en du el la kvar memorujoj --- la 32-GB-karton kaj la 62 uzeblajn GB de `msi` --- kaj tio ne lasas spacon por optimismo, nek paroli pri ŝlosil-valora kaŝmemoro. La suma memoro de la floto neniam estis la problemo; la memorujoj estis.


## Tria rondo: la fiziko {#tria-rondo-la-fiziko}

Unu lasta bato: tri-flanka disigo. La 5090, `msi` trans la drato, kaj la propraj iom super 60 GB da RAM de hyperion eksponitaj kiel tria aparato tra loopback RPC-servilo. Sur papero tio estas ĉirkaŭ 155 GB da adresebla memoro por 108 GB da pezoj --- sufiĉe da spaco, finfine, kvankam sur papero ĝi sufiĉis la tutan tempon. En praktiko, la disigo estas statike dimensita je ŝarg-tempo, la GPU ankoraŭ bezonas 10 GB por labora kunteksto kaj aktivigoj, kaj mi estis alliginta `n_gpu_layers`-n je 99 --- ĉiu tavolo sur la karto --- kio diris al la memoro-taŭga rutinio flanken stari anstataŭ helpi. La memoro-alokinto tiam petis de `cudaMalloc` 40 GB sur 32-GB-karto, kaj sep horoj kaj kvardek-kvin minutoj post la starto de la servilo, ĝi mortis kun eraro de memormanko. La ĵurnalo de la RPC-servilo por tiu provo estas vico da identaj linioj --- _Accepted client connection / recv returned 0 (peer closed?) / Client connection closed_ --- la kliento mortanta denove kaj denove, ĝentile.

Jen la honesta vero, kiun mi povus lerni pli malmultekoste per pensado unue: eĉ se la disigo estus ŝargiĝinta, tranĉi unu modelon trans reto signifas: _ĉiu tokeno atendas la plej malrapidan ligilon_. 2.5 GbE sonas rapide, ĝis vi konscias, ke la MoE-vojo-elekto transformas la modelajn pezojn en fluon, ne en statikan amason. Ĝi estus malrapida ĉiuokaze. Distribuata inferenco estas problemo de bendolarĝo, vestita per komputa kostumo.


## Kial simple ne disigi la ekspertojn? {#kial-simple-ne-disigi-la-ekspertojn}

La evidenta demando --- [Parto 3]({{< relref "index.eo.md" >}}) jam montris la trukon. Strata estas MoE, kiu ankaŭ ne taŭgas en la VRAM, kaj ĝi funkcias metante la plej varmajn ekspertojn sur la rapidan memoron kaj la malvarmajn unu paŝon malantaŭen. Kial ne fari la saman aferon trans du maŝinoj: varmaj ekspertoj sur la 5090, malvarmaj ekspertoj sur `msi`?

Ĉar la RPC-reĝimo de llama.cpp ne havas tiun ŝaltilon, kaj la diferenco ne estas kosmetika. `tensor-split` asignas tutajn tensorojn al aparatoj je ŝarg-tempo kaj traktas foran memoron kiel sinkronan plenum-unuon: la komputa grafeo atendas la partneron unufoje po transformila bloko, trans la drato, sendepende de tio, al kiuj ekspertoj tokeno hazarde vojiĝas. La tavoligo de Strata funkcias, ĉar la disloko venas el la propraj vojo-elektaj statistikoj de la modelo kaj ĉiu saltono estas surboarda --- PCIe, nanosekundoj, tabelo de vojo-elekto, kiun la loka CPU mem konsultas. La RPC-disigo ne havas vojo-elektajn statistikojn, nek varmajn/malvarmajn arenojn, nek alligadon laŭ frekvenca profilo. Ĝi estas statika amaso da tensoroj kun etareta kablo tra la mezo de la komputa grafeo.

Ekzistas la rultempoj, kiuj farus ĝin ĝuste --- ekspert-paralelaj klusteraj rultempoj, aŭ dezajnoj, kiuj sendas nur la aktivigojn tien, kie la vojiĝintaj ekspertoj loĝas. Tio estas alia klaso de programaro; la llama.cpp RPC ne estas degradita versio de ĝi, ĝi estas tute alia formo. La ideo estis sana kaj la ilo malĝusta, kaj indas skribi, kiu el ili malĝustis, ĉar „la ilo malsukcesis” kaj „la ideo malsukcesis” estas konfuzataj en plej multaj konstruaj taglibroj --- inkluzive de miaj propraj antaŭaj.

Metu tion apud la resumo supre. Strata estas la sama _speco_ de modelo --- granda MoE, kiu ne taŭgas en la VRAM --- kaj ĝi kuras je 250 tokenoj/sekunde sur unu maŝino. La diferenco ne estas la modelo aŭ eĉ la aparataro. Ĝi estas, ke la tavoloj de Strata sidas tute surboarde (VRAM, alligita RAM, PCIe), elektitaj per la propraj vojo-elektaj statistikoj de la modelo, kaj dimensitaj tiel, ke la plej malrapida saltono mezuriĝas en nanosekundoj. La RPC-eksperimento renversis ĉiun el tiuj elektoj: naiva disigo, neniu vojo-elekta inteligento, kaj etareta kablo ĵetita rekte en ĉiun antaŭeniran paŝon. Sama fiziko, kontraŭa direkto.

Do ni ĉesis. Strata revenis enreĝiĝinte --- rapida, sana, kun vidado --- kaj la floto revenis al sia normala formo.


## Bonusa rondo: la malgranda GPU, kiu povis {#bonusa-rondo-la-malgranda-gpu-kiu-povis}

Dum la granda veteto mortis, mi estis stariginta malgrandan vidan modelon (Qwen3-VL-8B) sur la 8-GB 2080 SUPER. Donite al ĝi matematika laborfolio --- fotita, ne tajpita --- ĝi redonis la ĝustajn respondojn en malpli ol sekundo. Ĝi poste spektakle malsukcesis pri identigo de inĝeniera diagramo, kio tenas la atendojn honestaj. Tamen, por laborfolio-en, respondoj-el, ĝi estas vere utila laboro el karto, kiun mi aĉetis por tute alia jardeko.


## Kion la testaro anstataŭe diris {#kion-la-testaro-anstataŭe-diris}

M2.7 neniam finis testan rulon, do ĝi restis ekster la poenttabulo. La testaro ruliĝis kontraŭ ĉio alia, kio _povis_ --- la loĝanta Strata, Claude tra API, kaj tuta parado da aliaj lokaj kandidatoj. Kelkaj honestaj nombroj:

| Task           | Strata (local)          | Claude (API) | Best other local |
|----------------|-------------------------|--------------|------------------|
| Pure maths     | **1 s**                 | 7 s          | 57 s             |
| Ti*k*Z diagram | 221 s                   | 69 s         | 491 s            |
| Exam paper     | 556 s (gave up mid-run) | **152 s**    | 698 s            |
| Lesson draft   | **142 s**               | 165 s        | 926 s            |

La dividiĝo de venkoj estas sufiĉe klara. La loka loĝanto venkas la mallongajn taskojn kaj sufokiĝas ĉe la longaj; la nuba enirejo venkas la longajn --- kaj en unu rulo mortis jam post kvin sekundoj kun „Credit balance is too low”. Ambaŭ faktoj estas en la tabelo, ĉar ambaŭ faktoj veras, kaj ŝajnigi, ke iu flanko estas senerara, helpas al neniu.


## Kion la veteto aĉetis {#kion-la-veteto-aĉetis}

Unu sabato, 108 GB da elŝutoj, kaj nul dolaroj. Kion mi efektive ricevis reen, estis la vera formo de la floto, mezurita anstataŭ divenita. La longvosto de frontiera klaso restas en la nubo; la revo pri nokta bulko-generado restas neprovita; la 8-GB-karto estas nun oficiale vida laboristo; kaj la regulo por iu ajn estonta ambicio pri granda modelo estas tri nombroj sur servieto --- pezoj, RAM, linia rapido --- antaŭ ol eĉ unu elŝuto komenciĝas.

Kaj por esti justaj al la ideo: la tezo ne rompiĝis. 168 GB da kruda memoro por 108 GB da pezoj sufiĉas por gastigi ĉi tiun modelon, kaj la du maŝinoj vere havis ĝin. Kio rompiĝis, estas la distanco inter _havi_ la memoron kaj _elspezi_ ĝin --- la programaro por transformi kvar honestajn memorujojn en unu laborantan aron ne ekzistis en la formo, kiun mi havis.

Kaj la aranĝo mem ricevis sian laborpruvon. La asistanto rulis la tutan testaron --- la elŝutan skripton, la instalojn, la servilojn, la ĵurnalojn, la tabelon de tempoj, la verdikton „Taksita kaj Retirita” en la testaj notoj --- dum mia kontribuo estis la plano, la servieta aritmetiko, la rigardo al nvidia-smi, kiu kaptis la silentan retpaŝon, kaj la decidoj halti. Laboratoria kunulanto tiom bona ne igas la eksperimenton honesta; ĝi igas la malsukceson legebla, ĝis la megabajto, en la momento kiam ĝi okazas. Jen la interkonsento, kaj sabato pruvis ambaŭ duonojn de ĝi.

Montriĝas, ke ankaŭ la Etereco havas limon de bendolarĝo.

_Sekve: Parto 5 --- Agordado de 8-GB-karto. La dua akto de la malgranda GPU._
