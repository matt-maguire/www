+++
title = "Island Song"
featuredImage = "20250302-island_song_still.png"
date = 2025-03-02
lastmod = 2025-03-02T17:39:38+11:00
tags = ["Music"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

When I was in primary school, I learned the recorder. The teacher librarian taught us, even though she did not have any musical training herself. We were then told that we would be performing at the Sydney Opera House together with a number of other schools. It was very exciting, and our little group worked hard to try and play together as an ensemble. As we got closer to the time, a specialist music teacher was brought in to try and get us in better shape. I learned so much from those couple of lessons, and we got to perform in the Opera House.

One of the pieces we played was _Island Song_, written by Michael Hannan in 1983. There were 5 parts, with the melody being played by the "Descant 1" (soprano) group -- that was my group. The "Descant 2" and "Descant 3" groups played the harmony, and there was a Treble (Alto) part and a Tenor part. The treble recorder had the "wrong notes" on the hand-written score we were given -- I now know that this was because the treble recorder is in the key of F, whereas the others were all in the key of C. Our treble player somehow managed to play the right notes in the end.

During the COVID lockdown, I came across the yellowed hand-written pieces of paper, and decided to revisit that part of my childhood. The first thing was to typeset the score in order to preserve it and make it easier to read. I used a program called [LilyPond](https://lilypond.org), in which you describe the notes in a plain text format, and it converts the notes to a pdf file.

The lilypond source file first defines some general parameters such as the title, composer, tempo, etc. and then describes the notes that make up each part. At the end, the parts are combined together to produce a single score. Below I have posted the source in a single monolithic file, although lilypond does allow you to split out each part into its own separate file. LilyPond can also generate a MIDI file that can be played on the computer using a suitable MIDI player.

```text

\version "2.20.0"

\header {
  title = "Island Song"
  composer = "Michael Hannan (1983)"
}

global= {
  \time 4/4
  \key c \major
  \tempo "Preciso" 4 = 54
}

descantOne = \new Voice \relative c'' {
% Page 1
  r1 | r1 | r1 |
  r1 | r4 g8\mf ( b8) r4 a8( f8) | r4 g8( b8) r4 f8( d8) |
  r4 g16( a16 b8) r4 a8( f8) | r4 g16( f16 g8) r4 f16( c16 d8) | r8^"espressivo"\mf d'8-- d8-- [d8--] r8 d8( c4) |

% Page 2
  r8 d8-- d8-- [d8--] d16( e16 f8~ f4) | r8 d8( c8 [b8]) r8 d8( c4) | r8 d8-- d8-- [d8--] d16( f16 g8) r4 |
  r8 g8-- g8-- [g8--] g8( a16 f16~ f4) | r8 g8-- g8-- [g8--] g16( a16 bes8~ bes4) | r8 g8( f8 [e8]) r8 g8( f4) |
  r8 d16( e16 f16 e16 d8) c8( f8-.) f4 | r4 g2( f4) | r4 g4(~ g8 a8 f4) |

% Page 3
  r8 g8-- g8-- [g8--] g8( a16 f16~ f4) | r8 g8-- g8-- [g8--] g16( f16 d8) r4 | r4 d2( c4) |
  r4 d4(~ d8 e8 c4) | r8 d8-- d8-- [d8--] d8( e16 c16~ c4) | r8 d16( e16 f16 e16 d8) c8( f8-.) f4 |
  r1 | r1 | r1 |

% Page 4
  r1 | g,16-.\pp  g16-. r8 e'16-. e16-. r8 a,16-. a16-. r8 d16-. d16-. r8 | f,16-. f16-. r8 f'16-. f16-. r8 f,16-. f16-. r8 f'16-. f16-. r8 |
  g,16-. g16-. r8 e'16-. e16-. r8 a,16-. a16-. r8 d16-. d16-. r8 | f,16-. f16-. r8 f'16-. f16-. r8 g,16-. g16-. r8 d'16-. d16-. r8 | r8^"espressivo"\mf d8-- d8-- [d8--] r8 d8( c4) |
  r8 d8-- d8-- [d8--] d16-.(e16 f8~ f4) | r8 d8( c8 [b8]) r8 d8( c4) | r8 d8( e8 [f8]) g16( f16 g8-.) r4 |

  \bar "|."
}

descantTwo = \new Voice \relative c'' {
% Page 1
  r1 | r1 | r1 |
  r1 | r1 | r1 |
  r1 | r1 | r8_\markup {\italic "sempre" \dynamic p} b16-. b16-. r8 b16-. b16-. r8 a16-. a16-. r8 a16-. a16-. |

% Page 2
%  \set countPercentRepeats = ##t
  \repeat percent 9 { r8 b16-. b16-. r8 b16-. b16-. r8 a16-. a16-. r8 a16-. a16-. | }

% Page 3
  \repeat percent 6 { r8 b16-. b16-. r8 b16-. b16-. r8 a16-. a16-. r8 a16-. a16-. | }
  r8_\p c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. | r8 c16-. c16-. r8 c16-. c16-. r8 b16-. b16-. r8 b16-. b16-. | r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. |

% Page 4
  r8 c16-. c16-. r8 c16-. c16-. r8 b16-. b16-. r8 b16-. b16-. | r8_\p c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. | r8 c16-. c16-. r8 c16-. c16-. r8 b16-. b16-. r8 b16-. b16-. |
  r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. r8 c16-. c16-. | r8 c16-. c16-. r8 c16-. c16-. r8 b16-. b16-. r8 a16-. a16-. |
  \repeat percent 3 {r8 b16-. b16-. r8 b16-. b16-. r8 a16-. a16-. r8 a16-. a16-. |}
  r8 b16-. b16-. r8 b16-. b16-. a8( b8-.) r4 |

  \bar "|."
}

descantThree = \new Voice \relative c'' {
% Page 1
  r1 | r1 | r1 |
  r1 | r1 | r1 |
  r1 | r1 | r8_\markup { \italic "sempre" \dynamic p} g16-. g16-. r8 g16-. g16-. r8 f16-. f16-. r8 f16-. f16-. |

% Page 2
  \repeat percent 9 { r8 g16-. g16-. r8 g16-. g16-. r8 f16-. f16-. r8 f16-. f16-. | }

% Page 3
  \repeat percent 6 { r8 g16-. g16-. r8 g16-. g16-. r8 f16-. f16-. r8 f16-. f16-. | }
  r8_\markup {\dynamic p} e16-. e16-. r8 e16-. e16-. r8 fis16-. fis16-. r8 fis16-. fis16-. | r8 a16-. a16-. r8 a16-. a16-. r8 g16-. g16-. r8 g16-. g16-. | r8 e16-. e16-. r8 e16-. e16-. r8 fis16-. fis16-. r8 fis16-. fis16-. |

% Page 4
  r8 a16-. a16-. r8 a16-. a16-. r8 g16-. g16-. r8 g16-. g16-. | r8_\markup {\dynamic p} e16-. e16-. r8 e16-. e16-. r8 fis16-. fis16-. r8 fis16-. fis16-. | r8 a16-. a16-. r8 a16-. a16-. r8 g16-. g16-. r8 g16-. g16-. |
  r8 e16-. e16-. r8 e16-. e16-. r8 fis16-. fis16-. r8 fis16-. fis16-. | r8 a16-. a16-. r8 a16-. a16-. r8 g16-. g16-. r8 fis16-. fis16-. |
  \repeat percent 3 {r8 g16-. g16-. r8 g16-. g16-. r8 f16-. f16-. r8 f16-. f16-. |}
  r8 g16-. g16-. r8 g16-. g16-. f8( g8-.) r4 |

  \bar "|."
}

treble = \new Voice \relative c'' {
% Page 1
  r1 | r1 | r1 |
  r1 | r4 b8\mf( e8) r4 c8( a8) | r4 b8( e8) r4 c8( a8) |
  r4 b16( c16 d8) r4 c8( a8) | r4 b16( a16 b8) r4 c16( g16 a8) | r4 b8( d8) r4 e8( c8) |

% Page 2
  r4 b8( d8) r4 f8( c8) | r4 b8( d8) r4 e8( c8) | r4 b8( d8) r4 a8( c8) |
  r4 b8( d8) r4 e8 (c8) | r4 b8( d8) r4 f8( c8) | r4 b8( d8) r4 e8( c8) |
  r4 b8( d8) r4 b8( c8) | r4 b8( d8) r4 e8( c8) | r4 b8( d8) r4 f8( c8) |

% Page 3
  r4 b8( d8) r4 e8( c8) | r4 b8( d8) r4 a8( c8) | r4 b8( d8) r4 e8( c8) |
  r4 b8 ( d8) r4 f8 (c8) | r4 b8( d8) r4 e8( c8) | r4 b8( d8) r4 b8( c8) |
  r8^"espressivo"\mf e16( f16 g16 f16 e8) a,8( d8-.) d4 | r8 f16( g16 a16 g16 f16 e16) d8( b8-.) g4 | r8 e'16( f16 g16 f16 e8) a,8( d8-.) d4 |

% Page 4
  r8 f16( g16 a16 b16 c8) g16( a16 b16 c16 d8) r8 | r8\mf e,16( f16 g16 f16 e8) a,8( d8-.) d4 | r8 f16 g16 a16 g16 f16 e16 d8( b8) g4 |
  r8 e'16( f16 g16 f16 e8) a,8(d8-.) d4 | r8 f16( g16 a16 b16 c8) g16( a16 b16 c16 d4) | r8 b,16( c16 d16 c16 b8) c8( f8-.) f4 |
  r8 b,16( c16 d16 c16 b8) a8( f8-.) f4 | r8 d'16( e16 f16 e16 d8) c8( f8-.) f4 | r8 b,16( c16 d16 c16 b8) a8( b8-.) r4 |

  \bar "|."
}

tenor = \new Voice \relative c'' {
% Page 1
  g8-._\markup {\italic "sempre" \dynamic mf} g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 d16( c16 d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 |
  g8-. g8 r4 f8( d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 d16( c16 d8) r4 |
  g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 f8( d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 |

% Page 2
  g8-. g8 r4 d16( c16 d8) r4 | g8-. g8 r4 f16-.( a16-. c8) r4 | g8-. g8 r4 f8 (d8) r4 |
  g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 d16( c16 d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 |
  g8-. g8 r4 f8( d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 d16( c16 d8) r4 |

% Page 3
  g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 f8( d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 |
  g8-. g8 r4 d16( c16 d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 | g8-. g8 r4 f8( d8) r4 |
  c8-. [c8-.] e8( g8) d16-.( fis16-. a8) r4 | f8-. [f8-.] a8( c8) g16-.( b16-. d8) r4 | c,8-. [c8-.] e8( g8) d16-.( fis16-. a8) r4 |

% Page 4
  f8-. [f8-.] a8( c8) d16( b16 g8) b16( g16 f16 d16) | c8-.\mf [c8-.] e8( g8) d16-.( fis16-. a8) r4 | f8-. [f8] a8( c8) g16-.( b16-. d8) r4 |
  c,8-. [c8-.] e8( g8) d16-.(fis16-. a8) r4 | f8-. [f8-.] a8( c8) d16( b16 g8) a16( fis16 d8) | g8-. g8 r4 f16-. a16-. c8 r4 |
  g8-. g8 r4 d16( c16 d8) r4 | g8-. g8 r4 f16-. a16-. c8 r4 g8-. g8 r4 f8( g8-.) r4 |

  \bar "|."
}

\score {
  \new StaffGroup <<
    \new Staff \with { instrumentName = "D1" }
    << \global \descantOne >>
    \new Staff \with { instrumentName = "D2" }
    << \global \descantTwo >>
    \new Staff \with { instrumentName = "D3" }
    << \global \descantThree >>
    \new Staff \with { instrumentName = "Tr" }
    << \global \treble >>
    \new Staff \with { instrumentName = "Te" }
    << \global \tenor >>
  >>
  \layout { }
  \midi { }
}
```

The resulting pdf file containing the full score looks like this:

{{< embed-pdf url="20250302-island_song.pdf" >}}

You can also use lilypond to generate each part separately, so that each musician has a more compact set of sheets with only their part, meaning they need to turn the page less often.

The next step was to record each part. I used a metronome in my earbuds to try to keep each part in time. I used either a Canon M50 or a Sony A7R4 camera (I forget which) with a shotgun microphone to make the recordings. I then combined the footage of the separate parts into a single video using the [Kdenlive](https://kdenlive.org/) non-linear editor (NLE) on my Linux laptop. The result came out as follows:

{{< youtube 4Wa4eRCoskg>}}

I think it starts out ok, but later in the piece the timing seems a bit off and it starts to fall apart a bit. Even though the result was not great, it was my first attempt at making such a video and I had a lot of fun reliving my days of playing the recorder in the Sydney Opera House :-)
