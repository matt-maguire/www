+++
title = "My AI Teaching Setup, Part 4: The 108-Gigabyte Gamble"
featuredImage = "20261121-ai-setup-4-the-gamble.png"
date = 2026-11-21
lastmod = 2026-11-23T09:35:05+11:00
tags = ["AI", "LaTeX", "Hardware", "Teaching"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

Parts [two]({{< relref "20261116-ai-setup-2-the-fleet" >}}) and [three]({{< relref "20261116-ai-setup-3-the-organs" >}}) of this series were about the tidy part: the fleet, the resident model, and the knowledge base working the way the architecture diagrams promised. This is the part where I try something ambitious on a Saturday and spend the day watching my own assumptions lose an argument with physics. In a build log, the experiments that fail are usually the ones worth keeping.

---


## What already worked {#what-already-worked}

Before the gamble, it helps to remember what the fleet was already doing without breaking a sweat. [Part 3]({{< relref "20261116-ai-setup-3-the-organs" >}}) covered Strata in detail --- the Qwen-Next-style MoE living on `hyperion` and answering at about 250 tokens per second, vision included --- so here is the short version of _why_ it runs that fast.

Strata is a mixture-of-experts model: 24,576 small specialist sub-networks, with only a handful firing for any given token. The trick was tiering it deliberately on one machine. The hottest 10,720 experts, picked from the model's own routing-frequency profile, get pinned into the 5090's VRAM at boot. All 24,576 sit in a pinned 47 GB RAM arena one step behind. There is no SSD cold tier by design --- the rare cold routings take the PCIe bus instead. Every tier runs deliberately full, and there is not a network cable in sight: a token never leaves the machine that owns it.

Keep that design in mind, because the MiniMax experiment walked up to every piece of it and did the opposite.


## Why gamble at all {#why-gamble-at-all}

The open-weight model MiniMax M2.7 has been doing something hard to ignore: sitting on the SWE-bench podium next to closed frontier models, and treated across the industry as roughly "Sonnet-class" --- about eight to ten points behind the current Opus on Terminal-Bench (57.0% vs 65.4%). Not long ago that gap was a chasm; now it is a measurable notch.

For my setup, the temptation was not abstract curiosity. It was three very specific constraints:

-   **Student data stays home.** Names, marks and registers never leave the house --- that is a hard rule, not a preference. A local model can process marked student work; a cloud model, by that same rule, cannot.
-   **Marginal cost is the price of electricity.** I already made this argument for [Strata]({{< relref "20261116-ai-setup-3-the-organs" >}}). Having the same economics at flagship-adjacent quality is the obvious next itch to scratch.
-   **No outage tail risk.** I watched a cloud cascade idle a continent for a week; a brain on my own wire cannot be knocked offline by someone else's config push.

So the Saturday question was narrow enough to look sensible: _can a Sonnet-class model, running locally at zero marginal cost, take overnight artifact generation --- exam candidates, lesson drafts --- off the cloud altogether?_


## The plan {#the-plan}

M2.7 is a 230-billion-parameter mixture-of-experts model; in IQ4_XS quantisation its weights are 108 GB of file. No single machine in the fleet holds that on its own. What I did have, as [Part 2]({{< relref "20261116-ai-setup-2-the-fleet" >}}) laid out, were two machines that together held far more than enough raw memory: `hyperion` (an RTX 5090 with 32 GB VRAM and 64 GB of DDR5, normally Strata's home) and `msi` (64 GB of DDR4 and an 8 GB 2080 SUPER), wired together by a dedicated 2.5 GbE Ethernet link. That is 168 GB of memory for 108 GB of weights --- the capacity was never the problem. The catch is that it came as four separate pools, and llama.cpp's RPC mode, built to split a model across machines, would have to spend them as one working set. On paper, it looked close enough to try.

The Saturday schedule was set: pause Strata, load M2.7 distributed across the link, run my fixed bench kit (a Ti*k*Z diagram task, an exam generation task, a lesson task, and a pure-maths task), then hand the machine back to Strata.


## Who ran the bench {#who-ran-the-bench}

Worth saying up front, because it changes what this story is: the hands at the bench were the assistant's. The download was a script the assistant wrote --- `wget`, four shards, rate-limited to 8 MB/s so the household link survived the afternoon, 108.4 GB on disk by 14:48. The installs, the RPC servers on both machines, the log capture, the timing table: all run by the assistant over ssh while I watched the logs come in.

Even the bench kit is agent machinery. `run.sh` feeds one frozen prompt to an agent harness --- OpenCode driving Strata, Claude Code driving the cloud entry --- each run in a fresh workdir from a seed, no ad-libbed prompting, timing appended to a CSV. The rendered pages get judged by a different assistant than the one under test, on purpose: the proctor and the contestant don't share a scorecard.

What stayed mine was the plan, the arithmetic, and the stop calls. Hold that arrangement in mind through the failure below; this post is as much a test of the lab partner as of the model.


## Round one: the GPU that wasn't {#round-one-the-gpu-that-wasn-t}

The model loaded. No complaints, no warnings, everything looking fine. Then I checked `nvidia-smi`, which reported the 5090 using 18 megabytes.

The prebuilt llama.cpp binaries had arrived missing their CUDA libraries, so my grand "GPU" run had quietly shrugged and fallen back to the processors. No error, no protest --- just a silent fallback that would have wasted the entire day if I had trusted the load message instead of checking what the hardware was actually holding. Lesson one, learned cheaply enough: _verify the hardware is actually involved before taking the software's word for it._


## Round two: the RAM arithmetic {#round-two-the-ram-arithmetic}

With the right CUDA 13.5 libraries installed, the 5090 finally showed up to work --- 25 of its 32 GB in use. For a moment, that looked like progress. Then I looked at the other machine.

The split landed roughly 25 GB on the 5090 and handed the remaining 80 GB of weights to the RPC peer --- `msi`, which has 64 GB of RAM and about 62 of it usable. Doing what any literal-minded machine would do, it put roughly 39 GB of model weights into swap and started thrashing: 120 MB/s read, 127 MB/s write, 40% of the machine's time spent waiting on I/O. A ten-word test prompt took over thirty seconds and never finished. The forward pass wasn't slow; it was stalled, indefinitely.

Lesson two, which really should have been step zero: _do the RAM arithmetic for the placement you actually ran_. That split poured 108 GB of weights into two of the four pools --- the 32 GB card and `msi`'s 62 usable GB --- and that leaves no room for optimism, let alone for a key-value cache. The fleet's total memory was never the problem; the pools were.


## Round three: the physics {#round-three-the-physics}

One last swing at it: a three-way split. The 5090, `msi` across the wire, and hyperion's own 60-odd GB of RAM exposed as a third device through a loopback RPC server. On paper that is about 155 GB of addressable memory for 108 GB of weights --- room enough, finally, though it had been room enough on paper the whole time. In practice, the split is sized statically at load time, the GPU still needs 10 GB for working context and activations, and I had pinned `n_gpu_layers` at 99 --- every layer on the card --- which told the memory-fit routine to stand aside rather than help. The allocator then asked `cudaMalloc` for 40 GB on a 32 GB card, and seven hours and forty-five minutes after the server had started, it died with an out-of-memory error. The RPC server's log for that attempt is a row of identical lines --- _Accepted client connection / recv returned 0 (peer closed?) / Client connection closed_ --- the client dying again and again, politely.

Here is the honest truth I could have learned cheaper by thinking first: even if the split had loaded, slicing one model across a network means _every token waits for the slowest link_. 2.5 GbE sounds fast until you realise MoE routing turns the model weights into a stream, not a static pile. It would have been slow anyway. Distributed inference is a bandwidth problem wearing a compute costume.


## Why not just split the experts? {#why-not-just-split-the-experts}

The obvious question --- Part 3 already showed the trick. Strata is a MoE that doesn't fit in VRAM either, and it works by putting the hottest experts on the fast memory and the cold ones one step behind. Why not do the same thing across two machines: hot experts on the 5090, cold experts on `msi`?

Because llama.cpp's RPC mode doesn't have that knob, and the difference isn't cosmetic. `tensor-split` assigns whole tensors to devices at load time and treats remote memory as a synchronous execution unit: the compute graph waits on the peer once per transformer block, across the wire, regardless of which experts a token happens to route to. Strata's tiering works because the placement comes from the model's own routing statistics and every hop is on-board --- PCIe, nanoseconds, a routing table the local CPU consults itself. The RPC split has no routing statistics, no hot/cold arenas, no pinning by frequency profile. It is a static pile of tensors with an Ethernet cable running through the middle of the compute graph.

The runtimes that would do it properly exist --- expert-parallel cluster runtimes, or designs that ship only activations to wherever the routed experts live. That is a different class of software; llama.cpp RPC isn't a degraded version of it, it's a different shape entirely. The idea was sound and the tool was the wrong one, and it's worth writing down which one was wrong, because "the tool failed" and "the idea failed" get confused in most build logs --- including my own earlier ones.

Put that alongside the recap at the top. Strata is the same _kind_ of model --- a big MoE that does not fit in VRAM --- and it runs at 250 tokens per second on one machine. The difference is not the model or even the hardware. It is that Strata's tiers sit entirely on-board (VRAM, pinned RAM, PCIe), chosen with the model's own routing statistics, and sized so the slowest hop is measured in nanoseconds. The RPC experiment inverted every one of those choices: naive splitting, no routing intelligence, and an Ethernet cable dropped straight inside every forward pass. Same physics, opposite direction.

So we stopped. Strata came back online --- fast, healthy, vision included --- and the fleet went back to its normal shape.


## Bonus round: the little GPU that could {#bonus-round-the-little-gpu-that-could}

While the big gamble was dying, I had staged a small vision model (Qwen3-VL-8B) on the 8 GB 2080 SUPER. Handed a maths worksheet --- photographed, not typed --- it returned the right answers in under a second. It later failed spectacularly at identifying an engineering diagram, which keeps expectations honest. Still, for worksheet-in, answers-out, it is genuinely useful work out of a card I bought for a different decade entirely.


## What the bench kit said instead {#what-the-bench-kit-said-instead}

M2.7 never completed a bench run, so it stayed off the scoreboard. The bench kit ran against everything else that _could_ --- the resident Strata, Claude via API, and a parade of other local candidates. A few honest numbers:

| Task           | Strata (local)          | Claude (API) | Best other local |
|----------------|-------------------------|--------------|------------------|
| Pure maths     | **1 s**                 | 7 s          | 57 s             |
| Ti*k*Z diagram | 221 s                   | 69 s         | 491 s            |
| Exam paper     | 556 s (gave up mid-run) | **152 s**    | 698 s            |
| Lesson draft   | **142 s**               | 165 s        | 926 s            |

The split is plain enough. The local resident wins the short tasks and chokes on the long ones; the cloud entry wins the long ones --- and in one run died outright five seconds in with "Credit balance is too low". Both facts are in the table because both facts are true, and pretending either side is infallible helps nobody.


## What the gamble bought {#what-the-gamble-bought}

One Saturday, 108 GB of downloads, and zero dollars. What I actually got back was the fleet's real shape, measured instead of guessed at. The frontier-class long tail stays in the cloud; the overnight batch dream stays unproven; the 8 GB card is now officially a vision worker; and the rule for any future big-model ambition is three numbers on a napkin --- weights, RAM, link speed --- before a single download starts.

And to be fair to the idea: the thesis was not broken. 168 GB of raw memory for 108 GB of weights is enough to host this model, and the two machines genuinely had it. What broke is the distance between _having_ the memory and _spending_ it --- the software to turn four honest memory pools into one working set did not exist in the shape I had.

And the arrangement itself got its proof of work. The assistant ran the whole bench --- the download script, the installs, the servers, the logs, the timing table, the "Evaluated &amp; Retired" verdict in the bench notes --- while my contribution was the plan, the napkin arithmetic, the nvidia-smi glance that caught the silent fallback, and the stop calls. A lab partner that good doesn't make the experiment honest; it makes the failure legible, down to the megabyte, the moment it happens. That's the deal, and Saturday proved both halves of it.

The Eternity, it turns out, has a bandwidth limit.

_Next:_ [Part 5 — Tuning an 8 GB Card]({{< relref "20261122-ai-setup-5-tuning-an-8gb-card" >}}). _The little GPU's second act._
