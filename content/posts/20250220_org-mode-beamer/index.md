+++
title = "Org Mode Beamer Presentations"
date = 2025-02-20
lastmod = 2025-02-20T13:52:59+11:00
tags = ["Emacs", "OrgMode", "LaTeX", "Teaching", "Beamer"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

Earlier I talked about [using Org Mode to produce lesson worksheets]({{< relref "20250105_orgmode-lesson-notes" >}}) for my students. This year I am thinking about using powerpoint-style presentation slides instead of worksheets to encourage students to write more in their books. This naturally led me to look into how to do this with Org Mode.


## A Simple Lesson {#a-simple-lesson}

Making presentations in \\(\LaTeX\\) is normally done using the `beamer` package. It turns out that Org Mode has good support for beamer. An example of a simple lesson presentation iorg-mode file might look like this:

```org
#+title: Adding Terms
#+subtitle: Algebraic Techniques
#+author: Mr Maguire
#+startup: beamer
#+options: H:1 toc:nil
#+latex_class: beamer
#+latex_class_options: [bigger]
#+columns: %45ITEM %10BEAMER_env(Env) %10BEAMER_act(Act) %4BEAMER_col(Col) %8BEAMER_opt(Opt)
#+beamer_theme: SimplePlus
#+LATEX_HEADER: \usepackage{tasks}

* DO NOW
1. Identify the like terms:
   \[ 4ab^2, 4ab, 2a^2b^2, 3b^2a \]
   \vskip+5ex
2. Which is the constant term?
   | 3x | 4x^2 | 5x^0 | x |

* Adding Terms
To add terms:
- make sure the two terms are *like* terms
- add the coefficients

** Careful! :B_block:
Don't forget to take any minus signs into account!

* Practice
Simplify the following espressions
\begin{tasks}[after-item-skip=8ex](2)
\task $2x+3x$
\task $3a+5b+6a$
\task $a+b+2b+a$
\task $a^{2}+2ab-2ba+b^{2}$
\task* $a+2a-3a+4a-5a+6a-7a+8a-9a+10a$
\end{tasks}

* :B_ignoreheading:
#+begin_center
*What's $2n$ plus $2n$?*
\vskip+2ex
I don't know, it sounds $4n$ to me...
#+end_center
```

To export the org mode code to a PDF, we need to use a special export command to let org-mode know to make a beamer presentation rather than a normal \\(\LaTeX\\) document. On my system, the export process is bound to `C-c C-e l O` which will:

-   translate the org mode to a \\(\LaTeX\\) `*.tex` file
-   produce a PDF file from the \\(\LaTeX\\) file
-   open the PDF file in Emacs (I have pdftools installed in my Emacs config)

If I want to generate the PDF file without opening it (for example, if I already have the PDF file open in another window) then I use the key sequence `C-c C-e l P`.

This will then produce a PDF file that looks like this:

{{< embed-pdf url="20250220_beamer-lesson.pdf" >}}


## Preamble {#preamble}

So, let's break down the contents of the preamble in the org mode file:

-   The `#+title:`, `#+subtitle:`, `#+author:` and optionally `#+date:` are self-explanatory and are used to generate the title slide.
-   The `#+startup: beamer` line tells Emacs that this is a beamer org-mode file and that it should enable the special org mode beamer minor mode for syntax highlighting, etc.
-   The `#+options: H:1 toc:nil` line tells org mode that I don't want a table of contents slide, and that each level 1 heading in the org file should represent a new slide. If I put `H:2` instead, then level 1 headings would represent sections with a section heading slide, and the level 2 headings would be the actual content slides. I want to minimise that extra fluff for my students so that we can focus purely on content, and this is why I use `H:1`
-   The `#+latex_class: beamer` line uses the predefined Org Mode beamer class, and the `#+latex_class_options: [bigger]` sets a larger font size to help students make out the slides more clearly. Font size can be overridden for a particular slide using the `\fontsize{}{}` command at the start of that slide's content.
-   The `#+columns:` line enables a cool "column mode feature" in Emacs Org Mode. I don't want to discuss it here, but you can read mode about this in the [Org Mode manual](https://orgmode.org/manual/Column-View.html).
-   The `#+beamer_theme:` line sets the "look and feel" of the slides. I use a fairly simple theme to minimise cognitive load for the students. You can find other themes on the internet, such as at [this site](https://latex-beamer.com/tutorials/beamer-themes/).
-   You can then include any packages you might want to use with lines like `#+latex_header: \usepackage{}`. You can see I use the `tasks` package, which lets you easily format multi-column lists of problems like I did with the `exesheets` package in the [org mode lesson notes]({{< relref "20250105_orgmode-lesson-notes" >}}) post.


## Content {#content}

Now that we have set up the environment, we can start making some slides!

1.  The first slide I have is a "DO NOW" starter activity. The text in the level 1 heading becomes the title at the top of the slide. In the example, I have a simple enumerated list. You can see we can include maths notiation using the normal `"$ ... $"` and `"\[ ... \]"` markers. Sometimes it is better to use `"\( ... \)"` delimiters for inline maths expressions, as the dollar signs can be confused with currency symbols. If you want to write dollar signs in an expression, you can use a `\dollar` or `\USD` command.
2.  The second slide shows a bullet list. You can also use standard org-mode markup for **bold text**, _italics_, etc.. It also includes a highlighted text box, which you can create using a subheading marked with a `:B_block:` tag.
3.  The third slide makes use of the `tasks` package. The org mode beamer export filter doesn't work well with that package, so I find the easiest thing is to simply use \\(\LaTeX\\) code. You can then easily specify separation between the items with `after-item-skip` and how many columns of questions we want. Each question is introduced with a `\task` command, and if you want a longer question to span all the columns we can use `\task*` with an asterisk.
4.  The last slide shows how we can create a slide with no title. You can also use a \\(\LaTeX\\) environment such as `\begin{center} ... \end{center}` using the `#+begin_center ... #+end_center` directives. If we want to create some white space where we can write answers or annotate a solution, we can use a command like `\vskip+2ex` to create that vertical space.


## Further Reading {#further-reading}

This is a very quick introduction. If you want to learn more, I suggest you look at:

-   A org mode beamer [tutorial](https://orgmode.org/worg/exporters/beamer/tutorial.html) which was the main basis of the simple example I provided here.
-   The org mode beamer [user manual](https://orgmode.org/manual/Beamer-Export.html)
