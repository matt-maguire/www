+++
title = "My AI Teaching Setup, Part 2: The Fleet"
date = 2026-11-16
lastmod = 2026-11-16T10:35:13+11:00
tags = ["AI", "LaTeX", "Teaching", "Hardware"]
categories = ["Blog"]
draft = true
weight = 4001
author = "Matt Maguire"
+++

[Part 1]({{< relref "20261116-ai-setup-1-rented-intelligence" >}}) ended with the 8 GB ceiling and a decision. This is the hardware part, and the part where the system stopped being a computer and started being a staff.


## The gaming PC that never gamed {#the-gaming-pc-that-never-gamed}

In July I built hyperion: a proper gaming PC, RTX 5090, 32 GB of VRAM. The justifications were two, and I was honest about both at the time. The AI justification: if I was going further into this, I needed headroom — enough VRAM for a genuinely good open model with a long context window. The hobby justification: I have wanted a big gaming PC for roughly a decade, and AI was the first excuse strong enough to survive my own accounting.

The embarrassing résumé, three months later: games installed and played: zero. hyperion's entire working life is teaching. It serves the model that drafts my lessons, hosts my family's backups, and hums under the house doing nothing else at all. The hobby excuse never got spent; the work excuse turned out to be the true one. Whether the ATO agrees with that framing is a conversation for my accountant, not this blog.


## Models: the honest failures {#models-the-honest-failures}

When I wrote the August article, the daily driver was Gemma — a 31-billion-parameter dense model served by Ollama, with the Goose agent driving it. That setup genuinely worked, and then the ground moved under it, because this is the year the ground keeps moving. Gemma gave way to a 27-billion-parameter Qwen, and the keeper today is a large Mixture-of-Experts model — a big library of smaller specialists where each query only wakes the few experts it needs. That architecture is what makes a 32 GB card serve something genuinely clever: the _whole_ model doesn't need to fit, just the parts being consulted. It drafts my lesson notes and my assessment tasks to a standard I sign off on, and it runs on electricity, not on someone's pricing page.

In between, I collected failures, and they are the useful part:

-   A much-hyped model arrived with a reputation for cleverness. It aced the pure maths verification and then, asked to draw a number line, confidently stacked every label at the same coordinate. **Maths brain, broken hands.** A model's benchmark reputation told me nothing about whether it could produce _my_ documents.
-   A benchmark run died mysteriously until I found the cause: a context-window limit set in the wrong place, silently compressing the model's memory of its own work mid-task. **Half of "the model failed" is really "the plumbing failed."**
-   A machine running two models at once started paging pieces of them to disk. The model got subtly worse, in no particular way, which is the worst way for software to fail. **A model that has been swapped out is a model with a headache.**

The old office machine was not retired — it now runs a smaller, fast Qwen as the second-opinion engine, the 2080 finally earning a proper salary after its years of roleplay duty.


## The harness is the product {#the-harness-is-the-product}

The biggest lesson of the year, and the one I had no warning about: **the model matters less than the thing you wrap around it.** A mediocre model in a good harness beats a brilliant model in a bad one, consistently, on real work.

The harness is everything between "chat with a model" and "get a document out": it reads and edits files, runs the LaTeX compiler, sees the errors, fixes them, runs the verification, keeps a plan, and knows your house style. Goose, the agent I was using back in August, has since been joined — and largely outperformed — by a whole generation of them: OpenCode, Hermes, Qwen's own agent, Google's Antigravity. I wired them all to whichever model fits each job. The current best artifact-maker is an _orchestrator_: one model plans and delegates, cheaper models execute the subtasks in clean contexts, and the planner verifies the results. A committee, in other words. It outperforms any single model I can run, at a fraction of the cost of the obvious approach.

And the relationship changed somewhere in there. I stopped asking AI questions and started bouncing designs with it — asking the local model to help evaluate which model to install, having agents run the benchmarks while I slept, reading handover notes written by one agent to another. The system now includes its own staff. The human job became direction, taste, and refusing to trust anything unverified.

_Next: Part 3 — The Organs. The knowledge base that was already ten years old, the tools that make verification automatic, and the honest cost sheet, gaming column: zero._
