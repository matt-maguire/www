+++
title = "AI-agentoj kaj lokaj LLM-oj"
featuredImage = "20260823-gemma-goose.png"
date = 2026-08-23
lastmod = 2026-11-16T11:29:39+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

En mia [lasta artikolo]({{< relref "20260822-claude-code" >}}) pri AI-agentoj mi parolis pri la uzado de la AI-agento Claude Code. Temas pri komerca solvo, facile instalebla, kiu donas bonegajn rezultojn. La risko estas, ke la prezoj eble ne restos tiom alireblaj kiom hodiaŭ, kaj pro tio mi volis trovi alternativan solvon, kiu ŝirmas min de ŝanĝiĝantaj komercoj.

Ekzistas pluraj modeloj kun malfermaj pezoj, kiujn oni povas elŝuti kaj ruli loke sur sia hejma komputilo. Kelkaj el la avantaĝoj estas:

-   Ne estas kotizoj laŭ tokenoj nek tokenaj limoj -- vi povas konsumi tiom da tokenoj kiom vi volas, kiam vi aĉetis vian aparataron.
-   Ĉio, kion vi tajpas en via loke rulanta LLM, restas sur via komputilo, kio forigas ajn privatecajn zorgojn.
-   Neniu ekstera ento povas subite "fermi" la LLM, al kiu vi ekfidis.
-   Vi ne bezonas retan konekton.
-   vi ne devas kunhavi aparataron kun iu ajn alia.

Kompreneble, estas ankaŭ kelkaj malavantaĝoj:

-   vi bezonas sufiĉe potencajn aparatarajn specifojn por bona rendimento.
-   la modeloj, kiujn vi povas ruli, ne estas tute tiom kapablaj kiom la nubo-bazitaj modeloj, kaj VRAM-limoj povas limigi la grandecon de la kunteksta fenestro, kiun vi realisme povas uzi.


## Elektante modelon {#elektante-modelon}

Laŭ mia sperto pri la uzado de Claude, mi trovis ke mi kutime uzas ĉirkaŭ 100k tokenojn por krei aron da lecionaj notoj. Mia komputilo estas luda PC ekipita per NVIDIA RTX 5090 kun 32GB da VRAM. La kvanto da VRAM determinas la grandecon de la modelo, kiun vi povas uzi, same kiel la longon de la kunteksta fenestro. Mi serĉis modelojn, kiuj donus al mi 128k+ kuntekstan fenestron kaj kongruus kun la havebla VRAM. Kompreneble, mi ankaŭ demandis AI-babilroboton pri rekomendoj.

La unua rekomendo, kiu revenis, estis uzi Ollama por ruli la modelojn. Rulante lokan Ollama-servilon, estas simple elŝuti diversajn tipojn de modeloj kaj ŝalti inter ili. La rekomenditaj modeloj apartenis al du ĉefaj kategorioj:

-   **Densaj modeloj:** tiuj modeloj emas esti pli potencaj, sed povas ankaŭ esti pli malrapidaj
-   **Mixture of Experts:** ankaŭ konata kiel miksaĵo de spertuloj, tiuj konsistas el pluraj pli malgrandaj modeloj. Antaŭe estas iom da logiko, kiu direktas la demandon al unu el la "spertuloj", kiuj specialiĝas pri certaj taskoj, kaj tiam via videokarto bezonas ruli nur tiun pli malgrandan specialan modelon, kio rapidigas ĝin.

Du modeloj, kiuj elstaris, estis _Qwen_ de la ĉina firmao Ali Baba kaj _Gemma_ de Deepmind de Google. Mi eksperimentis kun variantoj el ambaŭ familioj. Fine mi elektis Gemman, ĉefe pro ĝiaj kapabloj pri bild-procezado (pli pri tio poste).

La du modeloj, al kiuj mi alvenis, estis:

