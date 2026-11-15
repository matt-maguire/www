+++
title = "From Lessons to Unit Planning"
featuredImage = "20260824-unit-planning.png"
date = 2026-08-24
lastmod = 2026-11-16T10:08:01+11:00
categories = ["Blog"]
draft = true
weight = 3001
author = "Matt Maguire"
+++

## Introduction: Moving from Micro to Macro {#introduction-moving-from-micro-to-macro}


### The Limitation of Isolated Lesson Prep {#the-limitation-of-isolated-lesson-prep}

-   Single-lesson generation solves daily grunt work, but teaching requires long-term coherence across a 4–6 week unit.
-   Cognitive overhead: Balancing scope &amp; sequence, textbook exercises, syllabus outcomes, and calendar constraints.


### Objective for Part 3 {#objective-for-part-3}

-   Elevating the local Goose / Gemma (and Claude) agent workflow from drafting single lessons to architecting entire units.
-   Leveraging agentic workflows for top-down sequencing, syllabus compliance auditing, differentiated material creation, and assessment design.


## Ingesting the Foundation: Syllabus, Program, and Textbook {#ingesting-the-foundation-syllabus-program-and-textbook}


### Feeding Multi-Modal Reference Material {#feeding-multi-modal-reference-material}

-   Staging syllabus outcomes, stage statements, and school scope-and-sequence documents.
-   Ingesting full textbook chapters using the `pdftopbm` OCR/image pipeline from Part 2.


### Generating the Unit Roadmap {#generating-the-unit-roadmap}

-   Structuring an $N$-lesson unit trajectory:
    -   Concept introduction &amp; diagnostic checks.
    -   Explicit instruction &amp; modeled examples.
    -   Guided practice and consolidation.
    -   Revision, application, and summative review.
-   Structuring the agent output: A clear, sequential unit timeline matrix.


## Automated Syllabus Gap Analysis {#automated-syllabus-gap-analysis}


### The Cross-Referencing Audit Loop {#the-cross-referencing-audit-loop}

-   Prompting the agent to audit the draft unit against exact syllabus dot points:

    > "Compare our 12 proposed lesson plans against Syllabus Outcome [X]. Identify any omitted sub-topics, unaddressed terminology, or missing question archetypes."


### Uncovering Common Blind Spots {#uncovering-common-blind-spots}

-   Catching subtle omissions: Edge-case problem types, specific reasoning/proof requirements, or prerequisite skills.


### Producing the Coverage Report {#producing-the-coverage-report}

-   Generating a Markdown/Org table showing each syllabus requirement mapped to specific lessons and activities before writing the LaTeX notes.


## Differentiated Resource Generation {#differentiated-resource-generation}


### The Three-Tier Scaffolding Model {#the-three-tier-scaffolding-model}

-   Transforming a single core mathematical concept into three leveled variations:
    -   **Foundation / Support:** Step-by-step scaffolds, simplified arithmetic, worked templates, and visual hints.
    -   **Core / Standard:** Standard syllabus problems, application questions, and routine multi-step calculations.
    -   **Extension:** Unscaffolded multi-step proofs, non-routine modeling, and challenging unfamiliar contexts.


### Diagrammatic Differentiation with TikZ {#diagrammatic-differentiation-with-tikz}

-   Maintaining visual consistency: Modifying TikZ geometric/statistical figures across tiers (e.g., adding angle hints/auxiliary lines for support vs. stripped diagrams for extension).


## Assessment Task &amp; Marking Guide Development {#assessment-task-and-marking-guide-development}


### Designing Balanced Assessment Tasks {#designing-balanced-assessment-tasks}

-   Constructing diagnostic quizzes and end-of-unit summative tasks mapped to taxonomy levels (Fluency, Understanding, Reasoning, and Problem Solving).


### Automated Solution Keys and Rubrics {#automated-solution-keys-and-rubrics}

-   Emitting complete, fully worked LaTeX solutions alongside question papers.
-   Generating explicit marking schemes: Allocating method marks (\\(M\\)), answer marks (\\(A\\)), notation criteria, and notes on common student misconceptions.


## Automated Math Verification &amp; Tool Integration {#automated-math-verification-and-tool-integration}


### Sanity-Checking Solutions via Python / SymPy {#sanity-checking-solutions-via-python-sympy}

-   Utilizing the agent's tool execution to pipe generated questions through SymPy/Python.
-   Ensuring arithmetic, integration/differentiation, and algebraic simplifications are algebraically verified prior to `latexmk` compilation.


### Batch LaTeX Pipeline {#batch-latex-pipeline}

-   Automating the compilation of teacher notes, student handouts, differentiated worksheets, and assessment files in one agentic loop.


## Conclusion and What's Next {#conclusion-and-what-s-next}


### The Shift in Teaching Workflow {#the-shift-in-teaching-workflow}

-   Moving from administrative content assembly to high-level instructional design, freeing up energy for active classroom pedagogy.


### Next Steps / Teaser {#next-steps-teaser}

-   Future explorations: Developing custom Model Context Protocol (MCP) servers, interactive classroom visualizers, or student feedback loops.
