+++
title = "Exporting Org Mode Tables to LaTeX"
date = 2025-01-17
lastmod = 2025-01-23T18:19:58+11:00
tags = ["Emacs", "OrgMode", "LaTeX", "Teaching"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

In my previous post on using [Org mode to make lesson notes]({{< relref "20250105_orgmode-lesson-notes" >}}), I mentioned that it is possible to export Org mode tables to \\(\LaTeX\\). There are some benefits to using Org mode instead of raw \\(\LaTeX\\) when making tables:

-   Org mode has a powerful [table editor](https://orgmode.org/manual/Built_002din-Table-Editor.html) that makes it easy to draw out a table and move rows and columns around. Org mode will automatically make the all the columns line up nicely.
-   Since the cells in an Org mode table are all nicely lined up, it makes the table much easier to visualise without having to count `&` symbols in raw \\(\LaTeX\\) code.
-   Org mode allows the use of formulas in a table, somewhat like a spreadsheet -- this is harder to achieve using \\(\LaTeX\\) directly.

The tables produced by Org mode are pretty basic by default. However, you can use the \\(\LaTeX\\) [tabularray](https://ctan.org/pkg/tabularray) package to customise the look of your tables. Incidentally, if you are not already using the modern _tabularray_ to produce your \\(\LaTeX\\) tables, you should definitely look into it -- it is way more powerful and easy to use compared to the more traditional table packages (you can thank me later).

So, let's produce a simple table for plotting the function \\(y=x^2\\):

```org
#+attr_latex: :environment tblr :align hlines,vlines,column{1}={gray9},column{2-Z}={r}
| $\bm x$ | $-2$ | $-1$ | 0 | 1 | 2 |
| $\bm y$ |    4 |    1 | 0 | 1 | 4 |
```

Some remarks:

-   the `#+attr_latex:` line immediately preceeding the table tells Org mode to customise the \\(\LaTeX\\) code for the table.
    -   The `:environment` attribute tells Org mode to use tabularray's _tblr_ table environment instead of the \\(\LaTeX\\) default table environment.
    -   The `:align` attribute allows us to specify the parameters that the _tblr_ environment is expecting. You can see here I've asked for horizintal and vertical lines; I want the first column containing my variable names \\(x\\) and \\(y\\) to be shaded grey, and I want my numbers to be right-justified.
-   I've used `$` signs around the variable names and the negative numbers so that they get rendered correctly. I also used the `\bm` from the \\(\LaTeX\\) [bm package](https://ctan.org/pkg/bm) to have the variable names bold.

This will produce a table that looks like this:

{{< figure src="20250117_table1.png" >}}

Sometimes you want to leave certain cells in the table blank, and leave enough space for students to write their own values into the table. This is easy using the _tblr_ environment:

```org
#+attr_latex: :environment tblr
#+attr_latex: :align hlines,vlines,row{2}={15mm},column{1}={gray9},column{2-Z}={20mm,c}
| $\bm x$ | $-2$ | $-1$ | 0 | 1 | 2 |
| $\bm y$ |      |      |   |   |   |
```

-   here I put the `:environment` and `:align` attributes on separate lines, just to show that this is an option.
-   I've specified that the second row should be 15mm high
-   I specified that columns \\(2\\) through \\(Z\\) should be $20$\\,mm wide. The \\(Z\\) has a special meaning when specifying rows and columns in a _tblr_ environment -- it refers to the last row or column. This means I don't need to adjust the range specification if I add and remove columns.

If we then export to PDF, the table will look like this:

{{< figure src="20250117_table2.png" >}}

As I said before, _tblr_ is a convenient modern way to specify \\(\LaTeX\\) tables, and it is a very powerful package. For more information about what it can do, check out the _tabularray_ [documentation](https://mirror.aarnet.edu.au/pub/CTAN/macros/latex/contrib/tabularray/tabularray.pdf).
