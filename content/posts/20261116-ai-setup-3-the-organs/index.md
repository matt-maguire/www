+++
title = "My AI Teaching Setup, Part 3: The Organs"
date = 2026-11-16
lastmod = 2026-11-15T22:47:46+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs"]
categories = ["Blog"]
draft = true
weight = 3004
author = "Matt Maguire"
+++

[Part 2]({{< relref "20261116-ai-setup-2-the-fleet" >}}) was about models and harnesses. But a model in a harness is still just a clever voice. What turned the setup into a _system_ was the organs: the memory, the senses, and the reflexes bolted on around it. This is the final part, and it starts with a full circle.


## The knowledge base that was already ten years old {#the-knowledge-base-that-was-already-ten-years-old}

People ask how I got my teaching knowledge into the AI. The answer is anticlimactic: I didn't. I had spent a decade writing it in plain text — Org-mode notes, syllabus annotations, lesson reflections, the whole zettelkasten from [Part 0]({{< relref "20261116-ai-setup-0-plain-text" >}}).

What the system needed was a way to _search by meaning_ rather than by keyword, so the model can be asked "what have I written about students confusing gradient with rise-over-run?" and get my own past thinking back, cited. That is what a vector database does: it indexes text by what it means. Qdrant, running on the office box, holds the knowledge base — my notes, the NESA syllabus, the textbooks, the maths resources — and every agent in the house queries it the same way.

The decade of plain text paid for itself in an afternoon. No conversion project, no vendor export, no PDF archaeology. Point the index at the folders. Done.


## The reflexes: never let the model do a human's job {#the-reflexes-never-let-the-model-do-a-human-s-job}

The organs that matter most are the ones that catch the model lying — and models lie with total confidence, in fluent sentences, about things you can check.

-   **SymPy does the maths.** No model computes an integral or solves an equation in this pipeline. Answers are verified symbolically by a computer-algebra system before anything reaches a student. A language model's idea of arithmetic is autocomplete with delusions of competence.
-   **LaTeX does the marks arithmetic.** Early drafts had the model adding up the marks per question, and it got them wrong often enough to be dangerous — an exam where the section header says 50 and the questions sum to 43. The fix was to stop asking the model to count at all: the document class now keeps a counter, every question registers its marks, and the total is _computed at compile time_. The model declares; LaTeX arithmetic. This single change eliminated a whole class of errors.
-   **The compiler is the proofreader.** Every generated document must compile, and the build is checked mechanically — question counts, mark totals, diagram presence — before I ever open the PDF.
-   **Search we own.** A private SearXNG instance handles the agents' web research — no per-query API bill, no third party logging what my tools look up about my syllabus.

The pattern: give the model the parts that need judgement — wording, structure, pedagogy, the draft — and give everything with a _checkable_ answer to a tool that cannot be wrong. The skill that matters is not prompting. It is **verification**, and building verification into the pipeline so it happens whether or not anyone remembers to ask.


## The staff {#the-staff}

The last organ is the one that surprised me most: the system now has employees. Agents with persistent memory and named personalities run on the machines — one manages the office box, reads the monitoring reports each morning, writes handover notes to itself, and has genuinely caught problems before I did. A watchdog checks every backup _completed_, not merely that the disk has room, and messages a summary each morning; the week it went in, it found a machine that had never once finished a backup, four days into a failure nobody had noticed.

This is where the persona stuff from Part 1 comes back around. An assistant with a personality is an interface, not a gimmick: it makes the interaction a conversation you _want_ to have, so you have it, daily, and the system stays tended rather than merely installed.


## The honest cost sheet {#the-honest-cost-sheet}

-   **Hardware:** hyperion (the 5090 gaming PC) plus the existing office machine. The gaming column of the ledger, three months in: zero games played. The AI got the machine at a discount, effectively, because the hobby paid most of the list price.
-   **Electricity:** a few cents an hour, humming under the house.
-   **Subscriptions:** I kept them. The January subscriptions — Claude and Google — still run, because the honest ending of a local-AI story is that the cloud never left. Back in August I wrote that for the hard cases "I still have the option of using the more powerful Claude or Gemini paid models." That turned out not to be a stopgap but the architecture: the rented tier is burst capacity and frontier reach, and the pipeline routes to whichever is right for the job. This was never ideology. It is engineering: own the core, rent the burst.
-   **Time:** the real cost, and the real return. Ten months of evenings, mostly spent _with_ the AIs who built most of it themselves.


## If you are not technical {#if-you-are-not-technical}

The question every teacher asks: _I couldn't build this._ The honest answer from someone who has watched the whole thing unfold: the technical parts turned out to be the parts you can delegate. What the system actually required — from a maths teacher, not an engineer — was the habits: insist on verification, keep everything in text, start from joy rather than productivity, timebox the rabbit holes instead of banning them, and refuse to ship anything unverified.

The machine that was supposed to save me time first gave me a way of working I actually enjoy. The time-saving was downstream of the joy. That, more than any model or piece of hardware, is the thing I would tell you to buy.

---

_Disclosure, because you deserve it: these four articles were drafted with the system they describe — the agents supplied research, structure and many of these sentences; the errors that survived are mine, and I checked the claims that mattered. If that bothers you, good: that instinct is exactly the skill this series is trying to teach._
