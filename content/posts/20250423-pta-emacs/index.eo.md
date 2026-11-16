+++
title = "Pura-Teksta Libroteado per Emacs"
featuredImage = "20250423-hledger_report.png"
date = 2025-04-23
lastmod = 2026-11-16T11:29:45+11:00
tags = ["PlainTextAccounting", "Emacs", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Delonge min maltrankviligis, ke mi ne vere regas miajn financojn tiel bone kiel mi devus, kaj mi decidis fronti la aferon kaj fari ion pri ĝi. Mi celas akiri multe pli bonan vidon pri mia akcia portofolio, kaj certiĝi, ke mi ĝin ĝuste spuradas, por ke se mi vendus iujn akciojn, mi povus facile kalkuli kapitalgajnojn kaj simile dum la impostado. Mi ankaŭ tre volas pli bone kompreni miajn elspezadajn ŝablonojn, por vidi, kie mi eble malŝparas monon.

Ĉar mi lastatempe multe amuzis min enrigardante Emacs Org, mi esploris, ĉu ĝi povus helpi min pli firme regi miajn financojn. Post iom da esplorado, mi ekrealizis, ke Org-mode eble ne estas la plej bona ilo por tio, kion mi celis, sed mi vidis aron da homoj parolantaj pri programo nomata Ledger, kiu kiel enigon prenas purtekstajn dosierojn priskribantajn viajn financojn, kaj poste kribras la datumojn por produkti diversajn raportojn. Ĉar la datumoj estas administritaj per homlegeblaj tekstdosieroj ("librotenaj ĵurnaloj"), Emacs povas esti uzata kiel la preferata teksta redaktilo, kaj en Emacs eĉ ekzistas "Ledger mode" por prizorgi la kutiman sintaksan emfazon kaj aŭtomatan kompletigon, kiujn oni atendas de ajna teksta speciflingvo.


## Elektante Pura-Tekstan Libroteadan Ilon {#elektante-pura-tekstan-libroteadan-ilon}

Ŝajnas, ke ekzistas fervora komunumo, kiu uzas diversajn ilojn kiel Ledger por spuradi siajn financojn per purtekstaj dosieroj. Unu el la ĉefaj centroj estas la retejo <https://plaintextaccounting.org/>. Tiu retejo klarigas kelkajn bazajn konceptojn de librotenado kaj mencias la diversajn haveblajn ilojn. Do, mia unua tasko estis decidi, kiun ilon mi uzu.

1.  [Ledger](https://ledger-cli.org/) estas la plej malnova komandlinia ilo, kaj ĝi havas la plej multajn funkciojn. La ĵurnala formato estas sufiĉe tolerema kaj vaste uzata, kaj Emacs havas _Ledger_-reĝimon.
2.  [HLedger](https://hledger.org/) estas pli moderna reskribo de Ledger, verkita en Haskell. La programisto provis "purigi" iujn apartaĵojn de Ledger kaj plifaciligi la ilon. Al ĝi mankas iuj funkcioj de Ledger, sed la programisto klarigas, kiel pritrakti iujn oftajn uzkazojn per la pli simpla funkciaro. Ĝi ŝajnas esti pli aktive evoluata ol la aliaj iloj de pura-teksta librotenado (PTA). Emacs ankaŭ havas _HLedger_-reĝimon.
3.  [Beancount](https://beancount.github.io/) estas verkita en Python, kaj uzas alian formaton por siaj ĵurnalaj dosieroj ol Ledger kaj ĝiaj derivaĵoj. La formatado de la ĵurnalo estas pli strikta ol ĉe la aliaj iloj.
4.  [GNUCash](https://gnucash.org/) estas GUI-bazita aplikaĵo, kiu uzas SQL-databazon por la daŭra datumkonservejo. Tio tute ne estas pura-teksta librotenada programaro, sed mi decidis tamen enrigardi ĝin por vidi, ĉu ĝi eble pli taŭgus por miaj uzkazoj.

Post enrigardado de ĉi tiuj 4 iloj, mi decidis, ke mi vere volas pura-tekstan librotenadan ĵurnalon, kiun mi povus meti sub versian kontroladon uzante Git/Magit. GNUCash ŝajnis iom tro peza por tio, kion mi volas fari. Mi ne gvidas kompleksan entreprenon; mi simple gajnas salajron, faras iujn aksiajn transakciojn kaj provas regi miajn elspezojn. Por io tia simpla, tekstdosiero plene taŭgas, des pli ke mi jam intense uzas tekstdosierojn tra Emacs/Org-mode. Fine, mi decidis elekti HLedger:

-   ĝi estas pli ĝisdatigita, kaj al ĝi mankas la historiaj kapricoj kaj paŝoferoj de Ledger
-   la dosierformato estas tolerema, kaj ĉar al ĝi mankas iuj el la kompleksecoj de Ledger, mi kredis, ke mi ĝin pli rapide ekkaptus.
-   ĝi estas verkita en Haskell, kiu sonas kiel interesa lingvo, pri kiu mi eble lernos pli, se mi volus skribi iujn proprigojn.
-   se mi volus pli profunde enrigardi Beancount, ekzistas konvertaj iloj, kiuj povas helpi migri al la malsama formato.


## Bazaj Libroteadaj Terminoj {#bazaj-libroteadaj-terminoj}

Kiam mi jam elektis ilon, la sekva afero estis enrigardi, kiel librotenado funkcias. Ĉiuj ĉi PTA-iloj baziĝas sur la ideo de _duobla entrudado_. Mi ne volas detali ĉi tie pri duobla entrudado, ĉar aliaj faris ĝin pli bone ol mi povus. Mi rekomendas al vi legi la [enigantan artikolon](https://plaintextaccounting.org/What-is-Plain-Text-Accounting) sur la retejo plaintextaccounting.org. Sume tamen:

-   Mono estas spurata per enmetado de ĝi en _kontojn_ kaj elprenado el ili.
-   Estas 3 tipoj de kontoj, kiuj konservas valoron: **Aktivoj** (aĵoj, kiujn vi posedas), **Devoj** (aĵoj, kiujn vi ŝuldas al aliaj homoj) kaj **Kapitalo** (tio estas komplika, sed sur simpla nivelo ĝi estas mezuro de la mono, kiun vi enmetis en la entreprenon).
-   Estas 2 pliaj tipoj de kontoj, kiuj reprezentas monon enirantan aŭ elirantan el la entrepreno: **Enspezoj** (mono venanta de ekstera fonto) kaj **Elspezoj** (mono, kiun vi pagas al iu alia).
-   Se vi elprenas monon **el** unu aŭ pluraj kontoj, vi devas enmeti tiun saman sumon **en** unu aŭ plurajn kontojn. El fizika vidpunkto, tio estas io kiel la leĝo pri konservo de energio — mono ne povas esti kreita el nenio, ĝi povas nur esti translokigita de unu loko al alia.


## Agordante HLedger kaj HLedger-mode/Ledger-mode {#agordante-hledger-kaj-hledger-mode-ledger-mode}

Tie ĉi ne estas magio: mi simple instalis _hledger_ per mia pakaĵadministrilo, en mia kazo:

`pacman -Sy hledger hledger-ui hledger-web`

La lastaj du pakaĵoj estas laŭvolaj.

-   `hledger-ui` estas teksta uzantointerfaco (TUI) por hledger. La gamo de raportojn, kiujn ĝi povas fari, estas limigita kompare kun tio, kio eblas per la komandlinia ilo hledger. Tamen ĝi estas oportuna maniero rigardi normaĵojn kiel bilancoj kaj enspezoraportojn.
-   `hledger-web` provizas minimuman retumilan servon, kiu realigas retan GUI. Mi tute ne enrigardis ĝin, ĉar min ĉefe interesas resti ĉe tekstaj raportojn/Emacs.

Por instali hledger-mode en Emacs, vi povas sekvi la instrukciojn ĉe la [github de la programisto](https://github.com/narendraj9/hledger-mode). Mi ne tre profunde enrigardis ĝin, kaj nun mi pensas, ke Ledger-mode en Emacs eble eĉ pli bona elekto ol hledger-mode:

-   ledger-mode estas antaŭpakumita en la Doom Emacs-distribuado, kiun mi uzas.
-   ĝi ŝajnas havi iujn belajn funkciojn kiel M-q por vicigi viajn valutosumojn. Por vidi, kion mi celas, rigardu [ĉi tiun demonstracion](https://archive.org/details/ledger-mode_EmacsConf2019) de Emacs ledger-mode de EmacsConf 2019.

Mi devos esplori ĝin pli.


## Kreante Ĵurnalajn Entrudojn {#kreante-ĵurnalajn-entrudojn}

Ledger estas sufiĉe fleksebla laŭ la maniero, kiel vi uzas ĝin. Vi devas iom pripensi, kiajn aferojn vi volas spuradi. Ekzistas [kuirejoj](https://plaintextaccounting.org/Cookbook), kiuj sugestas diversajn manierojn strukturi viajn entrudojn. Kiel ekzemplon, mi volis spuradi mian salajran enspezon. Tre simple, vi povas registri, kiam vi vidas vian salajron kreditita al via banka konto:

```ledger
2024-04-24 pay day!
    revenues:salary      $-2000  ; out of my employer's account...
    assets:bank:savings   $2000  ; ...and into mine!
```

Ĉi tiu transakcio prenas $2000 de mia dunganto (fonto de enspezo aŭ enfluo), kaj metas ĝin en mian ŝparadokonton. Ĉe duobla entrudado, la sumoj ene de la transakcio ĉiam devas sumiĝi al nul, kaj tial ni havas du sumojn de egala kaj kontraŭa signo. Se vi provas enigi "neekvilibrigitan" transakcion, la sistemo ploras, kaj vi devos ripari la transakcion, por ke la sumoj aldoniĝu al nul.

Ĉar ĉi tiu regulo devas ĉiam validi, eblas lasi _unu_ el la sumoj blanka, kaj la sistemo deduktos, kia sumo devas iri tien, por ke ĉio sumiĝu al nul:

```ledger
2024-04-24 pay day!
    revenues:salary      $-2000  ; out of my employer's account...
    assets:bank:savings          ; system knows that $2000 must go into my account to maintain balance
```

Tiam mi decidis, ke mi volis spuradi, kiom da imposto la dunganto forigis de mia salajra paketo, kaj kiom li kontribuis al mia pensia konto. Ankaŭ, la dato, je kiu mi ricevas mian salajran liston, ne nepre estas la sama dato, je kiu la mono efektive iras en mian konton. Por pli flekseblaj raportojn, mi povas fari ion kiel ĉi tio:

```ledger
2024-04-23 Salary payslip
    revenues:salary:gross         $-2500  ; gross salary excluding taxes
    expenses:taxes:withholding      $500  ; employer pays this to tax office
    assets:receivable:salary       $2000  ; store net salary as receivable until money is received
    revenues:salary:super          $-200  ; compulsory employer contribution to superannuation account
    assets:receivable:super         $200

2024-04-24 Salary deposit
    assets:receivable:salary      $-2000  ; money received, take it out of receivables account
    assets:bank:savings                   ; and put it into my cheque acount.
```

Ĉi tie, mi povas spuradi en la konto `expenses:taxes:withholding`, kiom da mono la dunganto sendis al la impostoficejo. Kio ajn restas, dumtempe iras en konton nomatan `assets:receivable:salary`. Tio estas aktivo, ĉar la dunganto ŝuldas al mi monon. Tamen mi ne volas registri ĝin kiel eniron en mian ŝparadokonton, ĝis la dunganto efektive transigas la monon tien. Kiam li tion faras, mi tiam kreas duan transakcion, en kiu la mono estas translokigita el tiu tena "Receivables" (ricevota) konto kaj en mian ŝparadokonton je la dato, kiam la transigo efektive okazas. Se pro iu kaŭzo la transigo ne okazas, mi povas kuri raporton, kiu montras, ke mi havas iom da mono sidanta en "Receivables", kiu ankoraŭ ne trovis sian vojon al mia konto, kaj mi tiam scias, ke mi devas postkuri ĉe mia dunganto. Simile, mi povas spuradi la devigajn kontribuaĵojn, kiujn mia dunganto laŭleĝe devas fari al mia pensia konto, kaj mi povas rebilanci ĉe la fino de la jaro, kiam mi ricevas la jarraporton.


## Enigante Pli Da Datumoj {#enigante-pli-da-datumoj}

Kiam mi ekkomprenis la bazojn de kiel la librotenado spuradas la fluon de mono tra diversaj kontoj, kiujn oni povas poste raporti, mi trairis kaj enigis ĉiujn transakciojn, kiujn mi faris dum la pasinta financa jaro. Estis multe da enigitaj datumoj, kio sufiĉe tempoprenis, sed ĝi estis utila ekzerco, ĉar ĝi igis min detale ekzameni miajn financojn. Ĝi estis aparte helpa por aksiaj transakcioj, kie mi devis spuri, kio okazis kun fuzioj kaj akiradoj, kaj enrigardi la decidojn de la impostoficejo, kiujn mi devus sekvi, se mi decidus vendi iujn el tiuj akcioj.

Mi iom pli parolos en estonta artikolo pri kiel elŝuti merkatajn precdatumojn kaj kiel oni povas spuri la kapitalgajnojn de diversaj aksiaj eroj, kiujn oni eble aĉetis en malsamaj tempoj, kaj eble rigardi iujn el la raportojn, kiuj povas helpi kompreni, kie oni staras finance.


## Konkludo {#konkludo}

Tio estis amuza ekzerco, dum kiu mi lernis multe pri librotenado, kaj ankaŭ faris iujn surprizajn malkovrojn kiel ekzemple kiom da mono mi elspezas por kafo ĉiumonate! Mi sentas, ke mi estas en multe pli bona pozicio fari pli bonajn financajn decidojn, kaj estas agrable scii, ke dum la impostado mi jam havos la ŝlosilajn informojn ĉe la fingropintoj, sen devi traserĉi montojn da paperoj. Enigi iomete ĉiutage estas certe preferinde ol lasi ĉion amasiĝi por esti traktita en frenezega agado je la jarfino. Se vi volas iĝi pli bone organizita, mi sugestas enrigardi ĉi tiujn tipojn de iloj. Vi povas fari ĝin tiel simplan aŭ tiel kompleksan kiel vi volas, kaj vi certe lernos ion pri administrado de mono.
