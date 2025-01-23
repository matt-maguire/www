+++
title = "Using Org Mode / LaTeX for Lesson Notes"
featuredImage = "20250105_orgmode-banner.webp"
date = 2025-01-06
lastmod = 2025-01-23T14:43:46+11:00
tags = ["Emacs", "OrgMode", "LaTeX", "Teaching"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

Over the past year, I have been using \\(\LaTeX\\) together with Emacs to produce lesson notes for some of my classes. One reason I went with Emacs instead of TeX Studio and the like was that Emacs provided a lot of shortcuts and completion features. I was finding though that \\(\LaTeX\\) documents can sometimes become a bit verbose, which made me wonder about ways to make it easier to navigate the documents.

The other thing I was experimenting with last year was Emacs' Org Mode. This is a text "markdown" language that lets you structure your documents with headings that can be expanded and collapsed, so that you can quickly navigate documents, hide unneeded detail and focus on what's important in the here-and-now. The articles in this blog website are all written in Org Mode, and are then exported and pushed to the web. I was wondering whether Org Mode might allow me to more easily navigate the \\(\LaTeX\\) lesson notes that I have been producing and generate PDF documents for printing.


## The Case for Org Mode {#the-case-for-org-mode}

A bit of research revealed that Org Mode text files can indeed be exported to \\(\LaTeX\\) that can then be used to generate nice PDF documents. Org Mode is $\LaTeX$-aware, so that if you need to fall back on \\(\LaTeX\\) features that go beyond what the simple Org Mode document format can handle, you can easily include necessary snippets of \\(\LaTeX\\) code. The Babel feature of Org Mode also allows you to embed code and content produced by other programming languages such as Gnuplot, Python and Lisp. The simplicity of Org Mode's markdown language, its ability to manage complexity through its outlining mode and the flexibility in integrating other programming languages makes for a compelling case in producing maths-based content.

Another option I looked at was [Typst](https://typst.app/), a newer markdown-based solution for writing technical documnents. The basic markdown compiler is open-source and can be installed for free on your computer, but the full web-based service is a commercial offering. While the markdown language looks nice, the tool is not as mature as $\LaTeX$/TiKZ, and there would be something of a learning curve to get across [CetZ](https://github.com/johannes-wolf/cetz), which is Typst's answer to \\(\LaTeX\\)'s [TiKZ](https://tikz.net/) graphics package. Given that I am already using \\(\LaTeX\\) and Org Mode in spearate spheres of my work, I decided that it makes sense to combine these two tools.


## Setting Up Org Mode to Use \\(\LaTeX\\)'s _exesheet_ class {#setting-up-org-mode-to-use-latex-s-exesheet-class}

The \\(\LaTeX\\) documents I have been using are based on the \\(\LaTeX\\) [exesheet](https://ctan.org/pkg/exesheet) package. This allows me to easily typeset groups of problems either in single or multi-column mode. By default Org Mode uses the \\(\LaTeX\\) _article_ document class, so the first step was to see if I could get Org Mode to use the _exesheet_ class and its associated features. The approach I used is described in the following useful video:

{{< youtube 0qHloGTT8XE >}}

The creator of that video posted the the associated resources [here](https://jakebox.github.io/youtube/org_latex_video.html).

According to that video, the first step was to set up a custom class so Org Mode could recognise the _exesheet_ package. I therefore added the following code to my Doom Emacs _config.el_ file:

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

The `after!` macros make sure that Doom emacs runs the configuration code at the appropriate time during start-up. The first one defines the custom document class that refers to the \\(\LaTeX\\) exesheet package. The second one enables an extended feature that allows me use an Org Mode heading purely to structure my document and hide detail without creating a corresponding section heading in the final document.

The next step was to create a set-up file that includes a common set of preamble to declare packages and define macros that should be available to all my lesson note documents. I created a file called `lesson-preamble.org`. Some highlights from that file:

```nil
#+LATEX_CLASS: org-exesheet
#+LATEX_CLASS_OPTIONS: [12pt,a4paper,marginwidth=unset]
#+OPTIONS: toc:nil
#+LATEX_HEADER: \usepackage{amssymb} % Access to extra math symbols
#+LATEX_HEADER: \usepackage{amsmath} % Access to extra math symbols
#+LATEX_HEADER: \usepackage{diffcoeff}
...
```

The first line refers to the custom class I defined in my `config.el` Emacs configuration, and the second line sets up the page settings (eg. In Australia we normally use A4 size paper). The third line disables the generation of a table of contents, and then I list out the \\(\LaTeX\\) packages that my lesson notes typically rely on.

After the packages, I then have some macro definitions:

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

-   I set the margins to make better use of the available space on the worksheet (line 1).
-   The `cloze` macro (line 3) lets me easily create an underlined blank space for students to fill in during the lesson.
-   The `notes` macro (lines 5-14) allows me to create a set of lines for students to write their working out. I tend not to use this much, preferring to just leave some blank space for students to draw and write in as they wish.


## Let's Make a Lesson! {#let-s-make-a-lesson}

Now that we've done the set-up, let's put together some lesson notes. The first thing is to create a file ending in `.org` and refer to the setup file we just made:

```nil
#+SETUPFILE: lesson-preamble.org
```


### Title and Lesson Intentions {#title-and-lesson-intentions}

We will now create a Org section to display the lesson title and learning intentions:

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

-   In line 1, I use the tag `:ignore:` to supress the creation of a \\(\LaTeX\\) `\section` header. While Org Mode headings will normally create sections and subsections in your document, this particular Org Mode heading is just there for organisational purposes, so that I can fold up and hide away this beginning part of the document and make it easier to navigate straight to the section(s) that I am currently working on.

-   lines 2 and 5 output some \\(\LaTeX\\) code to make the heading in lines 3 and 4 be printed in small caps and nicely centered. It also sets up a nice \\(\LaTeX\\) `tcolorbox` to highlight the lesson intentions. The `#+latex:` lets you insert arbitrary lines of \\(\LaTeX\\) code into the final document if you want to achieve effects not easily realised through the standard Org Mode structures.

    Technically, the `tex and latex output logo math modetex and latex output logo math mode#+latex:` is not required, because if Org Mode detects any \\(\LaTeX\\) code in your document, it should automatically deal with in. In my Emacs theme though, the `#` at the front causes the line to be rendered in a dark grey font so that it fades into the background and doesn't distract so much from the "real" content.

-   Next in lines 7-10, we define the learning intentions. We create a list in Org Mode, simply use a bullet character such as `-`, `+` or `*` (that last one needs to be preceeded with white space so it is not interpreted as an Org Mode heading). On line 8 you see the `#+attr_latex:` directive which allows you to modify the \\(\LaTeX\\) environment that follows, this this case an `itemize` bullet list. In this example, I decrease the separation between the bullet list items to make the list more compact. This technique can be used to tweak the appearance of other environments throughout the document.

-   On line 12 I give a textbook reference that I normally highlight using italics, which in Org Mode is achieve by bracketing the text with `/` symbols (much nicer than writing `\emph{italic text}`, isn't it?).


### Warm up/Do Now {#warm-up-do-now}

Most teachers understand the benefit of giving students something to get started on as soon as they sit down, the so-called "Do Now" activity. It not only helps students transition into the lesson; it also allows for some "formative assessment" to determine how well the students understand the previously-covered material.

I like to put the "Do Now" activity on the front page in a box. So, let's write a heading and give the student some warm-up exercises to do.

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

-   Line 1 contains an Org Mode header that will create a new section in the document (since there is no `:ignore:` tag).
-   Next I create a `tcolorbox` around the warm-up exercises, not by typing \\(\LaTeX\\) code directly, but by bracketing the content with a `#+begin_tcolorbox ... #+end_tcolorbox` pair. This is a general technique you can use to insert a \\(\LaTeX\\) environment into your document, by typing `#+begin_<environment>/ ... #+end_<environment>`, where `<environment>` is the name of the \\(\LaTeX\\) environment you want to use.

    By default, a `tcolorbox` has a grey background. To supress that, you need to provide some options to the environment. That is of course done using the previously mentioned `#+attr_latex:` directive. You can see that line 2 sends the `tcolorbox` an option to make its background white.

-   Line 4, I ask the first question, using a stock-standard numbered list. I use the `\cloze{}` macro I defined earlier to give the students an underlined writing space to fill it -- just specify the length of the line in the curly braces.

-   Line 7 contains the next question, which consists of some sub-questions. If I want to write any inline maths notiation, I enclose it in `$ ... $` symbols just like in regular \\(\LaTeX\\). If you have currency in your text, this can cause problems, so an alternative is to use `\( ... \)`.

    To separate this question form the next, I put a `\tcbline` separator (which you can prefix with a `#+latex:` directive if you choose). It is important to indent it properly so Org Mode doesn't think this is the end of the numbered list.

-   Next I want to list out the subparts to the question. I start a new numbered list that is indented. Here I used `1.` to indicate a list item, but it doesn't really matter what number you write here. In the output, the indented items will be written with letters (a), (b), (c) etc.. You may notice I even doubled up on the number `1.` -- it doesn't matter, \\(\LaTeX\\) will take care for the correct numbering.

    Students will need space to write their answers, so at the start of the list I include a `#+attr_latex:` directive where I set the spacing between list items using `itemsep=2cm`.

-   For the final question, I show you a special list environment provided by the `exesheet` package: the `tablenuma` list. This allows you to set your problems using multiple columns, great for many short questions that don't need to take up a whole line.

    I again use an Org Mode numbered list, but I need to tell Org Mode to use the `tablenuma` environment by specifying `:environment tablenuma` parameter. Again, I want to give the students some writing space. However, this environment doesn't use `itemsep`; instead it uses `after-item-skip` to put space between items and `after-skip` to put space at the end of the list.

    One problem with `after-skip` is that if you don't continue with a normal paragraph, the space at the end gets "swallowed up" by \\(\LaTeX\\). A hack you can use is to put an invisible white space at the end using the `#+latex: ~` like I did in line 21.


### Lesson Body {#lesson-body}

Now we can get to the meat of the lesson.

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

Here you see the Org Mode headings being translated to the different sections and subsections, and we can "fold away" any detail we don't want to see.

-   We start on a fresh page using `\newpage`

-   We use Org Mode headings to add in new sections/subsections of the lesson, and we can use Org Mode's visibility cycling (`<TAB>` key) to hide sections.

-   I have used the `exesheet` command "`\exe`" to introduce an example that I want to work through with the students. By default the numbering of the `\exe` examples/exercises is global across the whole document.

-   I've included a TikZ diagram, just to show how you can freely insert \\(\LaTeX\\) code as needed.


### Set Some Classwork/Homework {#set-some-classwork-homework}

Finally, you might want to set the students some exercises from the textbook.

```org
* Classwork/Homework
#+begin_tcolorbox
Year 12 Awesome Maths Textbook
- Ex. 13A Q2-11, 14, 15.
#+end_tcolorbox
```


## Export to PDF {#export-to-pdf}

Now that we have written up our lesson notes, we can export to LaTeX/PDF. To make a PDF file, I might type something like `C-c C-e l p`, and I will get a PDF lookin something like this.

{{< embed-pdf url="20250105_example-lesson.pdf" >}}


## Final Thoughts {#final-thoughts}

The examples I gave were heavy in LaTeX code because I wanted to showcase the sorts of things that could be done when exporting from Org Mode. Overall, the collapsible headings in Org Mode make it easy to find your way around a complicated document, and it is possible to add even more Org Mode headings with the `:ignore:` tag if you want to make certain parts of the document collapsible.

I didn't show any examples of tables. Org Mode has a powerful table editor which makes tables much easier to produce and manage, and if you want to customise the look of a table, you can use the `#+attr_latex:` directive to tell Org Mode to use the powerful `tblr` package that all \\(\LaTeX\\) users should learn about if they haven't already.

One thing I would like to explore further is the embedding of code to generate content -- for example, to produce tables of $z$-values programatically.
