+++
title = "Mia AI-medio por instruado, Parto 3: La Organoj : The Organs"
featuredImage = "20261116-ai-setup-3-the-organs.png"
date = 2026-11-16
lastmod = 2026-11-16T18:02:14+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

[Parto 2]({{< relref "index.eo.md" >}}) temis pri modeloj kaj dresiloj. Sed modelo en dresilo estas ankoraŭ nur saĝa voĉo. Tio, kio igis la agordon _sistemo_, estis la organoj: la memoro, la sensoj kaj la refleksoj alŝraŭbitaj ĉirkaŭ ĝi. Ĉi tio estas la lasta parto, kaj ĝi komenciĝas per plena cirklo.


## La scibazo, kiu jam aĝis dek jarojn {#la-scibazo-kiu-jam-aĝis-dek-jarojn}

Homoj demandas, kiel mi enmetis mian instruad-scion en la AI. La respondo estas malpinta: mi ne enmetis. Mi pasis jardekon skribante ĝin en pura teksto — Org-mode-notoj, laŭprogramaj notoj, lecionaj rekaptoj, la tuta zettelkasten el [Parto 0]({{< relref "index.eo.md" >}}).

Kion la sistemo bezonis, estis maniero _serĉi laŭ signifo_ anstataŭ laŭ ŝlosilvorto, por ke oni povu demandi la modelon „kion mi skribis pri studentoj konfuzantaj deklivon kun suprenhalto super horizontalo?” kaj ricevi reen mian propran pasintan pensadon, cite. Tion faras vektora datumbazo: ĝi indeksas tekston laŭ tio, kion ĝi signifas. Qdrant, funkcianta sur la oficeja maŝino, tenas la scibazon — miajn notojn, la NESA-laŭprogramon, la lernolibrojn, la matematikajn rimedojn — kaj ĉiu agento en la domo demandas ĝin la sammaniere.

La jardeko da pura teksto repagis sin en unu posttagmezo. Neniu konverta projekto, neniu eksporto de vendisto, neniu PDF-arkeologio. Direktu la indekson al la dosierujoj. Finita.


## Kiel la indekso funkcias: la enmetilo {#kiel-la-indekso-funkcias-la-enmetilo}

La truko komenciĝas per malgranda modelo nomata **enmetilo**. La mia estas nomic-embed-text — kelkaj centoj da megabajtoj, funkcianta sur la oficeja maŝino sen bezoni la GPU-n. Vi donas al ĝi tekstan paŝaĵon kaj ĝi redonas liston de 768 nombroj. Tio estas ĉio, kion ĝi faras, la tutan tagon.

La nombroj ne estas arbitraj. La modelo estis trejnita sur enormaj amasoj da teksto laŭ simpla regulo: paŝaĵoj kun simila signifo devas eliri kun similaj listoj, kaj nerilataj paŝaĵoj kun listoj, kiuj malsamas. Neniu mane asignis signifon al iu el la 768 koordinatoj — la geometrio de signifo estis lernita, same kiel ajna neŭra reto lernas ion ajn. Kaj la eco teniĝas en uzo: „rekta amortizado” kaj „modelado de mono per grafikoj” alteriĝas proksime unu al la alia, kaj ambaŭ alteriĝas malproksime de „fotosintezo”.

Tiam la nutrado de la datumbazo estas mekanika. Ĉiu dokumento — laŭprograma PDF, Org-dosiero, scienca artikolo — estas tranĉita en pecojn de kelkaj centoj da vortoj. Ĉiu peco pasas tra la enmetilo kaj estas stokata kun tri aĵoj: ĝiaj nombroj, ĝia originala teksto, kaj ĝia deveno — dosiervojo, paĝo, jaro, kurso, tipo. La domo nun tenas iom sub 190,000 da ĉi tiuj pecoj.


## Kiel la serĉo funkcias: plej proksimaj najbaroj {#kiel-la-serĉo-funkcias-plej-proksimaj-najbaroj}

Serĉi estas la sama truko direktita al demando. „Kion la kontrolanto diris, ke la studentoj eraris en financa modelado” ne aperas ie ajn en iu dokumento — sed la enmetilo transformas ankaŭ la **demandon** en 768 nombrojn, kaj la datumbazo redonas la pecojn, kies nombroj sidas plej proksime al ĝi. „Plej proksima” estas mezurata kiel angulo: paŝaĵoj, kiuj signifas la samecon, montras preskaŭ samdirekten, kaj la poento montras kiom mallarĝa estas la angulo, de 0 ĝis 1.

