+++
title = "My AI Teaching Setup, Part 2: The Fleet"
featuredImage = "20261116-ai-setup-2-the-fleet.png"
date = 2026-11-16
lastmod = 2026-11-16T17:35:49+11:00
tags = ["AI", "LaTeX", "Teaching", "Hardware"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

[Part 1]({{< relref "20261116-ai-setup-1-rented-intelligence" >}}) ended with the 8 GB ceiling and a decision. This is the hardware part, and the part where the system stopped being a computer and started being a staff.


## The gaming PC that never gamed {#the-gaming-pc-that-never-gamed}

In July I built hyperion: a proper gaming PC, RTX 5090, 32 GB of VRAM. The justifications were two, and I was honest about both at the time. The AI justification: if I was going further into this, I needed headroom — enough VRAM for a genuinely good open model with a long context window. The hobby justification: I have wanted a big gaming PC for roughly a decade, and AI was the first excuse strong enough to survive my own accounting.

The embarrassing résumé, three months later: games installed and played: zero. hyperion's entire working life is teaching. It serves the model that drafts my lessons, hosts my family's backups, and hums under the house doing nothing else at all. The hobby excuse never got spent; the work excuse turned out to be the true one. I wonder whether I can claim it on my tax... probably a conversation for my accountant rather than this blog.


## Models: the honest failures {#models-the-honest-failures}

When I wrote the August article, the daily driver was Gemma — a 31-billion-parameter dense model served by Ollama, with the Goose agent driving it. That setup genuinely worked, and then the ground moved under it, because this is the year the ground keeps moving. Gemma gave way to a 27-billion-parameter Qwen, and that to a 30-billion-parameter Mixture-of-Experts model — still on plain Ollama, still on the same card. The MoE trick was proof that the next leap would be software, not hardware. The keeper today is a larger MoE: a big library of smaller specialists where each query only wakes the few experts it needs. That architecture is what makes a 32 GB card serve something genuinely clever: the _whole_ model doesn't need to fit, just the parts being consulted. It drafts my lesson notes and my assessment tasks to a standard I sign off on, and it runs on electricity, not on someone's pricing page.

{{< figure src="fig-5090-gemma.png" >}}

{{< figure src="fig-5090-qwen.png" >}}

{{< figure src="fig-strata-tiers.png" >}}

In between, I collected failures, and they are the useful part:

-   A much-hyped model arrived with a reputation for cleverness. It aced the pure maths verification and then, asked to draw a number line, confidently stacked every label at the same coordinate. **Maths brain, broken hands.** A model's benchmark reputation told me nothing about whether it could produce _my_ documents.
-   A benchmark run died mysteriously until I found the cause: a context-window limit set in the wrong place, silently compressing the model's memory of its own work mid-task. **Half of "the model failed" is really "the plumbing failed."**
-   A machine running two models at once started paging pieces of them to disk. The model got subtly worse, in no particular way, which is the worst way for software to fail. **A model that has been swapped out is a model with a headache.**

The old office machine was not retired — it now runs a smaller, fast Qwen as the second-opinion engine, the 2080 finally earning a proper salary after its years of roleplay duty.

{{< figure src="fig-msi-hybrid.png" >}}


## The harness is the product {#the-harness-is-the-product}

The biggest lesson of the year, and the one I had no warning about: **the model matters less than the thing you wrap around it.** A mediocre model in a good harness beats a brilliant model in a bad one, consistently, on real work.

The harness is everything between "chat with a model" and "get a document out": it reads and edits files, runs the LaTeX compiler, sees the errors, fixes them, runs the verification, keeps a plan, and knows your house style. The current best artifact-maker in the house is an _orchestrator_: one model plans and delegates, cheaper models execute the subtasks in clean contexts, and the planner verifies the results. A committee, in other words. It outperforms any single model I can run, at a fraction of the cost of the obvious approach.


## Fixing the agent's eyes {#fixing-the-agent-s-eyes}

The [August article]({{< relref "20260823-gemma-goose" >}}) ended with the _pdftopbm_ workaround: because Gemma could see images, Goose learned to convert each PDF page into an image and read the page the way I would. It worked. What I wanted next was the same eyes pointed inward. I wanted the agent to check the graphs it was producing — a parabola whose labels collide with the curve, a legend sitting on top of a shaded region, axes with no labels at all. The kind of thing a human catches with one glance at the compiled PDF.

Getting there meant trying other harnesses, because model and harness have chemistry, and some pairings simply do not work. A Qwen2-coder model that wrote perfectly good code refused to call tools properly inside Goose. The brain was fine; the coupling was broken. It is the same lesson as the model that got paged to disk: half of "the model failed" is really "the plumbing failed."

Then I found an actual bug. Goose ships with a catalogue of known models, and a model I serve myself is not in that catalogue. I had declared _supports_vision_ in my configuration; Goose ignored the declaration and quietly dropped every image from the request. The model never saw the graph. No error, no warning — the agent just critiqued output it could not see, confidently, in great detail.

I have never written Rust. So I asked the AI to help me patch Goose itself: read a codebase I could not read, find where provider capabilities get resolved, write the fix and the tests. Four hundred and forty added lines across ten files, submitted as [pull request 12769](https://github.com/aaif-goose/goose/pull/12769), merged the next day. The agent got its eyes back, and it now uses them on its own work: compile, look, spot the overlapping labels, fix the layout, re-render. The system repaired its own eyes somewhere in there, and I picked up a merged contribution to an open-source project I never planned to touch.


## Landing on Hermes {#landing-on-hermes}

By autumn the harness scene had exploded. Goose, the agent I was using back in August, has been joined — and largely outperformed — by a whole generation: OpenCode, Qwen's own agent, Google's Antigravity. All of them brilliant coding assistants. I wired them all to whichever machine suits each job, and I kept the best of them.

But what I have effectively landed on is different in kind. Hermes is not a coding assistant that waits for me to open a terminal. It is always on: it keeps memory between sessions, it lives in a WhatsApp thread so I can reach the house from anywhere, and it runs scheduled jobs — reminders, nightly backup checks, batches of model benchmarks that run while I sleep. It watches the fleet: watchdog agents that check disk space and backups every morning and escalate when something smells wrong. And it has colleagues: when one machine's agent finishes an experiment, it writes a handover note, and another agent picks the work up the next morning. They all read from the same shelf, too: a vector database — Qdrant — holding a decade of my plain-text notes, searchable by meaning rather than keyword, queried the same way by every agent in the house. But that shelf is the subject of [Part 3]({{< relref "20261116-ai-setup-3-the-organs" >}}).

The scope quietly widened. AI stopped being something I use to produce documents and became something that helps run the little household of machines the teaching setup grew into. Which is where the promise from [Part 1]({{< relref "20261116-ai-setup-1-rented-intelligence" >}}) gets paid: the assistant with a personality on purpose has arrived, and it runs this house.

And the relationship changed somewhere in there. I stopped asking AI questions and started bouncing designs with it — asking the local model to help evaluate which model to install, reviewing plans written by one agent for another to execute. The system now includes its own staff. The human job became direction, taste, and refusing to trust anything unverified.


## The fleet needs a larder {#the-fleet-needs-a-larder}

Ten machines and a staff of agents is one thing; the data they share is another. The spine of the whole setup — the Nextcloud that syncs my notes between home and school — sat on two external Seagate portables in a USB dock, mirrored at each other. And the USB bridge boards refuse to report the health of the drives inside them: ask them how the disks are doing and they answer with silence. I was running the household's memory on hard drives I could not monitor. That, plus the sheer size of the thing — three and a half terabytes and growing — is what finally pushed me to build a proper NAS.

There was one problem: I had not built a PC since the 1990s. hyperion had arrived prebuilt in July and I had not opened the side panel in the months since. Modern internals are a foreign country — drive cages and shrouds, SATA power, NVMe slots hiding under heatsinks. So the AI became my build partner, and its first act was reading. It downloaded the case and motherboard manuals, read them properly, and sent me the relevant pages as diagrams: where the 3.5-inch bays actually live, that the board has four SATA ports plus one connector that expands to four more, which slot the existing drive occupies — traced through the PCIe wiring without opening the case. It also corrected its own earlier guesses, which had come from my photographs. A photo shows what is installed today; a manual says what is possible. I have learned to tell them apart.

Shopping had one trap worth knowing: the cheapest big shelf drives record **shingled** — SMR — and rebuild disastrously once a second drive fails. The rule was conventional recording only, which is how two Seagate IronWolf 8TB drives ended up in the cart, going into hyperion itself rather than a new box: the gaming PC that never gamed doubles as the household's storage spine. Before formatting anything, the agent ran an identity gate — model, capacity, serial number, spinning-disk flag, read back from the drives and checked against what we ordered, because a half-seated cable must not become half a RAID. The pair went in as a mirrored btrfs volume labelled **ironwolf**, with a nightly watcher that reads drive health and scrubs for silent bit-rot.

Then the move: 3.5 TB ferried across a direct 2.5-gigabit link between the machines — one pass against the live tree while the site kept serving, a final delta pass while it was paused for the cutover. The details the AI kept in its pocket are the ones that bite: the mount point must match the application's data directory exactly, one level off and the site comes back broken; the new mount line must be incapable of hanging the boot; and the backup tool must be checked to follow the new mount, or the larder goes unbacked-up in silence. The retired portables were reformatted as time dials — snapshot targets, one per machine — so a file deleted this morning is a minute from a restore, and one dead machine is still not a lost decade.

For the story I have been telling, this is the beat that matters: the same assistant that patched Goose's Rust read a case manual and stood me up at the drive cage. AI did not just live inside the machine. It walked me through opening one, for the first time in twenty-five years.

_Next: Part 3 — The Organs. The knowledge base that was already ten years old, the tools that make verification automatic, and the honest cost sheet, gaming column: zero._
