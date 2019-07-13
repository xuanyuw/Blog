---
layout: post
title: "Noise Detox: Event-related experimental design and analysis"
author: "Xuanyu Wu"
categories: neuro
tags: [neuro]
image: eventRelate.png
---
<sub> Image Source: http://sitn.hms.harvard.edu/flash/2018/dopamine-smartphones-battle-time/</sub>

(This is the content of COGS118C (UCSD SS2 2019) Lecture 4: Noise & ERP)

In the [previous post](https://xuanyuw.github.io/Blog/neuro/LTI-and-SNR.html), I briefly talked about how do we know how bad the noise in our signal is, but this is definitely not enough: We need to know how to overcome noise, because that's our goal after all.

For most of the studies, we are interested in how brain react to a certain stimulus. It's almost a common sense that our brain signal changes when it's somehow triggered. And this change in brain signal (specifically potential) is called **Event-Related Potential(ERP)**.

<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190713_event_related_img/erp.png'></div>

I assume we all agree that doing just one trial of experiment is ridiculous in vast majority of the cases. Multiple-trial experiment gives us an opportunity to reduce noise.

<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190713_event_related_img/averaging.png'></div>

Very cool huh? But in order to do this, we need to make some assumptions just like what we did in [calculating SNR](https://xuanyuw.github.io/Blog/neuro/LTI-and-SNR.html):

1. Noise must be **Independent** of any experimental variable.
2. Noise must be **Stationary**, meaning its statistical measures (like mean, standard deviation, etc.) do not change over time. 
3. It must be **Ergoic**, which means averaging over trials is the same as averaging over time.

With that being said, averaging over N trials maintains signal rms while decreasing noise root mean square(rms), because noise is independent.

<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190713_event_related_img/signalandnoise.png', width = '320'></div>

Recall the equation of SNR:

<div style="text-align:center"><img src='https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190712_snr_img/snr.png'  width="300" height="140"></div>

When rms of noise is reduced, we get higher SNR, thus clearer signal.

With this logic in hand, we can derive the basic procedure for **Event-Related Analysis**:

1. Record raw data
2. Find event triggers
3. Cut a window of data around (before and after) the trigger (**epoching**)
4. Subtract pre-stimulus baseline to remove slow drifts
5. Average over groups/conditions to reduce noise.

That's it for noise (for now). But the battle with noise will never end.
