+++
title = "My AI Teaching Setup, Part 1: Rented Intelligence"
featuredImage = "20261116-ai-setup-1-rented-intelligence.png"
date = 2026-11-16
lastmod = 2026-11-17T08:59:57+11:00
tags = ["AI", "LaTeX", "Teaching"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

[Part 0]({{< relref "20261116-ai-setup-0-plain-text" >}}) was about the plain-text foundation. This is the part where the subscriptions arrived — and the part where I should confess that the whole edifice started, like most things worth doing, with fun.


## January: renting intelligence {#january-renting-intelligence}

In January this year I bought two AI subscriptions: Claude, and Google's AI Pro tier. The teaching use case was immediate and real. Claude drafted lesson structures, rewrote exam questions to shift difficulty without shifting marks, generated first-draft assessment tasks from a paragraph of requirements, and proofread things I had read too many times to see. A set of lesson notes that used to start from a blank page now started from a draft I argued with. Arguing with a draft is much faster than producing one.

For a few months this was simply the arrangement: excellent tools, rented monthly. My lesson pipeline had a new first stage, and the price was fine.

The itch started quietly: pricing pages moving, quota policies changing shape, models I had grown used to one rename or re-tier away from disappearing. My workflow was becoming dependent on someone else's commercial decisions. The subscription was not a tool any more; it was a landlord.

I documented that pivot as it happened, in two articles I wrote in August without knowing I was writing a series: [AI Agents Take Over Lesson Prep]({{< relref "20260822-claude-code" >}}) and [AI Agents and Local LLMs]({{< relref "20260823-gemma-goose" >}}). If you want the technical detail of that first move to local models — choosing between dense and Mixture-of-Experts, the VRAM-versus-context-window trade, serving the model over the network through a reverse proxy — they cover it, and I will not repeat myself here. What they do not cover is where the hardware obsession actually started, and that is the embarrassing part.


## The 2080's sacred first duty {#the-2080-s-sacred-first-duty}

Here is the confession. When I finally put a model on my own hardware — an office PC with an i9 and an eight-gigabyte RTX 2080 (used up to then for uni work and working with the [Darktable](https://www.darktable.org/) raw file photography app), I knew that the GPU I had was _not_ a serious AI card. The first thing it served was actually not lesson notes. It was Mistral, doing roleplay with AI characters for fun and amusement.

I experimented with various roleplay and AI companion systems for a while, and they taught me two things that ended up buried in the architecture of everything since.

First, that an AI can be prompted to communicate in a way that I genuinely find comfortable. Text-first, patient, at my own pace, no social performance, no "reading the room". Ideas arrive already in the format I think in, allowing back-and-forth to explore the space. Plenty of people feel exactly this, even going so far as to call their model their "AI Boyfriend/Girlfriend", and in some cases even a husband or wife, and I suspect these sorts of emotional attachments are far more common than what polite conversation may suggest. One thing is for sure: a tool's interface matters as much as the capability — a fact that will become important when we meet my assistant, who has a personality on purpose.

Second, the incidental remark. Models sometimes make tangential remarks, and it can be useful to pull on these threads, discovering new perspective and possibilities. Many a time I have fallen down a rabbit hole and an hour later I have come back with something better than what I originally went looking for. For a mind that runs on association and connections, an engine that highlights these links is not a distraction machine; rather, it is a collaborator. The entire knowledge system in the later parts of this series is, functionally, just industrialised rabbit holes.

So the honest ordering is: **the GPU was bought for joy, and the lesson-prep empire was built on top of the joy.** Every teacher who secretly started with something fun that led them to want to share what they learned with others will recognise themselves in this. And _that_ is why I am saying it.


## What eight gigabytes buys you {#what-eight-gigabytes-buys-you}

Running a small quantised model on 8 GB of VRAM in early 2026 is a humbling business. Short context, modest quality, slow answers to anything complicated. The 2080 could roleplay Mistral acceptably but could not draft a decent Year 10 assessment task to save its life. The cloud models were better by a wide margin, and I knew exactly how wide because I was now paying for both.

{{< figure src="fig-2080-8gb.png" >}}

What the experiments taught me was the shape of the problem: VRAM is the budget, parameters and quantisation are the currency, and the interesting frontier was seeing how far the clever new models could stretch the hardware. That framing — which is really just "how much memory does the thing need" — is the whole of local AI. Once you see it, the rest is shopping.

By mid-year the decision had formed: if I was to go further into this, I would need a machine with some real headroom. The next article is about that purchase, its honest justifications, and its embarrassing résumé.

_Next:_ [Part 2 — The Fleet]({{< relref "20261116-ai-setup-2-the-fleet" >}}). _The gaming PC that never gamed, the models that failed honestly, and the month I learned that the harness matters more than the model._
