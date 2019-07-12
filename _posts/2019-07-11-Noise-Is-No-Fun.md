---
layout: post
title: "Noise Is No Fun"
author: "Xuanyu Wu"
categories: neuro
tags: [neuro]
image: noise.jpg
---
<sub>Image source: https://imgflip.com/i/2sn1rp</sub>

(This is the extended content of COGS118C (UCSD SS2 2019) Lecture 4: Noise & ERP)

Noise is everywhere. Of course there are also A LOT of noise in our brain recordings. In this article, I'll talk about the major noises in brain recordings.

### "The noise in your brain"

#### Sensor and Environmental Noise

Any noise in the environment you are in can bring noises to the recording, including AC and trains that are passing by(yes, trains.). Another noise that belongs to this category is the aliasing noise in digitization we've mentioned before.

#### Non-Neural Physiological Noise

It turns out that a lot of intuitive movements can show up in the EEG data, and those are the noise we don't want. When I was participating the EEG study, they showed me how eye movements can affect EEG signals: when I blink or even look around the EEG signal would be drastically different. Of course moving in general can introduce similar noises. So I was told not to move and keep staring at the middle of the screen to reduce these noises (very tiring, I must say). But we can get rid of these noises using other measurement techniques, like using EOG(electrooculography) to reduce Blink effects and using accelerometer to reduce movement effects.

So far we assumed that these noises are independent from the brain's response. However, it's not always the case. When you want to, say, detect the response of a subject after he/she sees a picture. Then you cannot subtract the eye movements effects out of your signal.

#### Neuronal Stochasticity and Variability

The neuronal firing also have an inherent Stochasiticity (in human language: randomness) or Chaos property. Why is that? We don't know yet.
