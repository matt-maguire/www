+++
title = "Learn Japanese with Song Lyrics (Houki Boshi from Bleach)"
date = 2022-02-14
lastmod = 2025-01-23T18:19:59+11:00
tags = ["Music", "Languages", "Japanese"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

A great way to learn a language is through songs. They are usually repetitive, often (but not always) a bit slower than regular speech, and have a catchy tune. This helps to cement words and structures into the head.

I was watching an anime called [Bleach](https://en.wikipedia.org/wiki/Bleach_(TV_series)), and maybe a season into it they had a song at the end of each episode named [Houki Boshi](https://www.youtube.com/watch?v=2xs_-nl6C3E) (ほうき星) by the singer-songwriter [Younha](https://en.wikipedia.org/wiki/Younha). She's actually Korean, but released an album [Go! Younha](https://en.wikipedia.org/wiki/Go!_Younha) in Japanese, and _Houki Boshi_ was one of the hit singles on that album.

There is a live performance of the song on YouTube, which has Japanese subtitles:

{{< youtube qaUvEOeFthE >}}

Subtitles are a great resource for language learners. One of the challenges with Japanese of course is the unfamiliar writing system. These subtitles contain kanji, the Chinese-style characters that Japanese often use in their writing. One problem with kanji is that it can be difficult to work out how a kanji character is pronounced -- and the pronounciation this is often context-dependent in Japanese. One way to deal with this is to write the pronunciation above the kanji using small hiragana characters -- the hiragana provides an unambiguous guide as to how the character is pronounced. We call these little hirigana pronunciation guides _furigana_, and they are often used in children's books to help the cildren learn the pronunciation of the kanji.

The video above doesn't have furigana, so in order to become more familiar with the kanji, I decided to write out the song lyrics and include the furigana. For this I used the \\(\LaTeX\\) typesetting program with the [leadsheets](https://ctan.org/pkg/leadsheets) songbook package, and the _ruby_ package to add the furigana. Because I typed in the Japanese with unicode, I needed to use `xelatex` to build the PDF. I found [here on the web](https://www.animelyrics.com/anime/bleach/houkiboushi.jis.txt) an English translation to help remind myself what the various words mean. The final result is here:

{{< embed-pdf url="20220214-houki_boshi.pdf" >}}

As I typed up the lyrics, I looked up any words I wasn't familiar with, and studied the gramatical structures that were used (here the English translation was helpful as I could see how a particular nuance in English can be expressed in Japanese). You may notice that there is no romaji (writing Japanese with the Roman alphabet). When learning Japanese, it is recommended to learn the kana as quickly as possibly. When you write Japanese with Roman letters: it can obscure the way words change as you conjugate, it is very hard to read, and if you see real Japanese around it is almost never written in romaji. Therefore, it is best to ditch it as soon as possible.

You may notice that the live performance above leaves out the first two lines of the second verse -- I presume this was to shorten the performance for television. In the bleach anime, the song is even more abridged -- only the first and last line of the first verse is sung, followed by the final chorus.

Studying song lyrics is a great way to gain more exposure to a language, and is lots of fun!
