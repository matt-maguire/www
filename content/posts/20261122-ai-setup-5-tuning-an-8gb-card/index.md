+++
title = "My AI Teaching Setup, Part 5: Tuning an 8 GB Card"
featuredImage = "20261122-ai-setup-5-tuning-an-8gb-card.png"
date = 2026-11-22
lastmod = 2026-11-23T09:15:24+11:00
tags = ["AI", "Hardware", "Teaching"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

There is a particular kind of trouble that starts with a reasonable question. I asked my AI assistant why the graphics card in one of my desktops was only 69% full, when I had deliberately squeezed a 19.8 GB model onto an 8 GB card and expected it to be tight. The answer came back fast and tidy: that is the ceiling, the expert layers live in system RAM, nothing more can fit. It was wrong, and it took me pushing back twice before the real answer showed up. The temptation with an assistant is to switch your brain off, because the answers arrive so easily. This story is an argument that the opposite is what pays: critical thinking becomes more important, not less, when the typing is someone else's job.


## The machine and the model {#the-machine-and-the-model}

The box is the one I called _msi_ in [Part 2]({{< relref "20261116-ai-setup-2-the-fleet" >}}): an Intel i9 Gen10 box with 64 GB of 2020-era DDR4, an RTX 2080 SUPER with 8 GB of VRAM, running day and night as the house gateway. The model is Qwen3.6-35B-A3B, a community build, quantised down to 19.8 GB on disk. It carries 35 billion parameters, but only about three billion of them take part in generating any one token. That gap is the whole trick, and it is what makes an 8 GB card worth arguing about.

The architecture is a mixture of experts. Each of the model's 40 layers carries 256 small feed-forward networks, the experts, and a router picks 8 of them per token. The shared parts of the model, the attention layers and embeddings, are small: about 2.6 GB. The experts are the bulk: about 17 GB. So llama.cpp splits them by location. The shared path lives on the graphics card; all 10,240 experts sit in system RAM, and for each token the CPU reads the eight chosen experts per layer across the memory bus. The 64 GB of RAM holds the expert weights with room to spare. The card holds the shared path and the context bookkeeping.

{{< figure src="fig-moe-split.png" >}}

This is not new territory for the blog. The first mixture-of-experts model on this same card was the Qwen3-30B-A3B bridge I mentioned in [AI Agents and Local LLMs]({{< relref "20260823-gemma-goose" >}}). What is new is that this time I tried to tune it properly, and the tuning went somewhere I did not expect.


## The measurement that measured the wrong thing {#the-measurement-that-measured-the-wrong-thing}

Before changing anything I wanted a baseline, so I wrote a small benchmark: ask the model to list the first 500 Fibonacci numbers, time it, divide. The script ran on the same machine as the model. It reported about 20 tokens per second, and a sweep over CPU thread counts seemed to show that 8 threads beat 10, and that 12 and 20 threads were much worse.

Then the benchmark client moved to a different machine and the number jumped to 36.7. The old numbers had been the sound of the measurement fighting the thing measured. The model is pinned to CPU cores 0 through 9, and my benchmark script, a python process and a curl, was competing for those same cores. Every thread-count conclusion from that era was noise. Measured cleanly, 8 threads versus 10 came out 36.75 against 36.62, a coin flip. The real ceiling was the DDR4 memory bandwidth, and no number of threads was ever going to move it.

Lesson one, learned the expensive way: get the observer off the machine you are measuring.


## "That is the ceiling" {#that-is-the-ceiling}

With the baseline honest, I asked the assistant to try putting some experts on the GPU instead of all in RAM, and watched the VRAM number to see what happened. It did not move at all. I tried a different value of the same setting. Still nothing.

That flat number is the whole reason for the pushback, and it is worth spelling out how it works, because it generalises to any work you do with these assistants. A change that moves weights onto the card has to move the memory number. When it does not, one of two things is true: there is a sound explanation for why the change cannot matter, or the change is not actually taking effect. Both are checkable, and neither is acceptable to skip. So my second question carried an ultimatum: either explain why the memory utilisation did not go up, or go and find out whether the setting was doing anything at all. A change that produces no observable effect is a fault report, and I treated it as one.

The first answer I got was the confident one quoted at the top, and it dodged the ultimatum quietly: it was the assistant answering from experience with models of this shape rather than from my particular file. Fair enough: nothing in my question told it to check the file. The fix was to ask something better. Pointed at the model's own header, the same assistant produced the ground truth in one pass: the model has 40 layers. The setting I had been tuning takes a layer count, and any value above 40 means exactly the same thing. My careful sweep of settings had been comparing identical configurations. Nothing had actually been tried, and the ceiling was a misread flag.

Once the real numbers were on the table the arithmetic was easy. Each layer's experts weigh about 428 MB at this quantisation, and the card had about 2.5 GB free. Five layers fit.


## Five layers on the GPU {#five-layers-on-the-gpu}

One flag change later: `-ncmoe 35`, meaning keep 35 layers' experts in RAM and put the other five on the card. VRAM went from 5640 to 7768 MiB, 94.8% full. Throughput went from 36.75 to 38.1 tokens per second. Three and a half percent, modest, real, and free.

One more lever was available and did not survive contact with the machine. The server log suggests a flag that loads the model into plain RAM instead of memory-mapping the file. With that flag the card's footprint grows slightly, and the margin it eats is exactly the margin the five expert layers needed. The first real request ran the server out of memory and it crash-looped until I reverted. The log's suggestion is written for machines where RAM is the tight resource. Mine is not.


## The 95% I remembered {#the-95-i-remembered}

After the change I noticed the GPU utilisation gauge sitting at 30 to 40% while the model generated text, where I remembered it sitting near 95%. That bothered me for exactly as long as it took to measure both configurations with the same sampler: 26% with all experts in RAM, 32% with five on the card. The gauge had gone _up_ with my tuning. The 95% I remembered was real, but it belonged to a different phase of the work.

Reading a prompt is batched matrix math, hundreds of tokens per kernel launch, and it pegs the card at 90 to 95%. Generating an answer is one token at a time: the GPU computes for about nine milliseconds, waits about eighteen for the CPU to fetch experts across the memory bus, and repeats. A half-idle card during generation is what this machine looks like when the bottleneck is the RAM bus rather than the silicon. The utilisation gauge measures how often a kernel is running, not how fast answers arrive. Tokens per second is the only number worth watching.


## What I would tell myself {#what-i-would-tell-myself}

| Mistake                            | What it cost                | What fixed it                           |
|------------------------------------|-----------------------------|-----------------------------------------|
| Benchmark client on the test box   | about 45% of measured speed | Run the client elsewhere                |
| Letting a no-op change pass        | A false ceiling, unnoticed  | Treat "nothing moved" as a fault report |
| Tuning a flag past the layer count | An afternoon                | Read the model file header              |
| Believing a confident first answer | The above, twice            | Ask again, with the numbers             |
| Trusting the utilisation gauge     | Worry, nothing              | Watch tokens per second                 |

The last row of that table is the one about the assistant, and I want to frame it fairly. The wrong answer was fluent, specific, and shaped exactly like a right answer, because the assistant was answering from experience with models of this shape. That is the fast, cheap first draft that makes the whole working relationship worth having. What it needs from me is scorekeeping. I noticed that a setting I had changed had not moved the number it was supposed to move, and I asked again with that observation attached. The assistant took the pushback without friction, went and read the file, handed back the correction, and then did the arithmetic, ran the experiment, and measured the result. Getting a good answer out of these models is mostly a skill with questions: point them at the ground truth, keep track of which numbers moved, and treat the first answer as a draft worth testing. The models are genuinely good at this work. The human gets the most out of them by staying in the loop with receipts. That is the key message, and it is the opposite of the fear these models usually inspire. The assistant takes over the typing and the tool-running, and that is exactly why the human has to keep doing the judging: deciding which answers deserve to be believed, and asking for the receipts. Switching your brain off is the one move that wastes the whole arrangement.

The card is now genuinely full at 94.8%, the model answers at about 38 tokens a second with a 192,000-token context window, and the next real gain will come from faster memory rather than from any flag. The 2080 SUPER keeps its job for now. It has earned it. And the method it taught me, ask, check the numbers, ask again, has turned out to be the best deal in the whole setup, assistant included.
