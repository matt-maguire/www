+++
title = "Kiel kalkuli serian rezistilon por LED"
featuredImage = "20220202-Screen-Shot-2022-02-02-at-6.32.54-pm.png"
date = 2022-02-02
lastmod = 2026-11-16T10:40:40+11:00
tags = ["HamRadio", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Kiam ni konektas LED-on al baterio, ni ofte bezonas rezistilon en serio por limigi la korrenton. Mi vidis tiun demandon aperigi en Facebook-grupo, kaj mi pensis, ke povus esti utile uzi ĝin kiel simplan ekzemplon pri cirkvitodezajnado.

La baza cirkvito, kiun ni konsideros, estas montrita sube:

{{< figure src="20220202-quicklatex.com-06f3bedeaf7e311d9af11493d4616842_l3.svg" >}}

Ni havas korenton \\(i\\), kiu forlasas la baterion/fonton de tensio, pasas tra la rezistilo kaj kaŭzas tensiofalon \\(V\_R\\), poste pasas tra la LED kun tensiofalo \\(V\_{f\_{LED}}\\) antaŭ ol reveni al la baterio. Do, kiel ni povas eltrovi la valoron de la rezistilo, kiun ni devus uzi?

Por respondi tiun demandon, ni bezonas scii kelkajn aferojn pri la LED. Nome:

kiom da korento ni volas pasi tra la LED?
kia estos la tensio trans la LED ĉe tiu korento?

Kiel ni trovu la respondojn al tiuj demandoj? Ni devas konsulti la specifikaĵon de la LED-komponento!

Unue ni kontrolu la maksimuman toleratan valoron de la komponento. Rigardante la specifikaĵon, ni vidas la sekvantan tabelon:

{{< figure src="20220202-Screen-Shot-2022-02-02-at-5.23.35-pm-1024x420.png" >}}

Ni povas vidi, ke la maksimuma kontinua kurento por ruĝa LED estas 30 mA. Ĝenerale ni ne volas funkcigi komponenton rekte ĉe ĝia maksimuma limo, do kio okazas, se ni iom reduktas la korenton? Serĉante plu en la specifikaĵo, ni trovas la sekvantajn grafikojn:

{{< figure src="20220202-Screen-Shot-2022-02-02-at-5.24.13-pm-1024x469.png" >}}

La grafeo en figuro 3 montras al ni, kiom hela la LED estos ĉe diversaj korentoj. Ĉe 10 mA la intenso de la LED-lumo estas normigita al nivelo 1. Se ni pliigos la korenton ĝis 20 mA, la LED estos ĉirkaŭ 1,5-oble tiel hela kiel ĉe la nivelo de 10 mA. Se ni pliigos la korenton ĝis 30 mA (la maksimumo), la intenso estas nur ĉirkaŭ 1,8-oble tiel hela kiel ĉe 10 mA — do ĝi konsumas multe pli da potenco por malmulta gajno. Ĉe 20 mA ni ankoraŭ ricevas helan intencon kaj samtempe lasas konsiderindan rezervon sub la maksimumo, do ni desegnu nian cirkviton kun korento:

Kia estos la tensio trans la LED ĉe tiu korento? Rigardante la grafikon en figuro 2, ni vidas, ke la tensio trans la LED estos proksimume:

\\[V\_{f\_{LED}}=2\text{ V}\\]

Kia estos la tensio trans la rezistilo de R omoj? Ni scias, ke la korento tra la rezistilo devas esti 20 mA, do ni povas uzi la leĝon de Ohm por kalkuli la tensiofalon trans la rezistilo:

\\[\begin{align\*}
        V\_R & = i \times R \\\\
        & = 20 \times 10^{-3} \times R
        \end{align\*}\\]

Ni povas nun uzi la tensian leĝon de Kirchhoff, laŭ kiu la sumo de tensioj en buklo ĉiam egalas al 0 V. Se ni iras horloĝdirekten ĉirkaŭ la cirkvito, la tensifonto helpas la korenton kaj do estas pozitiva, dum la rezistilo kaj la LED kontraŭas la korenton, do iliaj signo estas negativaj. La ekvacio fariĝas:

\\[\begin{align\*}
        V - V\_R - V\_{f\_{LED}} & = 0 \\\\
        V - i \times R - V\_{f\_{LED}} & = 0 \\\\
        i \times R &= V - V\_{f\_{LED}} \\\\
        R & = \frac{V - V\_{f\_{LED}}}{i}
        \end{align\*}\\]

Se ni supozas, ke ni uzas 9 V baterion, do anstataŭigante la nombrojn ni ricevas:

\\[\begin{split}R & = \frac{(9 - 2)}{20 \times 10^{-3}} \\\ & = 350~\Omega\end{split}\\]

Por sekureco, ni povas suprenrondi ĝin al la plej proksima normalizita valoro, kio estus 390 Ω rezistilo kun la kolora kodo oranĝa-blanka-bruna. Denove kontrolante la korenton, ni ricevas:

\\[\begin{split} i & = \frac{V - V\_{f\_{LED}}}{R} \\\\
    & = \frac{(9 - 2)}{390} \\\\
    & = 18\text{ mA}\end{split}\\]

Tio ankoraŭ estas akceptebla. Do, ni nun finis nian cirkvitodezajnon:

{{< figure src="20220202-quicklatex.com-fecf716ddc8012b94614dba19222cab7_l3.svg" >}}
