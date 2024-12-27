+++
title = "Programming Languages"
date = 2024-12-28
lastmod = 2024-12-28T04:34:43+11:00
tags = ["Programming", "LISP"]
categories = ["Docs"]
draft = false
weight = 2002
+++

# LISPs {#lisps}


## Scheme {#scheme}

Scheme is a variant of LISP that was famously used at MIT to teach students about functional programming concepts. It is also used in the GNU Guile Scheme as an extension language.

Some resources are:

-   [SCIP programming course](https://ocw.mit.edu/courses/6-001-structure-and-interpretation-of-computer-programs-spring-2005/video_galleries/video-lectures/) from MIT is free.


## Common Lisp {#common-lisp}

Common LISP implementations tend to be much larger than Scheme, which makes it powerful for solving practical probems, but this large feature set could be a bit of a distractor when first starting out.

-   Differences between Scheme and Lisp are descibed at <https://wiki.c2.com/?LispSchemeDifferences>.
-   SBCL (Steel Belt Common Lisp) is a good choice, particularly when combined with Emacs and SLIME. Instructions for setting it up are available at <https://github.com/rabbibotton/clog/blob/main/LEARN.md>.


## Emacs Lisp {#emacs-lisp}

This is of course a good one to learn if you use the Emacs editor. Since it is already sell integrated into the editor, no need to waste time setting up external compilers and connectors such as SLIME.

-   Elisp is well documented in the Emacs Manual and the Emacs help system. Examples:
    -   [Introduction to Programming in Elisp](https://www.gnu.org/software/emacs/manual/html_node/eintr/index.html)
    -   [Elisp Reference Manual](https://www.gnu.org/software/emacs/manual/html_node/elisp/index.html)
-   it has powerful tools such as Edebug exist, profilers, regression testing, benchmarkers, ...
-   it is single-threaded, so may not be suitable for some applications.