-   gemma4:31b -- temas pri densa modelo kun 31 miliardoj da parametroj. Kun mia videokarto mi povas komforte ruli kuntekstan fenestron de 128k. Tio estas mia ĉiutaga laborilo.
-   gemma4:26b -- temas pri Mixture of Experts (MoE) modelo, kiu okupas malpli da memoro. Kun ĉi tiu modelo mi pliigis mian kuntekstan fenestron ĝis 256k, tiel ke mi povas interŝanĝi pli grandan kuntekstan fenestron kontraŭ iomete malpli kapabla modelo.

Rimarku, ke Ollama defaŭlte uzas nur malgrandan kuntekstan fenestron. Vi povas anstataŭi la defaŭltan kuntekstan fenestron per Modelfile kun agorditaj parametroj, kaj poste uzi Ollama por krei agorditan modelon, kiu montras reen al la sama pezo-matrica dosiero, kiun vi jam elŝutis por la originala modelo. Vi povas demandi vian AI-LLM kiel fari tion. Kiam tio estis farita, venis la tempo ekigi la LLM:

```text
$ ollama run gemma4-31b-128k
>>> Hi Gemma! Are you ready to make some lesson files for me?
Thinking...
"Hi Gemma! Are you ready to make some lesson files for me?"
The user wants to create educational content (lesson files).
Friendly, eager, collaborative.

    *   Am I capable of creating lesson plans? Yes.
    *   Can I generate worksheets, quizzes, slides outlines, and instructional materials? Yes.
    *   What do I need from the user to be effective? Subject, grade level, learning objectives, duration, and specific
formats.

    *   *Enthusiasm:* Acknowledge the request with a positive tone.
    *   *Capability Statement:* Clearly state what can be done (lesson plans, quizzes, activities, etc.).
    *   *Requirement Gathering:* Ask for the necessary details to ensure the quality of the output.
...done thinking.

I am absolutely ready! I would love to help you put together some high-quality lesson files.

To make sure these are exactly what you need, just let me know a few details for each lesson:

...
```

Kiel vi vidas, ĉi tiu LLM defaŭlte malkaŝas sian internan pensadon. Tio estas ankoraŭ unu parametro, kiun oni povas agordi.


## Elektante AI-agentan klienton {#elektante-ai-agentan-klienton}

Kiam mi havis Ollama-servilon funkciantan, mi volis agordi AI-agenton kiel mi faris kun Claude. Inspektante la Emacs-pakon _agent-shell_, mi rimarkis, ke ĝi subtenas AI-agenton nomatan _Goose_ de _Block Open Source_. Ĉi tiu agento subtenas orkestradon de kompleksaj plurpaŝaj taskoj per taskolistoj. Ĝin oni povas ruli tra Emacs agent-shell, aŭ ekzistas labortabla aplikaĵo, kiu ofertas uzant-amikan manieron agordi kaj uzi la AI-agenton.

Mi provis krei kelkajn lecionojn uzante la agenton Goose. Ĝi funkcias sufiĉe bone, tre simile al Claude Code. Mi trejnis ĝin pri kiel formati lecionojn laŭ mia gusto, kiel mi faris kun Claude. Kiel Claude Code, Goose havas ebligeblan etendaĵon de skiloj.

Unu afero, kiu ne tiom bone funkciis, estis kiam mi donis al Goose/Gemma fontan dokumenton enhavantan matematikan notacion kaj diagramojn. Ĝi provis elŝuti la tekston, sed formuloj fariĝis fuŝitaj, kaj enhavo ekzistanta kiel bildoj, ekzemple tabeloj kaj diagramoj, tute ne estis elŝutita. Ĉio ĉi funkciis glate kun Claude, sed mi ankoraŭ ne pretis rezigni.


## Importado de fontaj/referencaj dokumentoj {#importado-de-fontaj-referencaj-dokumentoj}

Ni nun revenas al la punkto, ke la modelo Gemma subtenas bild-procezadon. Anstataŭ uzi nefideblan teksto-elŝutan ilon, mi serĉis vojojn utiligi la bild-procezajn kapablojn de Gemma por inspekti la fontan dokumenton kaj vide rekoni la matematikan notacion, tabelojn kaj diagramojn. Mi instalis programon nomatan `pdftopbm`, kiu konvertas ĉiun paĝon de PDF-dokumento en Portable Bitmap-dosieron, kiun Gemma povas rekon.

