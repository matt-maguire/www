+++
title = "La uzo de Org Mode / LaTeX por lecionaj notoj"
featuredImage = "20250105_orgmode-banner.webp"
date = 2025-01-06
lastmod = 2026-11-16T11:39:36+11:00
tags = ["Emacs", "OrgMode", "LaTeX", "Teaching", "Lingvoj", "Esperanto"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Dum la pasinta jaro mi uzas \\(\LaTeX\\) kune kun Emacs por krei lecionajn notojn por kelkaj el miaj klasoj. Unu kialo, pro kiu mi elektis Emacs anstataŭ TeX Studio kaj similaj programoj, estas ke Emacs disponigas multajn rapidvojojn kaj kompletigajn funkciojn. Tamen mi rimarkis, ke $\LaTeX$-dokumentoj foje iĝas iom multvortaj, kaj mi ekpensis pri vojoj por pli facile navigi tra la dokumentoj.

La alia afero, kun kiu mi eksperimentis pasintjare, estis la Org Mode de Emacs. Temas pri «markdown»-lingvo de pura teksto, kiu ebligas al vi strukturi viajn dokumentojn per ĉapo, kiujn oni povas malfermi kaj faldi, tiel ke vi povas rapide navigi tra dokumentoj, kaŝi nebezonatajn detalojn kaj koncentriĝi je tio, kio gravas ĉi-momente. La artikoloj en tiu ĉi bloga retejo ĉiuj estas skribitaj en Org Mode, kaj poste eksportitaj kaj publikigitaj en la reto. Mi demandis min, ĉu Org Mode ebligus al mi pli facile navigi tra la $\LaTeX$-lecionaj notoj, kiujn mi produktas, kaj generi PDF-dokumentojn por presi.


## Argumentoj favore al Org Mode {#argumentoj-favore-al-org-mode}

Iom da esplorado montris, ke Org Mode-tekstodosieroj ja estas eksporteblaj al \\(\LaTeX\\), kio poste uzeblas por generi belajn PDF-dokumentojn. Org Mode konscias pri \\(\LaTeX\\), tiel ke se vi bezonas returni al $\LaTeX$-funkcioj, kioj transpasas tion, kion la simpla Org Mode-dokumenta formato povas priskribi, vi povas facile enmeti necesajn erojn de $\LaTeX$-kodo. La funkcio Babel de Org Mode ankaŭ ebligas al vi enmeti kodon kaj enhavon produktitan de aliaj programlingvoj kiel Gnuplot, Python kaj Lisp. La simpleco de la markdown-lingvo de Org Mode, ĝia kapablo mastrumi kompleksecon per sia konturiga reĝimo kaj la fleksebleco ĉe integrigo de aliaj programlingvoj formas konvinkan argumentaron por krei matematikan enhavon.

Alia eblo, kiun mi rigardis, estis [Typst](https://typst.app/), pli nova markdown-bazita solvo por skribi teknikajn dokumentojn. La baza markdown-kompililo estas malfermfonta kaj instaleblas senpage sur via komputilo, sed la plena interreta servo estas komerca oferto. Kvankam la markdown-lingvo aspektas agrable, la ilo ne estas tiom matura kiom $\LaTeX$/TiKZ, kaj ekzistus ia lernkurbo por ekkoni [CetZ](https://github.com/johannes-wolf/cetz), kio estas la respondo de Typst al la grafika pako [TiKZ](https://tikz.net/) de \\(\LaTeX\\). Ĉar mi jam uzas \\(\LaTeX\\) kaj Org Mode en apartaj sferoj de mia laboro, mi decidis, ke estas sensence kombini ĉi tiujn du ilojn.


## Agordi Org Mode por uzi la klason _exesheet_ de \\(\LaTeX\\) {#agordi-org-mode-por-uzi-la-klason-exesheet-de-latex}

La $\LaTeX$-dokumentoj, kiujn mi uzas, baziĝas sur la $\LaTeX$-pako [exesheet](https://ctan.org/pkg/exesheet). Ĝi ebligas al mi facile kompostigi grupojn da problemoj en unukolumna aŭ plulkolumna reĝimo. Defaŭlte Org Mode uzas la $\LaTeX$-dokumentan klason _article_, do la unua paŝo estis ekscii, ĉu mi povus igi Org Mode uzi la klason _exesheet_ kaj ĝiajn rilatajn funkciojn. La alproksimiĝo, kiun mi uzis, estas priskribita en la sekvanta utila video:

{{< youtube 0qHloGTT8XE >}}

La kreinto de tiu video afiŝis la rilatajn rimedojn [ĉi tie](https://jakebox.github.io/youtube/org_latex_video.html).

Laŭ tiu video, la unua paŝo estis agordi propran klason, por ke Org Mode rekonu la pakon _exesheet_. Tial mi aldonis la sekvutan kodon al mia dosiero _config.el_ de Doom Emacs:

```elisp
(after! ox-latex
(add-to-list 'org-latex-classes
             '("org-exesheet"
               "\\documentclass{exesheet}
           [NO-DEFAULT-PACKAGES]
           [PACKAGES]
           [EXTRA]"
               ("\\section{%s}" . "\\section*{%s}")
               ("\\subsection{%s}" . "\\subsection*{%s}")
               ("\\subsubsection{%s}" . "\\subsubsection*{%s}")
               ("\\paragraph{%s}" . "\\paragraph*{%s}")
               ("\\subparagraph{%s}" . "\\subparagraph*{%s}"))))

(after! ox
  (require 'ox-extra)
  (ox-extras-activate '(ignore-headlines)))
```

La makrooj `after!` certigas, ke Doom Emacs rulas la agordan kodon ĉe la ĝusta momento dum la starto. La unua difinas la propran dokumentan klason, kiu referencas al la $\LaTeX$-pako exesheet. La dua ebligas etenditan funkcion, kiu permesas al mi uzi Org Mode-ĉapon sole por strukturi mian dokumenton kaj kaŝi detalojn, sen krei respondan ĉapon de sekcio en la fina dokumento.

La sekvanta paŝo estis krei agordan dosieron, kiu enhavas komunan antaŭaron por deklari pakojn kaj difini makroojn, kiuj estu haveblaj al ĉiuj miaj dokumentoj de lecionaj notoj. Mi kreis dosieron nomitan `lesson-preamble.org`. Jen kelkaj elstaraĵoj el tiu dosiero:

```nil
#+LATEX_CLASS: org-exesheet
#+LATEX_CLASS_OPTIONS: [12pt,a4paper,marginwidth=unset]
#+OPTIONS: toc:nil
#+LATEX_HEADER: \usepackage{amssymb} % Access to extra math symbols
#+LATEX_HEADER: \usepackage{amsmath} % Access to extra math symbols
#+LATEX_HEADER: \usepackage{diffcoeff}
...
```

La unua linio referencas al la propra klaso, kiun mi difinis en mia Emacs-agordo `config.el`, kaj la dua linio agordas la paĝajn agordojn (ekz. en Aŭstralio ni kutime uzas A4-formatan paperon). La tria linio malaktivigas la generadon de tabelo de enhavo, kaj poste mi listigas la $\LaTeX$-pako, sur kiujn miaj lecionaj notoj kutime apogiĝas.

Post la pako, mi sekve havas kelkajn makrodifinojn:

```nil
#+LATEX_HEADER: \geometry{margin=1.5cm}
#+LATEX_HEADER: \newcommand{\cloze}[1]{\underline{\hspace*{#1}}}
#+LATEX_HEADER: \newcommand{\notes}[3][\empty]{%
#+LATEX_HEADER:     \noindent\ifthenelse{\equal{#1}{\empty}}
#+LATEX_HEADER:             {\\}
#+LATEX_HEADER:             {\vspace{#1}\\}
#+LATEX_HEADER:     \foreach \n in {1,...,#2}{%
#+LATEX_HEADER:         \ifthenelse{\equal{#1}{\empty}}
#+LATEX_HEADER:             {\rule{#3}{0.5pt}\\}
#+LATEX_HEADER:             {\rule{#3}{0.5pt}\vspace{#1}\\}
#+LATEX_HEADER:         }
#+LATEX_HEADER: }
```

-   Mi agordas la marĝenojn por pli bone uzi la haveblan spacon sur la taskfolio (linio 1).
-   La makroo `cloze` (linio 3) ebligas al mi facile krei substrekitan blankan spacon, kiun la studentoj plenigu dum la leciono.
-   La makroo `notes` (linioj 5-14) ebligas al mi krei aron da linioj, sur kiuj la studentoj skribu siajn laborpaŝojn. Mi kutime ne tre uzas ĝin, preferante simple lasi iom da blanka spaco, ke la studentoj desegnu kaj skribu laŭplaĉe.


## Ni faru lecionon! {#ni-faru-lecionon}

Nun kiam ni faris la agordadon, ni kunmetu iujn lecionajn notojn. La unua afero estas krei dosieron finiĝantan per `.org` kaj referenci al la agorda dosiero, kiun ni ĵus kreis:

```nil
#+SETUPFILE: lesson-preamble.org
```


### Titolo kaj celoj de la leciono {#titolo-kaj-celoj-de-la-leciono}

Nun ni kreos Org-sekcion por montri la titolon de la leciono kaj la lernoceloj:

```org
* Header :ignore:
#+latex: \begin{center}\textsc{
  Y12 Advanced Topic 1: Differential Calculus \\
  Lesson 1: Introduction to Differentiation
#+latex: }\end{center}\begin{small}\begin{singlespacing}\begin{tcolorbox}[fonttitle=\bfseries,title=\textbf{Learning intentions:}]

Students will:
#+attr_latex: :options [itemsep=0pt]
- Revise the foundations of Indices, including index laws involving negative indices
- Revise fractional indices.

/Textbook Reference: Y11 Canbridge Advanced Ex. 7A, 7B/
#+latex: \end{tcolorbox}\end{singlespacing}\end{small}
```

-   En linio 1 mi uzas la etikedon `:ignore:` por malebligi la kreadon de $\LaTeX$-ĉapo `\section`. Kvankam Org Mode-ĉapo kutime kreas sekciojn kaj subsekciojn en via dokumento, ĉi tiu aparta Org Mode-ĉapo estas tie nur por organizaj celoj, tiel ke mi povas faldi kaj kaŝi ĉi tiun komencan parton de la dokumento kaj pli facile navigi rekte al la sekcio(j), kun kiuj mi nun laboras.

-   Linioj 2 kaj 5 eligas iom da $\LaTeX$-kodo por ke la ĉapo en linioj 3 kaj 4 esti presita per malgrandaj verskletoj kaj bele centre. Ĝi ankaŭ agordas belan $\LaTeX$-`tcolorbox`-on por emfazi la celojn de la leciono. La `#+latex:` ebligas al vi enmeti arbitrajn liniojn de $\LaTeX$-kodo en la finan dokumenton, se vi volas atingi efikojn, kiujn ne facile realigas la normaj Org Mode-strukturoj.

    Fakte, la `tex and latex output logo math modetex and latex output logo math mode#+latex:` ne estas bezonata, ĉar se Org Mode detektas iun $\LaTeX$-kodin en via dokumento, ĝi devus aŭtomate trakti ĝin. Tamen en mia Emacs-etoso la `#` ĉe la komenco kaŭzas, ke la linio estas montrata per malhelgriza tiparo, tiel ke ĝi malhelas en la fonon kaj ne tiom distragas de la «vera» enhavo.

-   Sekve, en linioj 7-10, ni difinas la lernocelojn. Ni kreas liston en Org Mode simple per buleita signo kiel `-`, `+` aŭ `*` (la lasta bezonas antaŭan blankan spacon, por ke ĝi ne estu interpretata kiel Org Mode-ĉapo). En linio 8 vi vidas la direktivon `#+attr_latex:`, kiu ebligas al vi modifi la $\LaTeX$-medion, kiun sekvas — en ĉi tiu kaso buleitan liston `itemize`. En ĉi tiu ekzemplo mi malpliigas la spacigon inter la eroj de la buleita listo por fari la liston pli kompakta. Ĉi tiun teknikon povas uzi por ĝustigi la aspekton de aliaj medioj tra la tuta dokumento.

-   En linio 12 mi donas lernolibran referencon, kiun mi kutime emfazas kursive, kio en Org Mode atingatas per ĉirkaŭigo de la teksto per `/`-simboloj (multe pli bele ol skribi `\emph{italic text}`, ĉu ne?).


### Varmiĝo / Faru nun {#varmiĝo-faru-nun}

Plej multaj instruistoj komprenas la avantaĝon de donado al la studentoj de io, per kio komenci, tuj kiam ili sidiĝas — la tiel nomata aktiveco «Faru nun». Ĝi ne nur helpas al la studentoj transiri en la lecionon; ĝi ankaŭ ebligas iom da «forma takso» por ekscii, kiom bone la studentoj komprenas la antaŭe traktitan materialon.

Mi ŝatas meti la aktivecon «Faru nun» sur la unuan paĝon en skatolo. Do, ni skribu ĉapon kaj donu al la studentoj iujn varmiĝajn ekzercojn farendajn.

```org
* Warm-Up
#+attr_latex: :options [colback=white]
#+begin_tcolorbox
1. The inverse operation to *differentiation* is \cloze{3cm}
   \tcbline

2. For each of the following equations, find $\diff{y}{x}$

   #+attr_latex: :options [itemsep=2.8cm,after=\vspace{2cm}]
   1. $y=x^2$
   1. $y=\sin x$
   \tcbline

3. Differentiate the following:

   #+attr_latex: :environment tablenuma :options [after-item-skip=2.8cm,after-skip=2cm](2)
   1. $y=\cos(x)$
   2. $f(x)=\tan x$
   3. $g=\frac{1}{x^2}$
   4. $y=e^{\sin x}$
   #+latex: ~
#+end_tcolorbox
```

-   Linio 1 enhavas Org Mode-ĉapon, kiu kreos novan sekcion en la dokumento (ĉar ne estas etikedo `:ignore:`).
-   Poste mi kreas `tcolorbox`-on ĉirkaŭ la varmiĝajn ekzercojn, ne tajpante $\LaTeX$-kodon rekte, sed ĉirkaŭigante la enhavon per paro `#+begin_tcolorbox ... #+end_tcolorbox`. Jen ĝenerala tekniko, kiun vi povas uzi por enmeti $\LaTeX$-medion en vian dokumenton, tajpante `#+begin_<environment>/ ... #+end_<environment>`, kie `<environment>` estas la nomo de la $\LaTeX$-medio, kiun vi volas uzi.

    Defaŭlte `tcolorbox` havas grizan fonon. Por forigi tion, vi devas provizi iujn opciojn al la medio. Tio kompreneble fariĝas per la antaŭe menciita direktivo `#+attr_latex:`. Vi povas vidi, ke linio 2 sendas al la `tcolorbox` opcion fari ĝian fonon blanka.

-   En linio 4 mi starigas la unuan demandon uzante kutiman numeritan liston. Mi uzas la makroon `\cloze{}`, kiun mi difinis antaŭe, por doni al la studentoj substrekitan skribospacon por ĝin plenigi — simple specifiku la longon de la linio en la kurvaj krampoj.

-   Linio 7 enhavas la sekvutan demandon, kiu konsistas el kelkaj subdemandoj. Se mi volas skribi iun enlinian matematikan notacion, mi ĉirkaŭigas ĝin per `$ ... $`-simboloj samkiel en ordinary \\(\LaTeX\\). Se via teksto enhavas valuton, tio povas kaŭzi problemojn, do alia eblo estas uzi `\( ... \)`.

    Por disigi ĉi tiun demandon de la sekvanta, mi metas `\tcbline`-disigilon (kiun vi povas laŭvole antaŭigi per `#+latex:`-direktivo). Gravas enŝovi ĝin ĝustece, por ke Org Mode ne opiniiu, ke jen la fino de la numerita listo.

-   Poste mi volas listigi la subpartojn de la demando. Mi komencas novan numeritan liston, kiu estas enŝovita. Ĉi tie mi uzis `1.` por indiki listan eron, sed vere ne gravas, kiun numeron vi skribas ĉi tie. En la eligo, la enŝovitaj eroj estos skribitaj per literoj (a), (b), (c) ktp. Eble vi rimarkas, ke mi eĉ duobligis la numeron `1.` — ne gravas, \\(\LaTeX\\) prizorgos la ĝustan numeradon.

    La studentoj bezonos spacon por skribi siajn respondojn, do ĉe la komenco de la listo mi inkluzivas direktivon `#+attr_latex:`, kie mi agordas la spacigon inter la listaj eroj per `itemsep=2cm`.

-   Por la lasta demando, mi montras al vi specialan listan medion, kiun provizas la pako `exesheet`: la liston `tablenuma`. Ĝi ebligas al vi aranĝi viajn problemojn per pluraj kolumnoj, bonege taŭga por multaj mallongaj demandoj, kiuj ne bezonas okupi tutan linion.

    Denove mi uzas numeritan liston de Org Mode, sed mi devas diri al Org Mode uzi la medion `tablenuma` per la parametro `:environment tablenuma`. Denove mi volas doni al la studentoj iom da skribospaco. Tamen ĉi tiu medio ne uzas `itemsep`; anstataŭe ĝi uzas `after-item-skip` por meti spacon inter la eroj kaj `after-skip` por meti spacon ĉe la fino de la listo.

    Unu problemo kun `after-skip` estas ke, se vi ne daŭrigas per normala alineo, la spaco ĉe la fino estas «englutita» de \\(\LaTeX\\). Truko, kiun vi povas uzi, estas meti nevideblan blankan spacon ĉe la fino per ~#+latex: ~, kiel mi faris en linio 21.


### Korpo de la leciono {#korpo-de-la-leciono}

Nun ni povas aliri al la kerno de la leciono.

```org
\newpage

* Differentiation Rules
To differentiate a combination of functions, we have a number of rules.

** Product Rule
To differentiate the product of functions $y=u(x) \times v(x)$, use the product rule:
\[y = u'v + uv'\]

\exe Differentiate $y=x \sin x$
\vspace{3cm}

* Gradients
To calculate a gradient:

\begin{tikzpicture}
  \begin{axis}[
    minor tick num=0,
    axis x line=middle, axis y line=middle,
    xlabel=$x$,ylabel=$y$,
    xmin=-2, xmax=5, ymin=-2, ymax=5,
    xtick=\empty,
    extra x ticks={1,3},
    extra x tick labels={$x_1$,$x_2$},
    ytick=\empty,
    extra y ticks={1.8,3.4},
    extra y tick labels={$y_1$,$y_2$},
    ]
    \addplot [mark=none,domain=-0.5:4] {1+0.8*x};
    \addplot [mark=*] plot [
    error bars/.cd,
    y dir=minus,y fixed relative=1,
    x dir=minus, x fixed relative=1,
    error bar style={dotted},
    ] coordinates {(1,1.8) (3,3.4)};
    \draw (axis cs:3,3.4) |- (axis cs:1,1.8) node at (axis cs:2,1.5) {$x_2-x_1$} node at (axis cs:3.8,2.6) {$y_2-y_1$};
  \end{axis}
\end{tikzpicture}

#+attr_latex: :options [fonttitle=\bfseries,title={Gradient Formula}]
#+begin_tcolorbox
To find the gradient:
  \begin{align*}
    \text{Gradient} &= \frac{\text{rise}}{\text{run}} \\
                    &= \frac{y_2-y_1}{x_2-x_1}
  \end{align*}
#+end_tcolorbox

\exe Find the gradient of the line segment joining the points $P(1,2)$ and $Q(3,4)$
\vspace{1cm}
```

Ĉi tie vi vidas, ke la Org Mode-ĉapo estas tradukataj al la diversaj sekcioj kaj subsekcioj, kaj ni povas «faldi for» ajnajn detalojn, kiujn ni ne volas vidi.

-   Ni komencas sur nova paĝo uzante `\newpage`

-   Ni uzas Org Mode-ĉapo por aldoni novajn sekciojn/subsekciojn de la leciono, kaj ni povas uzi la ciklan ŝanĝadon de videbleco de Org Mode (la klavo `<TAB>`) por kaŝi sekciojn.

-   Mi uzis la komandon de `exesheet` nome "`\exe`" por enkonduki ekzemplon, kiun mi volas solvi paŝon post paŝo kun la studentoj. Defaŭlte la numerado de la `\exe`-ekzemploj/ekzercoj estas tutmonda tra la tuta dokumento.

-   Mi enmetis TikZ-diagramon, nur por montri kiel vi povas libere enmeti $\LaTeX$-kodon laŭbezone.


### Doni klaslaboron/hejmtaskojn {#doni-klaslaboron-hejmtaskojn}

Fine, eble vi volas doni al la studentoj iujn ekzercojn el la lernolibro.

```org
* Classwork/Homework
#+begin_tcolorbox
Year 12 Awesome Maths Textbook
- Ex. 13A Q2-11, 14, 15.
#+end_tcolorbox
```


## Eksporto al PDF {#eksporto-al-pdf}

Nun kiam ni skribis niajn lecionajn notojn, ni povas eksporti al LaTeX/PDF. Por krei PDF-dosieron, mi tajpus ion kiel `C-c C-e l p`, kaj mi ricevos PDF-an, kiu aspektos ie jene.

{{< embed-pdf url="20250105_example-lesson.pdf" >}}


## Finaj pensoj {#finaj-pensoj}

La ekzemploj, kiujn mi donis, estis riĉaj je LaTeX-kodo, ĉar mi volis montri la specojn de aferoj, kiuj eblas dum eksportado el Org Mode. Entute, la faldaj ĉapo en Org Mode faciligas orientiĝon en komplika dokumento, kaj eblas aldoni eĉ pli da Org Mode-ĉapo kun la etikedo `:ignore:`, se vi volas fari iujn partojn de la dokumento faldaj.

Mi ne montris iujn ekzemplojn de tabeloj. Org Mode havas potencan tabelredaktilon, kio tre faciligas la produktadon kaj mastrumadon de tabeloj, kaj se vi volas laŭigi la aspekton de tabelo, vi povas uzi la direktivon `#+attr_latex:` por diri al Org Mode uzi la potencon de la pako `tblr`, pri kiu ĉiuj $\LaTeX$-uzantoj devus lerni, se ili ankoraŭ ne faris.

Unu afero, kiun mi ŝatus plu esplori, estas la enmetado de kodo por generi enhavon — ekzemple por produkti tabelojn de $z$-valoroj programme.
