---
layout: post
title: "Convolution of Impulse Response in Digital Signal Processing"
author: "Xuanyu Wu"
categories: neuro
tags: [neuro]
image: convolution.jpg
---
<sub> Image Source: http://www.quickmeme.com/meme/3qi5mv </sub>

Suppose that eating broccoli makes you sad, and eating fried chicken makes you happy, then what would you feel if you eat fried chicken and broccoli in one bite? I know it's a weird setting in real life, but this is exactly something we are trying to do in Digital Signal Processing.

One of the method to infer what the system would do is called **Impulse Response**. Since we assume our brian signal system is a [Linear Time-Invariant System](https://xuanyuw.github.io/Blog/neuro/LTI-and-SNR.html), we can actually scaled and conduct time-delayed operations on it without changing the property of our result. Therefore, since we can easily get the result of each element, why not use them to get the overall result? This method is called **Divide (decomposition) and conquer (transformation)**
