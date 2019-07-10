---
layout: post
title: "From Brain To Computer: Signal Digitization And Sampling"
author: "Xuanyu Wu"
categories: neuro
tags: [neuro]
image: Digital-Transformation.png
---

<sub>Image Source: http://luman.io/meaning-not-mechanics%E2%80%8A-%E2%80%8Aa-human-approach-organizational-transformation/digital-transformation/</sub>

(This is the extended content of COGS118C (UCSD SS2 2019) Lecture 3: Time Series, Digitalization & Sampling)

You must have seen some kind of biomedical signals somewhere: whether it is electrocardiogram (ECG) in some sort of medical TV series or the signal wave in EEG data. These wiggle lines all have the same property: they are **analog**, meaning they are **continuous in both time and amplitude**. But we know the computer stores data points, which are discrete, meaning they are separate from each other. There are countless points in a continuous line, so clearly if you store all the data into your computer, it would explode.

Then how do neuroscientists do all sort of analysis with their computers safe and sharp? It is not because they have insanely good computers, but because the continuous signals are digitized. And this process is called **ADC(Analog to Digital Conversion)**. Remember the definition of analog? In order to make the data fit the computers, we need to convert both the signal itself through digitization and time series through sampling.

![digitization](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/digitization.png)
<sub> Image Source: Wim van Drongelen - Signal Processing for Neuroscientists_ An Introduction to the Analysis of Physiological Signals-Academic Press (2006) Fig 1.6 </sub>

I really like this figure, because it makes the concept of ADC so much easier.

Now let's look at the horizontal lines of the grid. You must notice there are something called "A/D Levels" at the very right of this figure. What does that mean? Essentially, A/D levels determine how big of the amplitude your A/D converter can take. As the figure shows, you draw a line at each level, then you capture the signal points that fall onto the lines.
![A/Dlevels](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/adlevels.png)
The number of levels you have depends on the number of bits you use in you ADC. The equation looks like this: **# of levels for n-bits ADC = $2^n$**. For example, if you have 8 bits in your ADC, then you'll have $2^8 = 256$ levels. But you cannot just randomly decide how many bits you are gonna use in your ADC, because poor choices leads to poor results. For example:
![Saturation](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/saturation.png)
This overflow of signal is very bad, because we cannot capture this, the digitized signal would be distorted (basically becomes a flat line at the top). Trust me nobody wants this in their data. This situation (of course it has an official name) is called **Saturation**.

The other thing to consider is how big is the space between each horizontal line. In other words, what is the smallest change you can capture? And this is called the **Resolution** of your ADC. Here's the equation: **resolution = $\frac{range}{number of levels} = \frac{range}{2^number of bits}$**. And the range is just simply the range of your signal's amplitude (in EEG's case, voltage).

Let's look at the vertical lines of the grid.
![time](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/time.png)
Each line represent a sampling timestamp. I love the analogy of ADC clock from my instructor:
> Imagine a metronome clicking. Every time it clicks, you jot dow the value of the signal.

That is the basic idea of sampling in ADC. The other things are pretty clear by now: the **Sampling Frequency/Rate (fs)** is just the number of sample you take per second, measured in Hz ($ = /frac{1}{sec}$) And the **Sample interval** in the figure above is just the time you wait before next sample (**$ = \frac{1}{fs}$**)

The following question would be: how fast should I sample? Well, it actually varies from dataset to dataset. But there's a theorem called **Nyquist Sampling Theorem** that set the baseline for sampling:
> To accurately represent a periodic signal/process with an intrinsic frequency B, you have to sample at least twice as fast.
Note here, the sampling rate can only be strictly greater than 2B, because if you sample at the rate: fs = B, you'll probably get the same thing over and over again:
![1bsample](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/1bsample.png)
Likewise, if you sample at fs = 2B, you may be very lucky and miss all the signals (the straight line in the middle).
![2bsample](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/2bsample.png)

Okay, that's basically the basics of ADC and the things I've learned so far.