Kompreneble, mi ne volas konverti ĉiujn paĝojn de PDF-dosiero mane, precipe se min interesas nur malgranda elekto de paĝoj. Kiel aŭtomatigi tiun procezon? Se nur ekzistus iuspeca AI-ilo, kiu povus aŭtomatigi tiuspecan monotonan laboron... ho, atendu, ĉu ne por tio Goose/Gemma ekzistas?

Mi instrukciis Gemman ĉiam uzi `pdftopbm` por elŝuti paĝojn el ajna PDF-dokumento, pri kiu mi rakontas al ĝi, anstataŭ uzi teksto-elŝuton. Mi faris tion konstanta parto de skilosiero, kaj nun mi simple metis la PDF-en en mian lecian dosierujon, diris al Gemma kie la dosiero estas, kaj tra Goose ĝi trovos la PDF-en kaj englutos ĝin laŭbezone.

Mi poste malkovris, ke tion faras iloj kiel Claude malantaŭ la scenoj --- konverti al bildoj kaj uzi OCR.

Mi nun havis sufiĉe potencon AI-agentan aranĝon, plene loke rulantan sur mia luda PC. Dum mi estis hejme, mi povis ssh-en al mia luda PC kaj ruli emacs en teksta reĝimo. Sed kio se mi volus rigardi PDF-eligojn, aŭ aliri la LLM-for de hejmo? Sendube ekzistas vojo fari tion tra la interreto...


## Konektante la LLM al la interreto {#konektante-la-llm-al-la-interreto}

Kiam oni uzas la labortablan aplikaĵon Goose, oni devas montri al ĝi API-finejon, tra kiu ĝi povas aliri LLM. La Ollama-servilo rulanta sur mia luda PC aŭskultas TCP-pordon por envenantaj HTTP-petoj. Kutime ĝi ligiĝas al la loka loopback-aparato, tiel ke oni povas konektiĝi al ĉi tiu API nur loke de la luda PC mem. La demando estas, kiel mi povas eksponi ĉi tiun API-on sur la interreto sekure kaj sendanĝere.

La Ollama-servilo ofertas OpenAI-kongruan API-finejon, kun kelkaj limigoj.

1.  La API estas alirebla per HTTP-petoj, kiuj ne estas ĉifritaj. Sendi neĉifritan informon tra la interreto ne estas rekomendinde.
2.  La API-finejo ne subtenas ajnan aŭtentikigon. Tio signifus, ke ĉiu ajn sur la interreto povus aliri la modelon.

La solvo kompreneble estas enkonduki reversan prokurilon. La reversa prokurilo povas ricevi ĉifritajn HTTPS-konektojn kaj transsendi ilin al la HTTP-API-pordo de la Ollama-servilo. Ĝi povas ankaŭ prizorgi la aŭtentikigan funkcion por certigi, ke nur rajtigitaj klientoj povas aliri la API. Oni povas facile fari tion per programaro kiel _nginx_ aŭ _apache httpd_. Temas pri tre ofta aranĝotipo, kiun oni rapide trovas per reta serĉo aŭ demandante vian AI, do mi ne ripetos tiun informon ĉi tie.

La lasta paŝo estas montri al Goose la HTTPS-API-finejon kaj agordi API-ŝlosilon, kiun la reversa prokurilo akceptos.


## Sekvontaj paŝoj {#sekvontaj-paŝoj}

Kiam mi agordis mian hejman infrastrukturon tiel, ke ĝi estas facile alirebla de ie ajn, mi nun havas "senpagan" alternativon al la pagitaj AI-servoj (mi diras senpaga, sed kompreneble mi ankoraŭ pagas elektrokonsumon kaj aparataran amortizon). La hejma aranĝo produktas altkvalitajn lecionajn notojn. Okaze de kazoj, kie la hejma aranĝo malfacilas, mi ankoraŭ havas la eblon uzi la pli potencajn pagitajn modelojn Claude aŭ Gemini.

Do, kion alian mi povas fari per ĉi tiu aranĝo? Tio kompreneble estos la temo de la sekva artikolo.