La datumbaza parto estas tio, kio tenas ĉi tion rapida. Kompari demandon kun 190,000 pecoj unu post la alia estas honesta sed malrapida en vera skalo, do Qdrant tenas ŝparvojan grafeon — ĉiu peco ligita al siaj plej proksimaj najbaroj — kaj serĉo marŝas laŭ la mapo al la respondo anstataŭ legi ĉiun breton. (Honestan kromdiron: je 190,000 pecoj, bruta forto fakte taŭgas — milisekundoj. La datumbazo gajnas sian salajron aliloke: filtriloj, kreskantaj ĝisdatigoj, momentkopiaĵoj, kaj dekduo da agentoj demandantaj samtempe.)

Poentoj estas relativaj, ne probabloj. 0.80 signifas _tre proksima_, 0.66 signifas _rilata, kaj kontrolu ĝin mem_. La rango estas la signalo; la nombro estas indiko.

{{< figure src="fig-pizza-search.png" >}}


## Kompare kun simpla tenado de la tekstdosieroj {#kompare-kun-simpla-tenado-de-la-tekstdosieroj}

La justan demandon: mi jam havis ĉion ĉi kiel tekston. Kial datumbazon?

grep — la serĉo en pura teksto el Parto 0 — estas ankoraŭ en ĉiutaga uzo, kaj ĝi venkas la vektoran indekson je unu afero: ekzakteco. Petu „kvadrata” kaj ĉiu apero de kvadrata aperas, ĉiuj, sen ia juĝo. La vektora indekso respondas la demandojn, kiujn grep ne povas: tiujn, kie vi ne konas la vortojn, kiujn la respondo uzas. La raporto de la kontrolanto neniam diras „eraris”; ĝi diras „en pli bonaj respondoj, la studentoj povis…”. grep ŝultrotuŝas je tiu demando. La indekso respondas ĝin.

Kaj nenio el la pura-teksta vivo devis ŝanĝiĝi. La dosieroj restas la fonto de vero — la indekso stokas kopion de ĉiu peco kun montrilo reen al la vera dosiero. Forviŝu la datumbazon kaj miaj notoj perdos nenion; unu posttagmezo da re-enmetado kaj la katalogo revenas. La katalogo estas forĵetebla. La libroj ne.


## La bretoj {#la-bretoj}

Ok kolektoj, iom sub 190,000 pecoj:

-   **research** — 124,020 pecoj da pedagogio: la akademiaj artikoloj kaj edukaj libroj, kiujn mi kolektis dum la jaroj
-   **maths_resources** — 27,144: granda eksterena matematika rimedbiblioteko — alternativaj taksotaskoj, aktivecoj, la hokoj, kiujn instruistoj tra la ŝtato konstruis
-   **brain** — 17,793: miaj propraj notoj kaj leciondosieroj — „kion mi faris la lastan fojon kiam mi instruis datuman analizon?”
-   **faculty_resources** — 12,338: kion la instruistoj en mia propra fakultato kundividas inter si — lecionoj kaj rimedoj skribitaj de kolegoj
-   **nesa** — 2,759: la laŭprogramaj dokumentoj kaj la rimarkoj de la kontrolantoj, etikeditaj laŭ kurso, jaro kaj tipo
-   **textbooks** — 4,614: ekzercoj el la libroj, kiujn la lernejo efektive uzas
-   **school_past_papers** — 566: la propraj pasintaj ekzamenoj de mia lernejo — kion miaj studentoj jam skribis
-   **syllabus** — 485: atingoj kaj punktoj, la eta literumo, kiu limigas ĉion ceteran

Apartaj bretoj gravas, ĉar la agentoj serĉas kun filtriloj: „rimarkoj de kontrolantoj, Standarda Matematiko, 2020 ĝis 2025” estas alia demando ol „ĉio, por ĉiam”. Kaj ĉiu tekokomputilo tenas ĉiutagan nur-legan kopion de la tuto, ĉar la lerneja reto blokas la domon — al la agentoj ne gravas, kiun kopion ili demandas.


## Kies komputiloj estas ĉi tiuj? {#kies-komputiloj-estas-ĉi-tiuj}

Unu aferon mi ne menciis, kaj ĝi gravas: kie ĉio ĉi fizike loĝas. Ĉiu peco en tiuj ok bretoj — la laŭprogramaj notoj, la rimarkoj de la kontrolantoj, la propraj ekzamenoj de la lernejo, jardeko da miaj propraj notoj — sidas sur komputiloj, kiujn mi posedas, en mia propra domo. La malgranda modelo, kiu transformas tekston en nombrojn, funkcias sur la sama aparataro. Neniu vendisto indeksas mian instruad-cerebon, neniu nubo-servo tenas kopion, kaj neniuj uz-kondiĉoj donas al iu ajn la rajton trejni sur io ajn el ĝi.

