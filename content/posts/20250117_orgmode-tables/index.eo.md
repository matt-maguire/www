+++
title = "Eksportado de Org-reĝimaj tabeloj al LaTeX"
date = 2025-01-17
lastmod = 2026-11-16T11:39:34+11:00
tags = ["Emacs", "OrgMode", "LaTeX", "Teaching", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

En mia antaŭa afiŝo pri uzado de [Org-reĝimo por lecionaj notoj]({{< relref "index.eo.md" >}}), mi menciis, ke eblas eksporti Org-reĝimajn tabelojn al \\(\LaTeX\\). Jen kelkaj avantaĝoj de Org-reĝimo kompare kun kruda \\(\LaTeX\\) kiam oni faras tabelojn:

-   Org-reĝimo havas potencon [tabelredaktilon](https://orgmode.org/manual/Built_002din-Table-Editor.html), kiu faciligas desegni tabelon kaj movi vicojn kaj kolumnojn. Org-reĝimo aŭtomate ĝustigas ĉiujn kolumnojn.
-   Ĉar la ĉeloj de Org-reĝima tabelo estas bele alĝustigitaj, multo pli facile estas vidi la tabelon sen nombradi `&`-simbolojn en kruda $\LaTeX$-kodo.
-   Org-reĝimo permesas uzi formulojn en tabelo, iom kiel kalkultabelo -- tio estas pli malfacile atingite rekte per \\(\LaTeX\\).

La tabeloj produktitaj de Org-reĝimo estas sufiĉe bazaj defaŭlte. Tamen vi povas uzi la $\LaTeX$-pakon [tabularray](https://ctan.org/pkg/tabularray) por laŭvoli la aspekton de viaj tabeloj. Interali, se vi ankoraŭ ne uzas la modernan _tabularray_-on por krei viajn $\LaTeX$-tabelojn, vi definitive uzu ĝin -- ĝi estas multo pli potenca kaj facile uzebla kompare kun la pli tradiciaj tabelaj pako (vi povas danki min poste).

Do, ni kreu simplan tabelon por desegni la funkcion \\(y=x^2\\):

```org
#+attr_latex: :environment tblr :align hlines,vlines,column{1}={gray9},column{2-Z}={r}
| $\bm x$ | $-2$ | $-1$ | 0 | 1 | 2 |
| $\bm y$ |    4 |    1 | 0 | 1 | 4 |
```

Kelkaj rimarkoj:

-   la linio `#+attr_latex:` tuj antaŭ la tabelo diras al Org-reĝimo laŭvoli la $\LaTeX$-kodon de la tabelo.
    -   La atributo `:environment` diras al Org-reĝimo uzi la tabelmedion _tblr_ de tabularray anstataŭ la defaŭlta $\LaTeX$-tabelmedio.
    -   La atributo `:align` permesas al ni specifi la parametrojn, kiujn la _tblr_-medio atendas. Vi vidas, ke mi petis horizontalajn kaj verticalajn liniojn; mi volas, ke la unua kolumno kun la nomoj de la variabloj \\(x\\) kaj \\(y\\) estu grize ombrumita, kaj ke la nombroj estu dekstre ĝustigitaj.
-   Mi uzis `$`-signojn ĉirkaŭ la nomoj de variabloj kaj la negativaj nombroj, por ke ili estu ĝuste rendrataj. Mi ankaŭ uzis `\bm` el la $\LaTeX$-[pako bm](https://ctan.org/pkg/bm) por ke la nomoj de variabloj estu dikaj.

Tio produktos tabelon, kiu aspektas tiel:

{{< figure src="20250117_table1.png" >}}

Iufoje vi volas lasi certajn ĉelojn de la tabelo malplenaj kaj lasi sufiĉe da spaco por ke lernantoj skribu siajn proprajn valorojn en la tabelon. Estas facile fari tion per la _tblr_-medio:

```org
#+attr_latex: :environment tblr
#+attr_latex: :align hlines,vlines,row{2}={15mm},column{1}={gray9},column{2-Z}={20mm,c}
| $\bm x$ | $-2$ | $-1$ | 0 | 1 | 2 |
| $\bm y$ |      |      |   |   |   |
```

-   ĉi tie mi metis la atributojn `:environment` kaj `:align` sur apartaj linioj, nur por montri, ke tio estas eblo.
-   Mi specifis, ke la dua vico estu 15 mm alta
-   Mi specifis, ke la kolumnoj de \\(2\\) ĝis \\(Z\\) estu $20$\\,mm larĝaj. La \\(Z\\) havas specialan signifon dum specicado de vicoj kaj kolumnoj en _tblr_-medio -- ĝi referencas la lastan vicon aŭ kolumnon. Tio signifas, ke mi ne devas ĝustigi la intervalan specifon, se mi aldonas aŭ forigas kolumnojn.

Se ni poste eksportas al PDF, la tabelo aspektos tiel:

{{< figure src="20250117_table2.png" >}}

Kiel mi diris antaŭe, _tblr_ estas oportuna moderna maniero specifi $\LaTeX$-tabelojn, kaj ĝi estas tre potenca pako. Por pli da informoj pri tio, kion ĝi povas fari, rigardu la [dokumentaron](https://mirror.aarnet.edu.au/pub/CTAN/macros/latex/contrib/tabularray/tabularray.pdf) de _tabularray_.
