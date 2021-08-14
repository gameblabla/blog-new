---
author: "Gameblabla"
date: 2019-03-12
linktitle: More work on SMS Plus GX
title: More work on SMS Plus GX
tags: ['smsplus', 'rs97', 'cfw', 'bittboy', 'hq2x', 'scalers']
categories: ['Projects']
weight: 10
---

Hello guys, Another quick update on SMS Plus GX !

Unfortunately, i realised that i broke save states along the way some time ago, and it took me some time to fix it. It took me a while to fix it but i managed to do it.

I also found a bug in Fantastic Dizzy that was not in Ekeeke’s fork i believe but i introduced it when i did not properly remove the overscan code.

This resulted in horrible glitches in Dizzy when running in PAL mode… This is now fixed of course but most people would not even notice it because most games are forced to run in NTSC mode.

But i also decided to take a look at trying to improve the graphics on the RS-97.

Of course, i could wait for TonyJiH to add support for Bilinear filtering via the IPU but i wanted to something better or at least more suitable for our testcase.

**Upscalers**

At first, i looked at Hq2x.

While it does improve the visuals, the memory consumption is greatly increased and the only implementations that does improve on that support only ARGB888.

There are other alternatives like xBRZ but again, same issue and it’s just as CPU intensive, the RS-97 will certainly struggle with it.

I was a bit saddened a bit at first and i was about to give up on that idea…

**Scale2x**

When the Macintosh II was released, it only supported high resolutions. This forced game companies at the time to either to revamp their games or simply double the pixels.

But Eric Johnston while working at LucasArt in 1992, came up with EPX to address the rough edges when doubling the pixels.

While not jawdropping, the results are decent and there’s no degradation or visual loss to the picture.

Years later, unaware of **EPX**, Andrea Mazzoleni implemented Scale2x for the AdvanceMame project. It produces a very similar graphical effect, though it is implemented differently.

After i found a simple implementation of it in pygame and giving it a try, i knew i found the right candidate.

Click on the pictures to see the details.

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/edusoft_scr1.png)

*Native (256x192) vs Scale2x (512x384)*

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/edusoft_scr2.png)

*Scale2x (512x384 -> 320x240) vs Bilinear (320x240)*

Unfortunately, it does not look as good when downscaled to 320x240, though i still think that it looks better than bilinear filtering.

But at least it doesn’t eat 60MB of RAM unlike Hq2x and the speed of it is still decent.

**Improvements**

We are wasting a lot of performance when we try to downscale it. By design, Scale2x cannot target other resolutions, it simply doubles the output.

Given that the RS-97’s screen resolution is 320x240, this forces us to downscale it.

It used to be a heck of a lot slower when we were using SDL_SoftStretch so i used MAME4ALL’s simple scaler code.

Too bad that it does not use 32-bits pointers, we could have cut bandwith by half.

On the RS-97, this could be improved but only if TonyJiH implements downscaling via the IPU. And he does not plan to work on that until next month…

Assuming it is implemented, we wouldn’t need to downscale back to 320x240, saving a lot of performance.

This would also mean that the bilinear code can go.

Oh, and i also managed to make SMS Plus GX work on the Ritmix RZX-50 and it works ok, if somewhat sluggish on games like Sonic 1 FM.

That’s to be expected though, given the low-end SoC.