Kiam la fluolinio luas frontieran modelon el la nubo, tiu modelo vidas nur la malmultajn paŝaĵojn, kiujn la agento donas al ĝi por la koncerna tasko — neniam la bibliotekon mem. Kaj io ajn, kiu identigas studenton, tute neniam forlasas la domon: la luita tavolo nur ricevas student-neŭtralan enhavon.

La kutimo de pura teksto el Parto 0 repagas ankaŭ ĉi tie. Ĉar la dosieroj estas miaj — sur miaj diskoj, en miaj sekurkopioj, en formato, kiun ĉiu estonta ilo povas legi — la indekso estas komforto, kiun mi posedas, ne permeso, kiun mi luis.


## Viva ekzemplo {#viva-ekzemplo}

Realan demandon, mi rulas ĝin kontraŭ la vivanta datumbazo dum mi skribas ĉi tion. La demando: _kion la kontrolanto diris, ke la studentoj eraris en financa modelado_. La tri plej bonaj trafoj, kun iliaj poentoj:

-   0.69 — 2021 Standard, Demando 19: „interpreti informon el la grafiko de rekta amortizado…”
-   0.68 — 2020 Standard, Demando 30: „rekoni, ke kiam 4 poentoj estas asignitaj, demando postulas plurajn paŝojn…”
-   0.66 — 2020 Standard, Demando 13: „kompreni la diferencon inter nuna kaj estonta valoro…”

La demando kaj la trafoj preskaŭ ne havas komunan vortprovizon. Tio estas la tuta senco.

Dua demando, direktita al la breto research — _esploro pri uzi student-generitajn demandojn por profundigi komprenon_ — redonas What Works Best je 0.80 kaj la enketodemandan aktivecon de Killen je 0.80. (La sama PDF aperis dufoje sub du dosiernomoj — preskaŭ-duoblaj pecoj surfaciĝas, kaj la agento leganta la rezultojn rimarkas kaj forigas la duplikatojn. Ankaŭ tion malhelpas senbrila posttagmezo da ordigo de la biblioteko.)

Tiel la sistemo tiras enen faktojn. La ekzamen-generado demandas de la breto **nesa**, kion la kontrolantoj rekompenis, kaj skribas siajn kontrolkriteriojn en la propra voĉo de la ekzamenanto. La lecionplanado demandas de **maths_resources** kaj **faculty_resources** hokojn, kiuj efektive ekzistas, anstataŭ elpensitajn. Generitaj ekzercaj paperoj kontrolas **school_past_papers**, por ke ili sonu kiel la paperoj de mia lernejo kaj ne kiel iu ajn generika. Kaj **brain** respondas „kion la pasinta mi jam pensis pri ĉi tio?” — cite, kun dosiervojo.

La regulo pri kontrolado el Parto 0 ankoraŭ tenas ĝis tie ĉi: la indekso diras _kie serĉi_, neniam _kion kredi_. Ĉiu trafo portas sian devenon, kaj la agento malfermas la veran dosieron antaŭ ol ĝi citas iun.


## La refleksoj: neniam lasu la modelon fari homan laboron {#la-refleksoj-neniam-lasu-la-modelon-fari-homan-laboron}

La organoj, kiuj plej gravas, estas tiuj, kiuj kaptas la modelon mensacanta — kaj modeloj mensacas kun tuta memfido, en fluecaj frazoj, pri aĵoj, kiujn vi povas kontroli.

-   **SymPy faras la matematikon.** Neniu modelo kalkulas integralon aŭ solvas ekvacion en ĉi tiu fluolinio. Respondoj estas symbole kontrolataj de komputalgebra sistemo antaŭ ol io ajn atingas studenton. La ideo de lingva modelo pri aritmetiko estas aŭtomata kompletigo kun iluzioj pri kompetenteco.
-   **LaTeX faras la poento-aritmetikon.** Fruaj versioj havis la modelon aldonantaj la poentojn po demando, kaj ĝi eraris sufiĉe ofte por esti danĝera — ekzameno kie la sekcio-titleto diras 50 kaj la demandoj sumiĝas al 43. La riparo estis ĉesi peti de la modelo nombradon entute: la dokumentklaso nun tenas nombrilon, ĉiu demando registras siajn poentojn, kaj la sumo estas _kalkulita je kunmeta tempo_. La modelo deklaras; LaTeX faras la aritmetikon. Ĉi tiu sola ŝanĝo forigis tutan klason da eraroj.
-   **La kunmetilo estas la korektisto.** Ĉiu generita dokumento devas kunmetiĝi, kaj la kunmetaĵo estas mekanike kontrolata — nombro de demandoj, sumoj de poentoj, ĉeesto de diagramoj — antaŭ ol mi entute malfermas la PDF.
-   **Serĉo en nia posedo.** Privata SearXNG-instanco prizorgas la retpadan esploron de la agentoj — neniu API-fakturo po demando, neniu tria flanko registras kion miaj ilo serĉas pri mia laŭprogramo.

