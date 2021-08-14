---
author: "Gameblabla"
date: 2019-02-27
linktitle: More RS-97, bittboy news
title: More RS-97, bittboy news
tags: ['blog', 'rs97', 'cfw', 'wiki', 'bittboy']
categories: ['Maintenance', 'Projects']
weight: 10
---

Hello guys, hope you had a great year. (A bit late for that but meh)
If you follow my stuff on Dingoonity & youtube, you may have noticed that i got a bittboy and ported some stuff to it.

Unfortunately, there’s still some more work to be done before it can be considered complete.

Steward-fu ported some more stuff (including PCSX-Rearmed) but of course he has little regard for speed or quality control… he even admitted to me he does not play the games for long.

So yeah, i ported Quake & Quake 2 to it, including the RS-97 and gave it to some people. The Quake 2 port in particular took me almost a year before I got it to boot due to some portability issues with the code as well as compiler bugs that only got resolved in later buildroot releases. Really.

I also made a basic port of SRB2-Kart to the bittboy and it runs very sluggishly as you may have expected… I even applied some more additional optimizations to it and it still chuggles. Bittboy v2 with fixed SD card bus actually runs that game better due to faster swap but it’s still a terrible experience so i can’t recommend it.


Btw, if you wonder and ask me : “Where’s the RS-97 port of SRB2-Kart ?”

The RS-97 would be much more suitable for this game but unfortunately,
the old kernel (2.6.31.3) prevents it from even booting and even a later kernel release (2.6.35) will fail on that game.

Basically, what happens is that it loads all the stuff (wads and all) then it becomes stuck on the first bootlogo screen…

Looking at the log, I am completely dumbfounded as to why it fails to go past that.

Perhaps it requires some more additional portability fixes ? 
But the same binary works on QEMU-MIPS with a slightly more recent kernel so yeah…

Another 3D game I tried to port was Crafti, the minecraft clone that i’ve ported successfully to the Dreamcast. (also works on the TI-Nspire)

This game is very troublesome, like most of the time the 3D stuff refuses to work properly at all and would flat-out crash.
This happens on both the bittboy and RS-97.

I managed to port that game to the GCW0 without any issues and here I found out that it fails to even run at all.

This ran on an Nspire with 64Mb of RAM so I doubt that memory limitations is an issue at all so what gives ? Another game I should try with an older GCC release perhaps.

So what’s the next step ?
Quake 3 of course !

Unfortunately, this game requires OpenGL 1.1 acceleration, no software renderer is available.
There was a MorphOS release some years ago that used TinyGL so the code is very old and very MorphOS specific so that’s a huge bummer and pain in the arse.

This is one of the those games that would flat-out crash TinyGL so cheating won’t work here either.

Quake 2 basically required me to get rid of the shared-library stuff (far from a trivial task ! No one had done so for this game) and Quake 3 ups it to eleven.

Why not just use Xorg along with Mesa3D then ?
Well, if only I could get that to work ! Xorg simply refuses to show up no matter what I do.

This is balls but I hope I can solve this. I suspect this will require support for the linux console but it seems like I could wrong…

Anyway, If i can get Xorg to even show up xeyes, then I would be a happy man because it theorically means that i can use it together with Mesa3D’s software renderer.

Even if this works properly, this should only be used as a last resort : Xorg is very memory-intensive and the same stuff that can run without it (SDL, Allegro4…) will run slower under Xorg. Also, it’s the only way of running SDL2 stuff since it doesn’t support fbcon and it requires KMS/DRM for 3D acceleration in the console. We can only support fbcon for now so yeah, balls again…

So why no one gave this a try before ?
Well the odd screen resolution (320x480) and immature toolchain meant that it would have been all but useless,  not to mention out of consideration. 

RetroFW thankfully fixes this with its IPU implementation, among with other features.

What it doesn’t fix however, is the the ugly old-ass linux kernel.

Sadly, pcerceui has no plans for merging JZ4760 support upstream, them niggas are too focused on the RetroMini, which uses a JZ4725B. 
But Senor Quack told me that’s among their plans. (but none of them own an RS-97, how ironic)


Oh, and I opened a wiki about those devices here :

**Update from 2021 : Now dead, i need move it to github...**


If you expect more RS-97 news : Senquack is planning to release part5 of the MIPS recompiler.
This update is important as it will be the first one with a fully assembly coded GTE implementation that uses the MXU : this will make it one of the first software to make use of the MXU instruction set.

As for me, well I managed to greatly improve the HLE bios implementation.
While doing so, i’ve fixed games like :
- Digimon World (now booting up)
- Umihara Kawase Shun (now boots)
- Parasite Eve II (saving works)
- Medievil II (now goes ingame)
- LEGO Racers (saving works, mips rec will get an upcoming fix)

Senquack also told me that he made some HLE improvements on his side related to clocks.

I can’t wait to see those, because some games like Nuclear Strike requires perfect timing with memory cards for saves/loading for example…

It’s fair to see that these additions will make it the best PCSX release so far on this earth and I can’t wait to see it all running on the RS-97.

Btw, Senquack also fixed the Tetris with Cardraptor boot issue after I pestered him about it and the fact that Mednafen already had a fix. (complete with changelog and comment describing the issue)

PCSX-Rearmed accepted my PR to fix the game and my build of PCSX4ALL already contains the fix.
Due to a bug in the recompiler however, it will only work properly over interpreter.

This should get a proper fix in part5.

That’s all for now, thank you. I am really getting tired of this shit I admit.
