+++
title = "Migrating my Blog from Wordpress to Org Mode"
date = 2024-12-26
lastmod = 2025-01-23T14:43:46+11:00
tags = ["Emacs", "OrgMode", "Esperanto", "Shavian", "𐑖𐑱𐑝𐑾𐑯"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

Lately I have been using Org Mode with Emacs quite a bit to help get better organised at work and keep track of meetings. The next logical step was to migrate my blog away from Wordpress and integrate it into my Org Mode workflow.

The benefits are:

<!--more-->

-   not having to host and maintain a Wordpress instance on my home computer, the with security risks that that entails.
-   a static website hosted on GitHub will generally load quicker, and is not dependent on a consumer-grade internet connection that goes down from time to time.
-   since I am already using Org Mode/Emacs in my day-to-day work, it is convenient having a common format for all my notes, be they personal or for publication.

Now that I am on the Christmas break, I have had some time to investigate the various options, and I have settled on using ox-hugo to export my Org Mode blog posts and documents to a Hugo website using the ''KeepIt'' theme. This theme has some nice features such as \\(\KaTeX\\) support for rendering maths equations and multi-lingual support for writing articles in other languages/scripts (such as Esperanto or the Shavian alphabet).

I've now reached the point where I think I can now decommission the Wordpress website, so let's see how we go!

If you feel like a bit of a chuckle, check out this video on the ubiquitousness of Emacs:

{{< youtube urcL86UpqZc >}}
