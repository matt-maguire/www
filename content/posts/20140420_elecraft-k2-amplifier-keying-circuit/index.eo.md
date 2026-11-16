+++
title = "Ŝlosada cirkvito por la Elecraft K2 amplifilo"
featuredImage = "20141420-IMG_0118-Version-2-768x1024-1.jpg"
date = 2014-04-20
lastmod = 2026-11-16T11:30:14+11:00
tags = ["HamRadio", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Mi konstruis [radiokonstruilaron Elecraft K2](http://www.elecraft.com/k2_page.htm), kaj mi finis la konstruadon la pasintan Paskofinsemajnon.

Kiam mi mendis la konstruilaron, mi nemendis la enan ATU nek la opcion de 100 W amplifilo, ĉar mi jam havas [amplifilon Elecraft KXPA100 kun ATU KXAT100](http://www.elecraft.com/KXPA100/kxpa100.htm). Tamen tiu amplifilo estas dezajnita por funkcii kun la radio [KX3](http://www.elecraft.com/KX3/kx3.htm), kaj do por ke ĝi funkcias kun la K2, mi bezonus konstrui ŝlosadan cirkviton.

Bonŝance, Elecraft publikigis en sia retejo [elegantan solvon](http://www.elecraft.com/Apps/Amp_keying_ckt.htm) por tio. Ĝi konsistas el kelkaj transistoroj montitaj sur prova panelo, kiu enŝoviĝas en PCB-soketon de pinlorango destinitan por la interfaca modulo de la transporter de la K2. Ĉar mi ne havas tiun opcion, mia radio ne havis la bezonatan kablansoketon, sed bonŝance la loka elektronika butiko vendas taŭgajn pinlorangojn kiel parton de ilia Arduino-a rango. Mi aĉetis unu el tiuj pinlorangoj, kune kun la aliaj bezonataj partoj.

La artikolo en la retejo de Elecraft provizas skemon kaj kelkajn fotojn de la finita panelo, sed ili ne provizis detalan informon pri kiel konstrui la cirkviton sur la prova panelo. Bonŝance, la cirkvito estas sufiĉe simpla kaj ne tre malfacile eltrovebla, sed mi decidis desegni diagramon, kiu donas pli bonan vidon pri kiel la komponentoj estas aranĝitaj kaj kiel la fizikaj konektoj estas faritaj.

{{< figure src="20141420-key_cct1.png" >}}

La punktitaj linioj montras konektojn irantajn sub la panelo (krom la pordo-kruro de la 2N7000 JFET — tiu iras sub la transistoro, sed laŭ la supro de la panelo).

Mi konstruis la cirkviton laŭ la sekvanta ordo:

1.  Metu la 8-pingan virinan pinlorangon sub la panelon ĉe la maldekstra flanko, kaj litu unu kruron de la 2,2 kΩ rezistilo al la pinglo elstaranta supren el la panelo ĉe pozicio A8. Tio tenas la pinlorangon enplace. La alia kruro de la 2,2 kΩ rezistilo iras tra truon B5.

2.  Metu la 10 kΩ rezistilon starantan verticale en truon C5, fleksu la supran kruron kaj trapiku ĝin tra truon D4. Fleksu la krurojn sub la panelon por teni la rezistilon enplace.

3.  Enmetu pecon de konekta drato (ekz. fortranĉitan komponentan kruron el via konstruado de la K2) tra truon D3. Supre de la panelo fleksu la draton tiel, ke ĝi atingu la pinglon 2 de la pinlorango elstarantan tra truon A2, kaj litu ĝin al tiu pinglo.

4.  Montu la dekstra-angulan Molex-ingon supren sur la panelon tra truoj E1 kaj F1. Sub la panelo fleksu la konektan draton, kiun vi enmetis en la antaŭa paŝo, tiel ke ĝi atingu la pinglon de la ingo elstarantan tra truon F1, kaj litu ĝin al la inga pinglo kaj tondu. Tio tenos la ingon enplace.

5.  La pordo- kaj fontokruroj de la 2N7000 JFET estas la du maldekstraj kruroj, se vi tenas la transistoron kun la plata flanko al vi. Fleksu tiujn du krurojn malantaŭen, kaj metu la restan kruron (la drenon) en truon C1. Fleksu la drenan kruron sub la panelon malsupren al la rezistiloj por teni la transistoron enplace.

6.  Litu la fontokruron (la maldekstran, kiun vi fleksis malantaŭen) al la pinglo 2 de la pinlorango, kiu elstaras el truo A2, kaj tondu. Atentu, ke vi ne ŝovu la konektan draton, kiu ankaŭ estas lita al tiu pinglo. Tondu kaj la transistoran kruron kaj la konektan draton.

7.  Litu la pordokruron (la mezan, kiun vi fleksis malantaŭen) al la pinglo 1 de la pinlorango, kiu elstaras el truo A1, kaj tondu.

8.  La drenon de la JFET, kiu iras malsupren tra truon C1, kaj la rezistilkrurojn elstarantajn tra truoj B5 kaj C5, oni devas fleksi tiel, ke ili ĉiuj renkontiĝu. Litu la tri krurojn kune kaj tondu.

9.  La emiterkruro de la 2N2222 BJT estas la plej maldekstra kruro, se vi tenas la transistoron kun la plata flanko al vi. Fleksu tiun kruron malantaŭen, kaj metu la mezan kruron (la bazan) tra truon C4 kaj la plej dekstran kruron (la kolektoron) tra truon C3. Litu la emiteran kruron, kiun vi fleksis malantaŭen, al la pinglo 4 de la pinlorango elstaranta tra truon A4.

10. Fleksu la kolektoran kruron elstarantan tra truon C3 tiel, ke ĝi atingu sub la panelon la pinglon de la Molex-ingo elstarantan tra truon E1, kaj litu la kruron al la inga pinglo.

11. Fleksu la bazan kruron elstarantan tra truon C4 tiel, ke sub la panelo ĝi renkontu la kruron de la 10 kΩ rezistilo elstarantan tra truon D4, litu la du krurojn kune kaj tondu.

12. Faru mallongan kablon (3" aŭ 4") kun la Molex-ŝtopilo sur unu fino. Litu la flankon de la kablo, kiu konektiĝas al la pinglo plej proksima al la flanko de la panelo, iranta tra truon F1, al la masa konektilo de la RCA-konektilo.

13. Instalu la RCA-konektilon en la truon sur la malsupra ĉasio markita "XVTR IN". Litu la outran flankon de la Molex-ŝtopila kablo al la interna konduktilo de la RCA-konektilo.

14. Enŝovu la ŝlosadan cirkvitpanelon en la soketon J13 sur la RF-panelo. Konektu la Molex-ŝtopilon al la Molex-ingo sur la ŝlosada cirkvitpanelo.

Kaj jen. Enŝovu unu finon de la RCA-RCA kablo en la novan RCA-konektilon ĉe la malantaŭo de via K2, kaj enŝovu la alian finon de la kablo en la RCA-soketon "PA KEY" ĉe la malantaŭo de la KXPA100. Konektu konektan kablon inter la ANT OUT de la K2 kaj la RF IN de la KXPA100, kaj vi estas preta.

Jen kelkaj fotoj de la finita cirkvito, de supre kaj de malsupre.

{{< figure src="20141420-IMG_0113-1024x768-1.jpg" >}}

{{< figure src="20140420-IMG_0114-1024x768-1.jpg" >}}

{{< figure src="20140420-IMG_0118-Version-2-768x1024-1.jpg" >}}
