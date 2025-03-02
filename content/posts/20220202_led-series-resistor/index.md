+++
title = "How to calculate the series resistor for an LED"
featuredImage = "20220202-Screen-Shot-2022-02-02-at-6.32.54-pm.png"
date = 2022-02-02
lastmod = 2025-03-02T17:25:15+11:00
tags = ["HamRadio"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

When we connect an LED to to a battery, we often need to connect a resistor is series to limit the current. I saw this question come up in a facebook group, and thought it might be useful to use this as a simple illustrative example of doing circuit design.

The basic circuit we will be considering is shown below:

{{< figure src="20220202-quicklatex.com-06f3bedeaf7e311d9af11493d4616842_l3.svg" >}}

We have a current \\(i\\) leaving the battery/voltage source, passing through the resistor giving a voltage drop of \\(V\_R\\), then passing through the LED with a voltage drop of \\(V\_{f\_{LED}}\\) before returning to the battery. So, how can we work out the value of the resistor we should use?

To answer that question, we need to know some things about the LED. Specifically:

how much current do we want to be passing through the LED?
what will be the voltage across the LED at that current?

How can we find the answer to those questions? We need to refer to the datasheet of the LED component!

First let’s check the maximum rating for the component. Looking at the datasheet, we see the following table:

{{< figure src="20220202-Screen-Shot-2022-02-02-at-5.23.35-pm-1024x420.png" >}}

We can see that the maximum DC current for a red LED is 30mA. We generally don’t want to run a component right on its maximum limit, so what happens if we reduce the current a bit? Digging further on the datasheet, we find the following graphs:

{{< figure src="20220202-Screen-Shot-2022-02-02-at-5.24.13-pm-1024x469.png" >}}

The graph in figure 3 shows us how bright the LED will be at various currents. At 10mA, the intensity of the LED light is said to be at a normalised level of 1. If we increase the current to 20mA, then the LED will be around 1.5 times as bright as the 10mA level. Increasing the current to 30mA (the max) then the intensity is only around 1.8 times as bright at the intensity at 10mA, so it is consuming a lot more power for little gain. At 20mA, we still get a bright intensity while leaving a fair amount of headroom below the maximum, so let’s design our circuit to have a current:

What will be the voltage across the LED at that current? Referring to the graph in figure 2, we see that the voltage across the LED will be approximately:

\\[V\_{f\_{LED}}=2\text{ V}\\]

What will be the voltage across the resistor of R ohms? We know the current through the resistor must be 20mA, so we can use Ohm’s law to work out the voltage drop across the resistor:

\\[\begin{align\*}
        V\_R & = i \times R \\\\
        & = 20 \times 10^{-3} \times R
        \end{align\*}\\]

We can now use Kirchoff’s voltage law, which says the sum of voltages in a loop is always equal to 0V. If we go clockwise around the circuit, the voltage source helps the current and is therefore positive, whereas the resistor and LED oppose the current, so their signs are negative. The equation becomes:

\\[\begin{align\*}
        V - V\_R - V\_{f\_{LED}} & = 0 \\\\
        V - i \times R - V\_{f\_{LED}} & = 0 \\\\
        i \times R &= V - V\_{f\_{LED}} \\\\
        R & = \frac{V - V\_{f\_{LED}}}{i}
        \end{align\*}\\]

If we assume we are using a 9V battery, then substituting in the numbers gives us:

\\[\begin{split}R & = \frac{(9 - 2)}{20 \times 10^{-3}} \\\ & = 350~\Omega\end{split}\\]

For safety, we can round it up to the nearest preferred value, which would be a 390&Omega; resistor with colour code orange-white-brown. Double-checking the current, we get:

\\[\begin{split} i & = \frac{V - V\_{f\_{LED}}}{R} \\\\
    & = \frac{(9 - 2)}{390} \\\\
    & = 18\text{ mA}\end{split}\\]

This is still acceptable. So, we have now completed our circuit design:

{{< figure src="20220202-quicklatex.com-fecf716ddc8012b94614dba19222cab7_l3.svg" >}}
