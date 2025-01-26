+++
title = "CSQ Summary"
date = 2013-03-12
lastmod = 2025-01-26T11:58:28+11:00
tags = ["HamRadio"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

## CSQ Summary {#csq-summary}

The following is a proposal from Bruce Prior N7RR for a signal reporting system based on [Copyability-Strength-Quality (CSQ)](http://antrak.org.tr/index.php?option=com_content&task=view&id=378&Itemid=83), and as an alternative to the traditional Readability-Strength-Tone (RST) system.


### C or Copyability {#c-or-copyability}

| Copyability | Description                                       |
|-------------|---------------------------------------------------|
| N           | no recoverable signal \\(^{\*}\\)                 |
| 0           | discernable but not copyable \\(^{\*}\\)          |
| 1-9         | 10% to 90% copy                                   |
| G           | Good 100% copy, but short of perfect              |
| P           | Perfect armchair 100% copy or full quieting on FM |

\\(^{\*}\\) For Copyability reports of N and 0, no Signal Strength or Quality reports are needed.


### S-Meter or Signal Strength {#s-meter-or-signal-strength}

| Strength | Description             |
|----------|-------------------------|
| 0        | no S-meter reading      |
| 1-9      | S-1 to S-9              |
| A        | 1 dB to 10 dB over S-9  |
| B        | 11 dB to 20 dB over S-9 |
| C        | 21 dB to 30 dB over S-9 |
| D        | 31 dB to 40 dB over S-9 |
| E        | 41 dB to 50 dB over S-9 |
| F        | 51 dB or more over S-9  |


### Quality {#quality}

| Quality | Description                                                              |
|---------|--------------------------------------------------------------------------|
| X       | characteristic steadiness of crystal (Xtal) control or eXcellent quality |
| R       | AC Ripple or buzz in transmission                                        |
| C       | Chirp or tail on make and/or break                                       |
| K       | key clicKs or other Keying transients                                    |
| O       | Overmodulation or Overdeviation in phone or digital modes                |


### Examples {#examples}

| Report | Description                                                                        |
|--------|------------------------------------------------------------------------------------|
| P6O    | for a PSK-31 signal: perfect 100 % copy at S-6, but overdeviated                   |
| 93X    | for a CW signal: 90 % copy at S-3 with excellent quality                           |
| G7O    | for an SSB signal: Good but less than perfect 100 % copy at S-7, but overmodulated |
| PAX    | for an RTTY signal: perfect 100 % copy about 10 dB over S-9 with excellent quality |
| P6X    | for an FM signal: full-quieting 100 % copy at S-6 with excellent quality           |


## RST Summary {#rst-summary}

For comparison with the proposed CSQ system, here are the definitions for the traditional RST system.o


### R or Readability {#r-or-readability}

| Readability | Description                                       |
|-------------|---------------------------------------------------|
| 1           | Unreadable                                        |
| 2           | Barely readable, occasional words distinguishable |
| 3           | Readable with considerable difficulty             |
| 4           | Readable with practically no difficulty           |
| 5           | Perfectly readable                                |


### Strength {#strength}

| Strength | Description                      | HF Signal | V/UHF Signal |
|----------|----------------------------------|-----------|--------------|
| 1        | Faint signal, barely perceptible | -121 dBm  | -141 dBm     |
| 2        | Very weak                        | -115 dBm  | -135 dBm     |
| 3        | Weak                             | -109 dBm  | -129 dBm     |
| 4        | Fair                             | -103 dBm  | -123 dBm     |
| 5        | Fairly good                      | -97 dBm   | -117 dBm     |
| 6        | Good                             | -91 dBm   | -111 dBm     |
| 7        | Moderately strong                | -85 dBm   | -105 dBm     |
| 8        | Strong                           | -79 dBm   | -99 dBm      |
| 9        | Very strong signals              | -73 dBm   | -93 dBm      |


### Tone {#tone}

| Tone | Description                                                |
|------|------------------------------------------------------------|
| 1    | Fifty cycle a.c or less, very rough and broad              |
| 2    | Very rough a.c., very harsh and broad                      |
| 3    | Rough a.c. tone, rectified but not filtered                |
| 4    | Rough note, some trace of filtering                        |
| 5    | Filtered rectified a.c. but strongly ripple-modulated      |
| 6    | Filtered tone, definite trace of ripple modulation         |
| 7    | Near pure tone, trace of ripple modulation                 |
| 8    | Near perfect tone, slight trace of modulation              |
| 9    | Perfect tone, no trace of ripple or modulation of any kind |

The following letters can be optionally suffixed:

| Suffix | Description                           |
|--------|---------------------------------------|
| X      | stable frequency (crystal control)    |
| C      | "chirp" (frequency shift when keying) |
| K      | key clicks                            |


## RSQ Summary {#rsq-summary}

For digital work, the Readability-Strength-Quality signal reporting system is sometimes used.


### R or Readability {#r-or-readability}

| Readability | Description                                                  |
|-------------|--------------------------------------------------------------|
| 1           | 0 % copy - undecipherable                                    |
| 2           | 20 % copy -occasional words distinguishable                  |
| 3           | 40 % copy - readable with difficulty, many missed characters |
| 4           | 80 % copy - Readable with no difficulty                      |
| 5           | 95 % + copy - Perfectly readable                             |


### Strength {#strength}

| Strength | Description              |
|----------|--------------------------|
| 1        | barely perceptible trace |
| 3        | Weak trace               |
| 5        | Moderate trace           |
| 7        | Strong trace             |
| 9        | Very strong trace        |


### Quality {#quality}

| Quality | Description                                 |
|---------|---------------------------------------------|
| 1       | splatter over much of the spectrum          |
| 3       | multiple visible pairs                      |
| 5       | One easily visible pair                     |
| 7       | One barely visible pair                     |
| 9       | Clean signal - no visible unwanted sidebars |
