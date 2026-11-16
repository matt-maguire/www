+++
title = "My AI Teaching Setup, Part 3: The Organs"
date = 2026-11-16
lastmod = 2026-11-16T16:59:22+11:00
tags = ["AI", "LaTeX", "Teaching", "Emacs"]
categories = ["Blog"]
draft = true
weight = 4001
author = "Matt Maguire"
+++

[Part 2]({{< relref "20261116-ai-setup-2-the-fleet" >}}) was about models and harnesses. But a model in a harness is still just a clever voice. What turned the setup into a _system_ was the organs: the memory, the senses, and the reflexes bolted on around it. This is the final part, and it starts with a full circle.


## The knowledge base that was already ten years old {#the-knowledge-base-that-was-already-ten-years-old}

People ask how I got my teaching knowledge into the AI. The answer is anticlimactic: I didn't. I had spent a decade writing it in plain text — Org-mode notes, syllabus annotations, lesson reflections, the whole zettelkasten from [Part 0]({{< relref "20261116-ai-setup-0-plain-text" >}}).

What the system needed was a way to _search by meaning_ rather than by keyword, so the model can be asked "what have I written about students confusing gradient with rise-over-run?" and get my own past thinking back, cited. That is what a vector database does: it indexes text by what it means. Qdrant, running on the office box, holds the knowledge base — my notes, the NESA syllabus, the textbooks, the maths resources — and every agent in the house queries it the same way.

The decade of plain text paid for itself in an afternoon. No conversion project, no vendor export, no PDF archaeology. Point the index at the folders. Done.


## How the index works: the embedder {#how-the-index-works-the-embedder}

The trick starts with a small model called an **embedder**. Mine is nomic-embed-text — a few hundred megabytes, running on the office box without needing the GPU. You hand it a passage of text and it returns a list of 768 numbers. That is all it does, all day.

The numbers are not arbitrary. The model was trained on enormous piles of text under a simple rule: passages that mean similar things must come out with similar lists, and unrelated passages with lists that differ. Nobody hand-assigned a meaning to any of the 768 coordinates — the geometry of meaning was learned, the same way any neural network learns anything. And the property holds up in use: "straight-line depreciation" and "modelling money with graphs" land close together, and both land far away from "photosynthesis".

Feeding the database is then mechanical. Every document — a syllabus PDF, an Org file, a research paper — is cut into chunks of a few hundred words. Each chunk goes through the embedder and is stored with three things: its numbers, its original text, and its provenance — file path, page, year, course, type. The house currently holds just under 190,000 of these chunks.


## How search works: nearest neighbours {#how-search-works-nearest-neighbours}

Searching is the same trick aimed at a question. "What did the marker say students got wrong in financial modelling" appears nowhere in any document — but the embedder turns the **question** into 768 numbers too, and the database returns the chunks whose numbers sit closest to it. "Closest" is measured as an angle: passages that mean the same thing point in roughly the same direction, and the score says how tight the angle is, from 0 to 1.

The database part is what keeps this fast. Comparing a question against 190,000 chunks one by one is honest but slow at real scale, so Qdrant keeps a shortcut graph — every chunk linked to its nearest neighbours — and a search walks the map toward the answer instead of reading every shelf. (Honest aside: at 190,000 chunks, brute force is actually fine — milliseconds. The database earns its keep elsewhere: filters, incremental updates, snapshots, and a dozen agents querying at once.)

Scores are relative, not probabilities. 0.80 means _very close_, 0.66 means _related, go and check it yourself_. The ranking is the signal; the number is a hint.

{{< figure src="fig-pizza-search.png" >}}


## Versus just keeping the text files {#versus-just-keeping-the-text-files}

The fair question: I already had all this as text. Why a database?

grep — the plain-text search from Part 0 — is still in daily use, and it beats the vector index at one thing: exactness. Ask for "quadratic" and every occurrence of quadratic appears, all of them, no judgement required. The vector index answers the questions grep cannot: the ones where you do not know the words the answer uses. The marker's report never says "got wrong"; it says "in better responses, students were able to…". grep shrugs at that question. The index answers it.

And nothing about the plain-text life had to change. The files remain the source of truth — the index stores a copy of each chunk plus a pointer back to the real file. Delete the database and my notes lose nothing; one afternoon of re-embedding and the catalogue is back. The catalogue is disposable. The books are not.


