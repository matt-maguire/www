+++
title = "Maths Extension 1: Integration — complete notes with worked examples"
date = 2026-11-23
lastmod = 2026-11-23T15:04:20+11:00
tags = ["Maths", "Teaching", "HSC"]
categories = ["Blog"]
draft = false
weight = 4001
description = "Maths Extension 1 integration notes: substitution, trig integrals, inverse-trig forms and volumes of revolution, with worked examples and the traps NESA examiners actually report."
toc = true
comment = false
+++

These are the integration notes I wish every Extension 1 student had read
before the Year 12 calculus block: the techniques the syllabus actually
tests, worked examples with the marking commentary baked in, and the traps
that cost the most marks according to NESA's own marking feedback. They
follow the _Further applications of calculus_ topic of the [NSW Mathematics Extension 1 syllabus](https://curriculum.nsw.edu.au/learning-areas/mathematics/mathematics-extension-1-2020/course).
None of these techniques are on the NESA reference sheet — you have to own
them.


## The one-line version of every technique {#the-one-line-version-of-every-technique}

| Technique          | Spot it when you see            | What you do                        |
|--------------------|---------------------------------|------------------------------------|
| Linear composition | \\(f(ax+b)^n\\) style           | Divide by \\(a\\)                  |
| Substitution       | A function _and_ its derivative | Let \\(u\\) be the inner function  |
| Double angle       | \\(\sin^2 x\\), \\(\cos^2 x\\)  | Linearise first                    |
| Logarithmic form   | \\(\frac{f'(x)}{f(x)}\\)        | \\(\ln\lvert f(x)\rvert + C\\)     |
| Inverse trig forms | \\(\frac{1}{a^2 \pm x^2}\\)     | \\(\tan^{-1}\\) or \\(\sin^{-1}\\) |

Everything below is one of these five wearing a disguise.


## Why the \\(+C\\) is not pedantry {#why-the-plus-c-is-not-pedantry}

An antiderivative is a _family_: if \\(F'(x) = f(x)\\), then every function
\\(F(x) + C\\) differentiates back to \\(f(x)\\). In differential-equation
questions, NESA marking guidelines award "obtains correct primitive" and
"evaluates the constant of integration" as _separate_ criteria — the constant
is a whole mark. In a definite integral there is no constant to forget, which
is exactly why substitution with changed limits (below) is the safer road.


## Technique 1: the linear composition rule {#technique-1-the-linear-composition-rule}

If \\(a \ne 0\\):

\\[ \int f(ax + b)\\,dx = \frac{1}{a}F(ax + b) + C \\]

where \\(F' = f\\). For example:

\\[ \int \cos\left(3x - \tfrac{\pi}{6}\right)\\,dx = \tfrac{1}{3}\sin\left(3x - \tfrac{\pi}{6}\right) + C \\]

The most common wrong answer in this block is forgetting the \\(\frac{1}{a}\\):
differentiating \\(\sin(3x)\\) gives \\(3\cos(3x)\\), so integrating \\(\cos(3x)\\)
must divide by 3. Check every answer by differentiating it back — ten seconds,
and it is how the best students self-mark during an exam.


## Technique 2: substitution {#technique-2-substitution}

_Indefinite._ \\(\displaystyle\int x(3x^2 + 5)^4\\,dx\\).

The derivative of \\(3x^2+5\\) is \\(6x\\), and an \\(x\\) is sitting right there.
Let \\(u = 3x^2 + 5\\), so \\(du = 6x\\,dx\\) and \\(x\\,dx = \tfrac{du}{6}\\):

\\[\begin{aligned}
\int x(3x^2+5)^4\\,dx
&= \frac{1}{6}\int u^4\\,du \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{u^5}{30} + C \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{(3x^2+5)^5}{30} + C
\end{aligned}\\]

_Definite — change the limits, every time._ \\(\displaystyle\int\_0^3 2x\sqrt{x^2+1}\\,dx\\).

Let \\(u = x^2 + 1\\), \\(du = 2x\\,dx\\). When \\(x = 0\\), \\(u = 1\\); when
\\(x = 3\\), \\(u = 10\\):

\\[\begin{aligned}
\int\_0^3 2x\sqrt{x^2+1}\\,dx
&= \int\_1^{10} u^{1/2}\\,du \\\\
&= \rule[-0.5em]{0pt}{2.6em}\left[\frac{2}{3}u^{3/2}\right]\_1^{10} \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{2}{3}\left(10\sqrt{10} - 1\right)
\end{aligned}\\]

NESA's own syllabus support material singles this out: when substituting in a
definite integral, change the boundary limits _as well as_ the variable.
Converting back to \\(x\\) and reusing the old limits is where sign and
arithmetic errors breed. Change the limits and the old variable never comes
back to cause trouble.


## Technique 3: \\(\sin^2 x\\) and \\(\cos^2 x\\) — linearise first {#technique-3-sin-2-x-and-cos-2-x-linearise-first}

You cannot integrate \\(\sin^2 x\\) directly. Use the double-angle identities to
make it linear:

\\[ \sin^2 x = \frac{1 - \cos 2x}{2} \qquad \cos^2 x = \frac{1 + \cos 2x}{2} \\]

\\[\begin{aligned}
\int\_0^{\pi/2} \sin^2 x\\,dx
&= \int\_0^{\pi/2} \frac{1-\cos 2x}{2}\\,dx \\\\
&= \rule[-0.5em]{0pt}{2.6em}\left[\frac{x}{2} - \frac{\sin 2x}{4}\right]\_0^{\pi/2} \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{\pi}{4}
\end{aligned}\\]

The same integrand shows up inside volumes of revolution, so make the
linearisation automatic rather than a rescue attempt under time pressure.


## Technique 4: the two forms students meet too late {#technique-4-the-two-forms-students-meet-too-late}

These appear in the syllabus support examples and have been showing up in
HSC questions since 2020:

\\[ \int \frac{dx}{a^2 + x^2} = \frac{1}{a}\tan^{-1}\frac{x}{a} + C \qquad \int \frac{dx}{\sqrt{a^2 - x^2}} = \sin^{-1}\frac{x}{a} + C \\]

\\(\displaystyle\int \frac{dx}{\sqrt{9 - 4x^2}}\\): let \\(u = 2x\\), \\(du = 2\\,dx\\):

\\[\begin{aligned}
\int \frac{dx}{\sqrt{9-4x^2}}
&= \frac{1}{2}\int \frac{du}{\sqrt{9 - u^2}} \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{1}{2}\sin^{-1}\frac{u}{3} + C \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{1}{2}\sin^{-1}\frac{2x}{3} + C
\end{aligned}\\]

The skill is algebraic manipulation _before_ integrating: factor and scale
until the integrand matches a stock form. Recent marking feedback praises
students who "recognised algebraic manipulations to present the integrand as
a recognisable simple integration" — that recognition is the whole game.


## Technique 5: areas between curves and volumes of revolution {#technique-5-areas-between-curves-and-volumes-of-revolution}

Area between two curves is the integral of (upper − lower), splitting at
intersection points. Volumes are the Extension 1 upgrade:

\\[ V\_x = \pi \int\_a^b y^2\\,dx \qquad V\_y = \pi \int\_c^d x^2\\,dy \\]

Rotating about the \\(y\\)-axis means integrating with respect to \\(y\\):
rewrite every boundary as \\(x = g(y)\\), and when the region floats away from
the axis, subtract the hole:

\\[ V\_y = \pi \int\_c^d \left(x\_{\text{outer}}^2 - x\_{\text{inner}}^2\right) dy \\]

_The region bounded by \\(y = x^2\\) and \\(y = 2x\\) (meeting at \\(x = 0, 2\\), so
\\(y\\) runs from 0 to 4) rotated about the \\(y\\)-axis._ The parabola gives
\\(x = \sqrt{y}\\) and the line gives \\(x = \tfrac{y}{2}\\). Which is outer? Test
\\(y = 1\\): \\(\sqrt{1} = 1 > \tfrac{1}{2}\\), so the parabola is outer:

\\[\begin{aligned}
V\_y
&= \pi \int\_0^4 \left(y - \frac{y^2}{4}\right) dy \\\\
&= \rule[-0.5em]{0pt}{2.6em}\pi \left[\frac{y^2}{2} - \frac{y^3}{12}\right]\_0^4 \\\\
&= \rule[-0.5em]{0pt}{2.6em}\pi\left(8 - \frac{16}{3}\right) \\\\
&= \rule[-0.5em]{0pt}{2.6em}\frac{8\pi}{3}
\end{aligned}\\]

Marking criteria give separate marks for writing the volume as a difference
of two integrals, getting the limits right, and evaluating — a correct setup
with an arithmetic slip still banks most of the marks. Draw the region first;
five consecutive years of marking feedback say the students who sketched did
better.


## Exam-style set — with the marks breakdown {#exam-style-set-with-the-marks-breakdown}

<div class="problem-set">

1.  \\(\displaystyle\int 2x\\,e^{x^2}\\,dx\\) — 3 marks: substitution \\(u = x^2\\)
    (1), correct primitive (1), \\(+C\\) (1).
2.  \\(\displaystyle\int\_0^1 \frac{x}{1+x^2}\\,dx\\) — 4 marks: logarithmic form
    or substitution (2), limits (1), exact value (1).
3.  \\(\displaystyle\int\_0^{\pi} \cos^2\tfrac{x}{2}\\,dx\\) — 3 marks:
    linearisation (1), integration (1), value (1).

</div>

Answers:

1.  \\(e^{x^2} + C\\)
2.  \\(\left[\tfrac{1}{2}\ln(1+x^2)\right]\_0^1 = \tfrac{1}{2}\ln 2\\)
3.  \\(\left[\tfrac{x}{2} + \tfrac{\sin x}{2}\right]\_0^{\pi} = \tfrac{\pi}{2}\\)


## Practice set {#practice-set}

<div class="problem-set">

1.  \\(\displaystyle\int (4x^3 - 3x^2 + 2x - 5)\\,dx\\)
2.  \\(\displaystyle\int x^2(x^3 + 1)^6\\,dx\\)
3.  \\(\displaystyle\int\_0^1 \frac{dx}{1 + x^2}\\)
4.  \\(\displaystyle\int\_0^4 2x\\,e^{x^2}\\,dx\\)
5.  \\(\displaystyle\int\_0^{1/\sqrt{2}} \frac{dx}{\sqrt{1 - 2x^2}}\\)
6.  The region under \\(y = \sqrt{x}\\) from \\(x = 0\\) to \\(x = 4\\) is rotated
    about the \\(x\\)-axis. Find the volume.

</div>

Answers:

1.  \\(x^4 - x^3 + x^2 - 5x + C\\)
2.  \\(\tfrac{1}{21}(x^3+1)^7 + C\\)
3.  \\(\left[\tan^{-1} x\right]\_0^1 = \tfrac{\pi}{4}\\)
4.  \\(\left[e^{x^2}\right]\_0^4 = e^{16} - 1\\)
5.  \\(\left[\tfrac{1}{\sqrt{2}}\sin^{-1}(\sqrt{2}\\,x)\right]\_0^{1/\sqrt{2}} = \tfrac{\pi}{2\sqrt{2}}\\)
6.  \\(V = \pi \int\_0^4 x\\,dx = 8\pi\\)


## The traps, ranked by marks lost {#the-traps-ranked-by-marks-lost}

1.  _Forgetting \\(\frac{1}{a}\\)_ in the linear composition rule.
2.  _Keeping the old limits_ after substitution in a definite integral.
3.  _Dropping \\(+C\\)_ on an indefinite integral or differential equation — it
    is a whole marking criterion.
4.  _Outer and inner the wrong way round_ in a washer volume. Test one value;
    the bigger radius is outer.
5.  _Integrating \\(\sin^2 x\\) as if it were \\(\sin x\\)_. Linearise.
6.  _Not checking the domain_ on inverse-trig forms — \\(\sin^{-1}\\) only
    accepts \\([-1, 1]\\), and examiners set limits that respect it.


## What this is, and what it is not {#what-this-is-and-what-it-is-not}

These notes are written by a NSW maths teacher for students and parents, from
the syllabus and the published marking feedback. They are not a replacement
for your teacher, your school, or the [official NESA materials](https://www.nsw.gov.au/education-and-training/nesa/hsc), which are
NESA's own copyright and worth reading directly. And if a technique is not in
the syllabus — integration by parts, for instance, is not in Extension 1 —
it is not here, and it will not be on your exam.

Next in this series: differential equations (separation of variables,
Newton's law of cooling, and the \\(Q = P + Ae^{kt}\\) model), then proof by
mathematical induction.

New notes like this one land on Mastodon:
[@MattEducator@mathstodon.xyz](https://mathstodon.xyz/@MattEducator) —
follow along there.
