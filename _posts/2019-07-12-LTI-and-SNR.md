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

In the [last post](https://xuanyuw.github.io/Blog/neuro/Noise-Is-No-Fun.html), I very briefly introduced the major noises in EEG recordings. The next question would be how bad the noise is in your recording. So now, we proudly present Signal to **Noise Ratio (SNR)**.

To perform SNR, we need to have a bold assumption: our brain signal is a **Linear Time Invariant (LTI) system**. Then what the heck is LTI system?

### Linear Time Invariant (LTI) System

The first part of LTI is Linear. Obviously, this system has something to do with linearity. 
![linearity](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190710_digitization_img/saturation.png)
