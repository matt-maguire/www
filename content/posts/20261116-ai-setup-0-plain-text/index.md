+++
title = "My AI Teaching Setup, Part 0: A Life in Plain Text"
date = 2026-11-16
lastmod = 2026-11-15T22:47:45+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs"]
categories = ["Blog"]
draft = true
weight = 3001
author = "Matt Maguire"
+++

This is the first of a few articles about the AI system I use for teaching maths. The honest summary: ten months ago I bought an AI subscription, and today I run a small fleet of machines and agents that draft my lessons, check my exams, index my syllabus, and argue with me about design at midnight. This first article is about the decade _before_ any of that, because it turns out to be the part that made everything else possible.


## Years of LaTeX {#years-of-latex}

I have written every teaching document of my career in LaTeX. It started with `mteach`, a class designed for maths teachers, and grew into my own house style: lesson notes, worksheets, assessment tasks, all of it text files that compile to PDF. This choice is the least glamorous and most consequential decision in the story. My entire professional output — every lesson, every question, every mark allocation — has lived for years as plain text in folders.

The cost of that choice was time. LaTeX is particular, and the diagrams are worse. Drawing one number line with labelled fractions, or a circle divided for fraction introduction, could take twenty minutes of TiKZ fiddling — coordinates, anchors, overlapping labels, the small talk of vector graphics. And the diagrams matter: I lean heavily on dual coding, getting the algebra and the picture to reinforce each other, so "skip the diagram" was never an option. The TiKZ tax was just a fact of life, paid every lesson-planning session.


## An Org-mode brain {#an-org-mode-brain}

The other half of the plain-text life is [Org mode](https://orgmode.org): my notes, tasks, calendar, knowledge base and lesson plans are all text files, cross-linked zettelkasten style. Teaching knowledge — NESA outcomes, syllabus quirks, which explanation worked for which topic — accumulated in thousands of small linked notes.

At the time this was just me being a text obsessive with an Emacs-shaped habit. It did not occur to me that I was building anything.


## The background I should confess {#the-background-i-should-confess}

Before teaching I spent years as an electrical engineer, mostly software, and I have been a computer hobbyist for as long as I can remember. Networking, terminals, a bit of neural-network theory from when it was a niche subject — none of it was unusual then, and none of it prepared me specifically for what happened in 2026. I mention it only because it smoothed the early road, and because the road got smoother still, for reasons that become the point of this whole series: the technical parts turned out to be the parts you could delegate.


## The first hours back {#the-first-hours-back}

The first thing AI ever did for me was TiKZ. I described the diagram in a sentence and it produced the coordinates. Twenty minutes became two, and the two were usually better, because the AI remembered anchor tricks I had forgotten. Then it was the LaTeX prose, the question wording, the "make this part harder but keep the marks the same" requests that used to be an hour of careful surgery.

And here is the quiet realisation that this series keeps circling back to: **I never had an integration project.** When AI tools arrived, they could already read my entire professional life. No PDF wrangling, no proprietary formats, no export step — the lessons, the notes, the brain itself were text files, and text is what these tools eat. People ask me how I got my knowledge into the AI. I didn't. I had been writing it in the AI's native language for years. I just had to point the AI at it.

That is the lesson of Part 0, and I did not earn it deliberately: **build your working life in plain text.** Not because AI is coming — although it is — but because text compounds. It links, it diffs, it survives every vendor, and one day it turns out to be the foundation everything else gets built on.

_Next: Part 1 — Rented Intelligence. The subscriptions that started it, the anxieties that followed, and the sacred first duty of an eight-gigabyte graphics card._
