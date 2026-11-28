+++
title = "Killing Google Analytics: GoatCounter on a teacher's site"
date = 2026-11-28
lastmod = 2026-11-28T11:17:24+11:00
tags = ["Blog", "Privacy"]
categories = ["Blog"]
draft = false
weight = 4001
author = "Matt Maguire"
+++

For a while this site ran Google Analytics, like half the web does. On 23 November I switched it off and replaced it with GoatCounter, a small open-source counter that sets no cookies. This is the reasoning, and what the first few days of honest numbers say about who actually reads a maths teacher's blog.


## Why GA came off a teacher's site {#why-ga-came-off-a-teacher-s-site}

I write about teaching, keyboards, Emacs and homelab. Students read it. That last sentence is most of the reason. A page a student might land on shouldn't be quietly wiring their browsing into Google's ad graph. GA4 also came with the EU cookie-consent mess — that "accept cookies" banner exists almost entirely because GA profiles people across sites. I don't want the banner, and I don't want what's behind it. So GA4 went, and with it the banner: no consent prompt is needed when nothing is being profiled.


## What GoatCounter is {#what-goatcounter-is}

GoatCounter is one script and one counter, open source, and you can self-host it if you want. I use the hosted version, which is free for a site this size. It records a page view and very little else: no cookies, no fingerprinting, no tracking across other sites. I switched off even the parts it offers. It logs "Chrome", not "Chrome 144.0.1", so the browser-plus-system-plus-screen-size combination can't be stitched into a fingerprint. Language collection is off. The whole dataset is a tally of paths and a rough country.


## The honest caveat {#the-honest-caveat}

Every number below includes me. I often refer to my own site, and I run an AI assistant that loads pages to confirm they work — those visits look like a Linux Chrome session and quietly pad the Linux column. GoatCounter has a self-visit switch (a small bookmarklet that flips a localStorage flag) so my own browsing stops counting, but the assistant's verification visits are harder to exclude cleanly. So read the absolute counts as "roughly, plus me and my tools." The shape — who reads this, and from where — is still real.


## What the first few days say {#what-the-first-few-days-say}

Over five days, 143 views. The readership is not what "maths teacher's blog" would predict.

| System  | Views |
|---------|-------|
| Windows | 56    |
| iOS     | 44    |
| Linux   | 17    |
| macOS   | 14    |
| Android | 12    |

More Linux views than macOS. The top referrers are Google, DuckDuckGo, and — this made me grin — my own `/tags/emacs` page pointing at other posts. The people landing here are the plain-text, Emacs, keyboard and homelab crowd; the maths is often how they found me, not why they stayed.

Then the geography. More views from Norway than from Australia, and Luxembourg sitting in the top six. That isn't cartography. Norway, France and Luxembourg are among the busiest VPN exit nodes on earth, and to a cookieless counter a reader on a VPN looks like they live in Oslo. So I appear to have a dozen Norwegians who are almost certainly American, and I find that delightful.


## What I don't have {#what-i-don-t-have}

No email capture, no session replay, no "user journey," no ad audiences. I can tell you that the corne keymap and the AI-setup series get read, that search and DuckDuckGo send people in, and that a suspicious number of you are on Linux. I cannot tell you anything about a specific reader. That's the point.


## The part that's actually easy {#the-part-that-s-actually-easy}

A teacher's site can know what it needs to know — which posts get read, where readers come from — without a surveillance stack bolted to it. The banner-free, cookie-free web isn't a sacrifice. It's just the analytics you use, minus the part you never needed.
