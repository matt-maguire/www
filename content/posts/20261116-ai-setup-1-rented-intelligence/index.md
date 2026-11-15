+++
title = "My AI Teaching Setup, Part 1: Rented Intelligence"
date = 2026-11-16
lastmod = 2026-11-16T10:35:06+11:00
tags = ["AI", "LaTeX", "Teaching"]
categories = ["Blog"]
draft = true
weight = 4001
author = "Matt Maguire"
+++

[Part 0]({{< relref "20261116-ai-setup-0-plain-text" >}}) was about the plain-text foundation. This is the part where the subscriptions arrived — and the part where I should confess that the whole edifice started, like most things worth doing, with fun.


## January: renting intelligence {#january-renting-intelligence}

In January this year I bought two AI subscriptions: Claude, and Google's AI tier. The teaching use case was immediate and real. Claude drafted lesson structures, rewrote exam questions to shift difficulty without shifting marks, generated first-draft assessment tasks from a paragraph of requirements, and proofread things I had read too many times to see. A set of lesson notes that used to start from a blank page now started from a draft I argued with. Arguing with a draft is much faster than producing one.

For a few months this was simply the arrangement: excellent tools, rented monthly. My lesson pipeline had a new first stage, and the price was fine.

The itch started quietly: pricing pages moving, quota policies changing shape, models I had grown used to one rename or re-tier away from disappearing. My workflow was becoming load-bearing on someone else's commercial decisions. The subscription was not a tool any more; it was a landlord.

I documented that pivot as it happened, in two articles I wrote in August without knowing I was writing a series: [AI Agents Take Over Lesson Prep]({{< relref "20260822-claude-code" >}}) and [AI Agents and Local LLMs]({{< relref "20260823-gemma-goose" >}}). If you want the technical detail of that first move to local models — choosing between dense and Mixture-of-Experts, the VRAM-versus-context-window trade, serving the model over the network through a reverse proxy — they cover it, and I will not repeat myself here. What they do not cover is where the hardware obsession actually started, and that is the embarrassing part.


## The 2080's sacred first duty {#the-2080-s-sacred-first-duty}

Here is the confession. When I finally put a model on my own hardware — an office PC with an i9 and an eight-gigabyte RTX 2080, which is _not_ a serious AI card — the first thing it served was not lesson notes. It was Mistral, doing roleplay.

I had been experimenting with roleplay and AI companion systems for a while before that, and they taught me two things that ended up buried in the architecture of everything since.

First, that an AI can be a form of communication I genuinely find comfortable. Text-first, patient, at my own pace, no social performance, no reading the room. Ideas arrive already in the format I think in. Plenty of people feel exactly this and I suspect it is more common than the polite conversation suggests. The interface matters as much as the capability — a fact that will become important when we meet my assistant, who has a personality on purpose.

Second, the incidental remark. Models say sideways things — a passing "this also appears in…" — and I follow the thread, and an hour later I have fallen down a rabbit hole and come back with something better than what I went looking for. For a mind that runs on association, an engine that generates associations is not a distraction machine; it is a collaborator. The entire knowledge system in the later parts of this series is, functionally, industrialised rabbit holes.

So the honest ordering is: **the GPU was bought for joy, and the lesson-prep empire was built on top of the joy.** Every teacher who secretly started with the fun stuff will recognise themselves. That is why I am saying it.


## What eight gigabytes buys you {#what-eight-gigabytes-buys-you}

Running a small quantised model on 8 GB of VRAM in early 2026 is a humbling business. Short context, modest quality, slow answers to anything complicated. The 2080 could roleplay Mistral acceptably and could not draft a Year 10 assessment task to save its life. The cloud models were better by a wide margin, and I knew exactly how wide because I was now paying for both.

What the experiments taught me was the shape of the problem: VRAM is the budget, quantisation is the currency, and the interesting frontier was models clever enough to be worth the hardware. That framing — which is really just "how much memory does the thing need" — is the whole of local AI. Once you see it, the rest is shopping.

By mid-year the decision had formed: if I was going further into this, I needed a machine with real headroom. The next article is about that purchase, its honest justifications, and its embarrassing résumé.

_Next: Part 2 — The Fleet. The gaming PC that never gamed, the models that failed honestly, and the month I learned that the harness matters more than the model._
