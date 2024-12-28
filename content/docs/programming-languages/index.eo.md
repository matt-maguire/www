+++
title = "Programadaj Lingvoj"
date = 2024-12-28
lastmod = 2024-12-28T11:13:06+11:00
tags = ["Programado", "LISP"]
categories = ["Docs"]
draft = false
weight = 3002
+++

# LISPs {#lisps}


## Scheme {#scheme}

Scheme estas varianto de LISP kiu estas fame uzita ĉe MIT por instruii studentojn pri konceptoj de funkcieca programado. Ĝi ankaŭ estas uzata en la GNU Guile etenda lingvo.

Kelkaj rimedoj estas:

-   [Strukturo kaj Interpretado de Komputilaj Programoj](https://media.githubusercontent.com/media/sarabander/sicp-pdf/master/sicp.pdf) (SIKP) libro.
-   [SIKP programada kurso](https://ocw.mit.edu/courses/6-001-structure-and-interpretation-of-computer-programs-spring-2005/video_galleries/video-lectures/) de MIT estas senpaga.
-   Por lerni GNU Guile varianto de Scheme: <https://www.gnu.org/software/guile/learn/>


## Komuna Lisp {#komuna-lisp}

Komuna LISP efektivigoj ofte estas pli grandaj ol Scheme, kiu igas ĝin pli potenca por solvi praktikajn problemojn, sed tiu granda funkciaro povas esti iom de distro kiam oni unue komencas.

-   Diferencoj inter Scheme kaj Lisp estas priskribitaj ĉe <https://wiki.c2.com/?LispSchemeDifferences>.
-   SBKL (Steel Belt Komuna Lisp) estas bona elekto, precipe kiam uzate kun Emacs kaj SLIME. Instruktoj por starigi ĝin disponas ĉe <https://github.com/rabbibotton/clog/blob/main/LEARN.md>.


## Emacs Lisp {#emacs-lisp}

Tiu kompreneble estas bona por lerni se oni jam uzas la Emacs redaktilo. Pro tio ke ĝi jam estas bone integrita en la redaktilon, ne necesas malŝpari tempon starigi eksternajn kompililojn kaj konektilojn kiel SLIME.

-   Elisp estas bone dokumentita en la Emacs manlibro kaj la Emacs helpa sistemo. Ekzemploj:
    -   [Enkonduko al Programado en Elisp](https://www.gnu.org/software/emacs/manual/html_node/eintr/index.html)
    -   [Elisp Referenca Manlibro](https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html)
-   Ĝi posedas potencaj iloj kiaj Edebug, profililoj, regressa testiloj, komparmarkiloj, ktp.
-   Ĝi estas unu-fadena, do pro tio povas esti malkonvena por kelkaj aplikaĵoj.