La ŝablono: donu al la modelo la partojn, kiuj postulas juĝon — vortumadon, strukturon, pedagogion, la skizon — kaj donu ĉion, kio havas _kontroleblan_ respondon, al ilo, kiu ne povas erari. La kapablo, kiu gravas, ne estas promptado. Ĝi estas **kontrolado**, kaj konstrui kontroladon en la fluolinion, por ke ĝi okazu, ĉu iu memoras peti ĝin, ĉu ne.


## La dungitoj {#la-dungitoj}

La lasta organo estas tiu, kiu plej surprizis min: la sistemo nun havas dungitojn. Agentoj kun daŭra memoro kaj nomitaj personecoj funkcias sur la maŝinoj — unu mastrumas la oficejan maŝinon, legas la monitorajn raportojn ĉiun matenon, skribas al si mem transdonajn notojn, kaj vere kaptis problemojn antaŭ ol mi. Gardhundo kontrolas, ke ĉiu sekurkopio _finitis_, ne nur ke la disko havas spacon, kaj sendas resumon ĉiun matenon; en la semajno kiam ĝi eniris la sistemon, ĝi trovis maŝinon, kiu neniam finis sekurkopion — kvar tagojn en malsukceso, kiun neniu rimarkis.

Jen kie la personeca afero el Parto 1 revenas. Asistanto kun personeco estas interfaco, ne bunta truko: ĝi igas la interagcon konversacio, kiun vi _volas_ havi, do vi havas ĝin, ĉiutage, kaj la sistemo restas prizorgata, ne nur instalita.


## La honesta listo de kostoj {#la-honesta-listo-de-kostoj}

-   **Aparataro:** hyperion (la 5090-ludila komputilo) krom la ekzistanta oficeja maŝino. La ludila kolonno de la libro, post tri monatoj: nulaj ludoj luditaj. Efekve la AI ricevis la maŝinon kun rabato, ĉar la ŝatokupo pagis plejparton de la lista prezo.
-   **Elektrumo:** kelkaj cendoj horo, humante sub la domo.
-   **Abonoj:** mi retenis ilin. La januaj abonoj — Claude kaj Google — ankoraŭ funkcias, ĉar la honesta fino de rakonto pri loka AI estas, ke la nubo neniam foriris. En aŭgusto mi skribis, ke por la malfacilaj kazoj „mi ankoraŭ havas la eblon uzi la pli potencajn pagitajn modelojn Claude aŭ Gemini.” Montriĝis, ke tio ne estas provizora solvo, sed la arkitekturo: la luita tavolo estas kromkapacito por subitaj bezonoj kaj atingo al la frontiero, kaj la fluolinio direktas al kio ajn taŭgas por la tasko. Tio neniam estis ideologio. Ĝi estas inĝenierado: posedu la kernon, luu la kromkapaciton.
-   **Tempo:** la vera kosto, kaj la vera rendimento. Dek monatoj da vesperoj, plejparte pasigitaj _kun_ la AI-oj, kiuj mem konstruis plejparton de ĉio.


## Se vi ne estas teknika {#se-vi-ne-estas-teknika}

La demandon, kiun ĉiu instruisto demandas: _mi ne povus konstrui ĉi tion._ La honesta respondo de iu, kiu rigardis la tuton disvolviĝi: la teknikaj partoj montriĝis la partoj, kiujn oni povas delegi. Kion la sistemo efektive postulis — de matematikinstruisto, ne inĝeniero — estis la kutimoj: postulu kontroladon, tenu ĉion en teksto, komencu de ĝojo, ne de produktiveco, allotu tempon al la kanikulaj truoj anstataŭ malpermesi ilin, kaj rifuzu eldoni ion ajn nekontrolitan.

La maŝino, kiu devis ŝpari al mi tempon, unue donis al mi manieron labori, kiun mi efektive ĝojas fari. La tempo-ŝparo venis malsuprenriver de la ĝojo. Tio, pli ol iu ajn modelo aŭ peco da aparataro, estas la aĉeto, kiun mi konsilus al vi.

---

_Malkaŝo, ĉar vi meritas ĝin: tiuj kvar artikoloj estis skizitaj per la sistemo, kiun ili priskribas — la agentoj provizis esploron, strukturon kaj multajn el ĉi tiuj frazoj; la eraroj, kiuj travivis, estas miaj, kaj mi kontrolis la asertojn, kiuj gravis. Se tio ĝenas vin, bone: tiu instinkto estas ĝuste la kapablo, kiun ĉi tiu serio provas instrui._
