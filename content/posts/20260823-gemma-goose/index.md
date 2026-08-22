+++
title = "AI Agents and Local LLMs"
featuredImage = "20260823-gemma-goose.png"
date = 2026-08-23
lastmod = 2026-08-23T08:37:32+10:00
tags = ["AI", "LaTeX", "Teaching", "Emacs"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

In my [last article]({{< relref "index.md" >}}) on AI agents I talked about using Claude Code AI agent. This is an easy-to-set-up commercial solution that gives excellent results. The risk is that the pricing may not remain as affordable as it is today, and as a result I wanted to find an alternative solution that insulates me from changing commercials.

There are a number of open-weight models that one may download and run locally on your home computer. Some of the benefits are:

-   The are no per-token charges and no token limits -- you can consume as many tokens as you like once you have purchased your hardware.
-   Anything you type into your locally running LLM stays on your computer, alleviating any privacy concerns.
-   An external entity cannot suddenly "shut down" the particular LLM that you have come to rely on.
-   You do not need to rely on a network connection.
-   you do not need to share hardware with anyone else.

Of course, there are some drawbacks:

-   you need pretty beefy hardware specs to get good performance.
-   the models you can run are not quite as capable as the cloud-based models, and VRAM limitations may limit the size of the context window you can realistically use.


## Choosing a model {#choosing-a-model}

Based on my experience with using Claude, I found that I would typically use around 100k tokens to produce a set of lesson notes. My computer is a gaming PC equipped with a NVIDIA RTX 5900 that has 32GB of VRAM. The amount of VRAM determines the size of the model you can use and well as the length of the context window. I look for models that would give me a 128k+ context window and fit within the VRAM I had available. Of course, I asked an AI chat bot for some recommendations.

The first recommendation that came back was to use Ollama to run the models. By running a local Ollama server, it is simple to download different types of models and switch between them. The types of models that were recommended fell in two main categories:

-   **Dense models:** these models tend to be more powerful, but can also be slower
-   **Mixture of Experts:** also known as Mixture of experts, these consist of a number of smaller models. There is some logic up front which routes the query to one of the "experts" which specialise in certain tasks, and then your graphics card only need to run this smaller specialist model, making it faster.

Two models which stood out were _Qwen_ from the Chinese company Ali Baba, and _Gemma_ from Google's Deepmind. I experimented with variants from both of these families. In the end, I settled on Gemma, mainly because of its image processing capabilities (more on this later).

The two models I arrived at were:

-   gemma4:31b -- this is a dense model with 31 billion parameters. With my graphics card, I can comfortably run a context window of 128k. This is my daily driver.
-   gemma4:26b -- This is a Mixture of Experrts (MoE) model that takes up less memory. With this model I have increased my context wondow to 256k, so that I can trade a larger context window for a slightly less capable model.

Note that Ollama by default only uses a small context window. You can override the default context window by making a Modelfile with customise parameters, and then use Ollama to create a customised model that points back to the same weighting matrix file you already downloaded for the original model. You can ask your AI LLM how to do this. Once that was done, time to fire up the LLM:

```text
$ ollama run gemma4-31b-128k
>>> Hi Gemma! Are you ready to make some lesson files for me?
Thinking...
"Hi Gemma! Are you ready to make some lesson files for me?"
The user wants to create educational content (lesson files).
Friendly, eager, collaborative.

    *   Am I capable of creating lesson plans? Yes.
    *   Can I generate worksheets, quizzes, slides outlines, and instructional materials? Yes.
    *   What do I need from the user to be effective? Subject, grade level, learning objectives, duration, and specific
formats.

    *   *Enthusiasm:* Acknowledge the request with a positive tone.
    *   *Capability Statement:* Clearly state what can be done (lesson plans, quizzes, activities, etc.).
    *   *Requirement Gathering:* Ask for the necessary details to ensure the quality of the output.
...done thinking.

I am absolutely ready! I would love to help you put together some high-quality lesson files.

To make sure these are exactly what you need, just let me know a few details for each lesson:

...
```

As you can see, by default this LLM makes its internal thinking explicit. This is yet another parameter that can be adjusted.


## Choosing an AI Agent client {#choosing-an-ai-agent-client}

Once I had an Ollama server up and running, I wanted to set up an AI Agent like I did with Claude. On inspecting the Emacs _agent-shell_ package, I noticed it supports an AI Agent called _Goose_ by _Block Open Source_. This agent supports orchestration of complex multi-step tasks through the use of to-do lists. It can be run through Emacs agent-shell, or there is a desktop application that provides a user-friendly way of configuring and using the AI agent.

I tried making some lessons using the Goose agent. It worked quite well, very similar to Claude Code. I trained it on how to format lessons the way I like, just like I did with Claude. Like Claude Code, Goose has a skills extension that can be enabled.

One thing that didn't work so well was when I fed Goose/Gemma a source document containing maths notation and diagrams. It would try to extract the text, but formulas would become garbled, and content existing as images such as tables and diagrams would not be extracted at all. All this worked seemlessly with Claude, but I wasn't ready to give up yet.


## Importing Source/Reference Documents {#importing-source-reference-documents}

We now come back to the point that the Gemma model supports image processing. Rather than using an unreliable text extraction tool, I looked for ways to leverage the image processing capabilities of Gemma to inspect the source document and visually recognise the maths notation, tables and diagrams. I installed a program called `pdftopbm`, which coverts each page of a PDF document into a Portable Bitmap file that Gemma can recognise.

Of course, I don't want to have to convert all the pages of a PDF file manully, especially if I am only interested in a small selection of pages. How to automate that process? If only there way some sort of AI tool that could automate that sort of grunt work... oh wait, isn't that what Goose/Gemma is for?

I instructed Gemma to always use `pdftopbm` to extract pages from any PDF document I told it about, instead of using text extraction. I made this persistent as part of a skill file, and now I just drop the PDF into my lesson directory, tell Gemma where the file it, and through Goose it will find the PDF and ingest it as needed.

I later found out that this is what tools like Claude do behind the scenes --- convert to images and use OCR.

I now had a quite powerful AI Agent setup running completely locally on my gaming PC. As long as I was at home, I could ssh into my gaming PC and run emacs in text mode. But, what if I wanted to view PDF output, or access the LLM when away from home? Surely there must be a way to make this work over the internet...


## Connecting the LLM to the Internet {#connecting-the-llm-to-the-internet}

When using the Goose desktop app, you need to point it towards an API endpoint through which it can access an LLM. The Ollama server running on my gaming PC listens on a TCP port for incoming HTTP requests. Normally it binds to the local loopback device, so that you can only connect to this API locally from the gaming PC itself. The question is, how can I expose this API on the internet in a safe and secure way.

The Ollama server provides an OpenAI-compatible API endpoint, with a couple of limitations.

1.  The API is accessed via HTTP requests, which are not encrypted. Sending unencrypted information over the internet is not recommended.
2.  The API endpoint does not support any authentication. This would mean anyone on the internet would be able to access the model.

The solution to this is of course to implement a reverse proxy. The reverse proxy can receive encrypted HTTPS connections, and forward them to the HTTP API port on the Ollama server. It can also take care of the authentication function to ensure that only authorised clients can access the API. This can easily be done with software like _nginx_ or _apache httpd_. This is a very common type of setup that is readily discovered by web search or asking your AI, so I won't duplicate that information here.

The final step is to point Goose to the HTTPS API endpoint, and configure an API key that the reverse proxy will accept.


## Next Steps {#next-steps}

Having set up my home infrastructure to be readily accessible from anywhere, I now have a "free" alternative to using the paid AI services (I say free, but of course I still pay electricity and hardware depreciation costs). The home setup produces high quality lesson notes documents. For cases where the home setup struggles, I still have the option of using the more powerful Claude or Gemini paid models.

So, what else can I do with this setup? That of course will be the subject of the next article.
