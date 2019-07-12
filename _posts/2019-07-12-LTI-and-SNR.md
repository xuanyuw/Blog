---
layout: post
title: "How do you know when something is too annoying?"
author: "Xuanyu Wu"
categories: neuro
tags: [neuro]
image: snr.png
---
<sub> Image source: https://me.me/i/please-increase-your-signal-to-noise-ratio-before-speaking-20729493 </sub>

(This is the extended content of COGS118C (UCSD SS2 2019) Lecture 4: Noise & ERP and Lecture 5:LTI System and Convolution)

In the [last post](https://xuanyuw.github.io/Blog/neuro/Noise-Is-No-Fun.html), I very briefly introduced the major noises in EEG recordings. The next question would be how bad the noise is in your recording. So now, we proudly present **Signal to Noise Ratio (SNR)**.

To perform SNR, we need to have a bold assumption: our brain signal is a **Linear Time Invariant (LTI) system**. Then what the heck is LTI system?

### Linear Time Invariant (LTI) System

The first part of LTI is Linear. Obviously, this system has something to do with linearity. Just to scare you, I'm going to pull up the math first.
![linearity](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/linearity.png)
The idea behind it is very simple: adding together the results that computed element-wise is the same as the result we get when we sum the elements together and then do the operation. In this case, the whole is NOT greater than the sum of the parts.

In general, some combination of adding signals and multiplying by constants (as shown in the equations above) will produce a linear operation. Back to our SNR assumption, the first thing we assume is Signal and Noise are linearly additive, meaning they are simply added on top of each other, like 1 + 1 = 2.

What is time-invariant then? Again, scary math first.
![timeinvariant](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/timeinvariant.png)
The idea is even easier than linearity: acting on the same signal later will produce the same result later. It's like cooking, for example. Let's say it normally takes you 10 min to make a bowl of ramen. If you start at 6pm, you'll get ramen at 6:10. But if you start at 9pm, you'll get ramen at 9:10. Ramen is the same ramen(as long as you don't change the recipe), it won't change itself just because you cooked it later. In SNR, we assume brain signals are time-invariant, that is late stimuli produce late responses.

And we also assume Noise is statistically independent, which means basically everything about the noise has nothing to do with brain signals. With those three assumptions, we can pretty much only focus on the important parts (real signal and noise) themselves.

Of course, LTI system is too ideal for real-world syst
ems, but it is a good start to let us tear down the complicated system and do some useful analysis.

### Signal to Noise Ratio (SNR)

To measure the signal and the noise, we first need to find a way to quantify them. Here, we calculate the average fluctuation. The idea here is very similar to variant and standard deviation: we measure how much it changes. In SNR, we use mean square (ms) to represent the **Power** of the signal and root mean square to represent the **Amplitude**:
<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/ms.png'  width="250" height="80">
<img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/rms.png'  width="280" height="110"></div>

After we have a standard measurement for signal and noise, we finally can get SNR. SNR is simply the ratio of the power of the signal to the power of background noise (the name is indeed very straight forward).
<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/snrpower.png'  width="200" height="90"></div>

However, sometimes signal and noise are expressed in decibels (dB), so we got an equation for that.

<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/snr.png'  width="300" height="140"></div>

(Find the complete transformation here: https://en.wikipedia.org/wiki/Signal-to-noise_ratio#Decibels)

Now that we have the equation, just plug in the value you have and you can go to town.