## The shelves {#the-shelves}

Eight collections, just under 190,000 chunks:

-   **research** — 124,020 chunks of pedagogy: the academic papers and education books I have collected over the years
-   **maths_resources** — 27,144: a large external maths resource library — alternative assessments, activities, the hooks teachers across the state have built
-   **brain** — 17,793: my own notes and lesson files — "what did I do the last time I taught data analysis?"
-   **faculty_resources** — 12,338: what the teachers in my own faculty share with each other — lessons and resources written by colleagues
-   **nesa** — 2,759: the syllabus documents and the markers' feedback, tagged by course, year and type
-   **textbooks** — 4,614: exercises from the books actually in use in the school
-   **school_past_papers** — 566: my school's own past exams — what my students have already sat
-   **syllabus** — 485: outcomes and dot points, the small print that constrains everything else

Separate shelves matter because the agents search with filters: "marking feedback, Standard Mathematics, 2020 to 2025" is a different question from "everything, forever". And every laptop keeps a nightly read-only copy of the whole thing, because the school network blocks the house — the agents do not care which copy they ask.


## Whose computers are these? {#whose-computers-are-these}

One thing I have not mentioned, and it matters: where all of this physically lives. Every chunk in those eight shelves — the syllabus annotations, the markers' feedback, the school's own exams, a decade of my own notes — sits on computers I own, in my own house. The small model that turns text into numbers runs on the same hardware. No vendor indexes my teaching brain, no cloud service holds a copy, and no terms of service give anyone the right to train on any of it.

When the pipeline rents a frontier model from the cloud, that model sees only the handful of passages the agent hands it for the job at hand — never the library itself. And anything that identifies a student never leaves the house at all: the rented tier only ever receives student-neutral content.

The plain-text habit from Part 0 pays off here too. Because the files are mine — on my disks, in my backups, in a format every future tool can read — the index is a convenience I own, not a permission I rent.


## A live example {#a-live-example}

A real query, run against the live database as I write this. The question: _what did the marker say students got wrong in financial modelling_. The top three hits, with their scores:

-   0.69 — 2021 Standard, Question 19: "interpret information from the straight-line depreciation graph…"
-   0.68 — 2020 Standard, Question 30: "recognise that when 4 marks are allocated, a question requires multiple steps…"
-   0.66 — 2020 Standard, Question 13: "understand the difference between present and future value…"

The question shares almost no vocabulary with the answers. That is the entire point.

A second query, aimed at the research shelf — _research on using student-generated questions to deepen understanding_ — returns What Works Best at 0.80 and Killen's inquiry-questions activity at 0.80. (The same PDF appeared twice under two filenames — near-duplicates surface, and the agent reading the results notices and deduplicates. This is also what an unglamorous afternoon of housekeeping the library prevents.)

This is how the system pulls in facts. Exam generation asks the **nesa** shelf what the markers reward, and writes its marking criteria in the examiner's own voice. Lesson planning asks **maths_resources** and **faculty_resources** for hooks that actually exist rather than invented ones. Generated practice papers check **school_past_papers** so they sound like my school's papers and not a generic one. And **brain** answers "what did past-me already think about this?" — cited, with a file path.

The verification rule from Part 0 still holds all the way down here: the index says _where to look_, never _what to believe_. Every hit carries its provenance, and the agent opens the real file before it quotes one.


## The reflexes: never let the model do a human's job {#the-reflexes-never-let-the-model-do-a-human-s-job}

The organs that matter most are the ones that catch the model lying — and models lie with total confidence, in fluent sentences, about things you can check.

-   **SymPy does the maths.** No model computes an integral or solves an equation in this pipeline. Answers are verified symbolically by a computer-algebra system before anything reaches a student. A language model's idea of arithmetic is autocomplete with delusions of competence.
-   **LaTeX does the marks arithmetic.** Early drafts had the model adding up the marks per question, and it got them wrong often enough to be dangerous — an exam where the section header says 50 and the questions sum to 43. The fix was to stop asking the model to count at all: the document class now keeps a counter, every question registers its marks, and the total is _computed at compile time_. The model declares; LaTeX does the arithmetic. This single change eliminated a whole class of errors.
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
