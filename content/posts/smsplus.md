---
author: "Gameblabla"
date: 2019-03-08
linktitle: My fork of SMS Plus GX (itself based on SMS Plus)
title: My fork of SMS Plus GX (itself based on SMS Plus)
tags: ['smsplus', 'rs97', 'cfw', 'bittboy']
categories: ['Projects']
weight: 10
---

Hello guys, i thought i would write this post to reflect me working my fork of SMS Plus and the progress and stuff i made on it.

So how it started ?

Well at first, SMS Plus was nothing more than a port of SMS_SDL to the RS-97.
The RS-97 is best described as the closest thing to a sucessor to the Dingoo A320. 

It had its fair share of issues at first but most of these got fixed with the advent of RetroFW by TonyJiH.

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/shadow_dancer.png)

SMS_SDL is based on SMS Plus, an emulator by Charles Mcdonald.

However the version it is based on was very old, the Dingux port itself had severe issues with sync, and it did not even support games like F16 Fighting Falcon & Rozetta no Shouzou.

This is because these games required emulation of the TMS9918, which is also incidentally used by SG-1000 games.

Support for these was added in SMS Plus 1.3 and the only actual port to have updated to that base (and beyond) was the PSP & PS Vita ports.

That was actually pissing me off because i couldn’t play these games and i was sure some people would complain to me about that when playing games on their RS-97’s.

At first, my plan was to upgrade to that newer base and be done with it.
However along the way, i discovered that Charles Mcdonald also released later versions of SMS Plus : 
- 1.5 (came with GPLv2 license but source code is lost sadly)
- 1.8, which unfortunately became proprietary.

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/falcon.png)

SMS Plus 1.5 added support for Korean mappers and that alone made me want to upgrade to the newer codebase.
However, i could not find a copy of it anywhere !

Version 1.8 was licensed under a proprietary license forbbiding non-commercial license, an unforunate turn of events.

Speaking of non-commercial clause issues, i discovered later that :

- The YM core was also forbidding non-commercial, so was the Z80 interpreter.
- The PSG’s emulator license has a problematic license.

Naturally what i did was to contact Charles Mcdonald about it and whenever he could change his mind about it.

I flat-out told him my goal was to make the emulator fully free software.
He did give me an answer and he told me that he thought SMS Plus 1.3 was the last version under the GPLv2.

He also seemed to be unwilling to relicense it, given that he got legal threats from companies using his emulator,
before stating that making SMS Plus was a mistake on his part and he never wants to hear of it again.

But despite this, he did seem to agree with my move for switching it to a fully free software emulator.

A short while after, i noticed that he deleted his SMS Plus repository from his github account.

*Make that of what you will…*  (But someone else did make a fork of it)

In the meantime, i also contacted Maxim’s about his PSG code. He said that he later licensed said code under a different license but he did use the (problematic) license at first.
Charles Mcdonald also told me his changes fall under his license.

So at first i was kind of stuck, i could only go as far as SMS Plus 1.3 or i could embrace SMS Plus 1.8 with its support for Korean mappers but compromise on the license.

But all of that didn’t matter anyway because i soon discovered another fork of SMS Plus…

**SMS Plus GX**
![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/smsplusgx_icon.png)

SMS Plus GX was an emulator by ekeeke for the Nintendo Wii/Gamecube.

At first, i thought it was going to be yet another outdated port of SMS Plus to the platform. Why ? I saw dozens of SMS Plus ports to other platforms and they all had their own shares of issues, not to mention all of them being outdated.

But i was very surprised to see that, not only did ekeeke have support for Korean mappers (apparently he got the code from Charles himself), he also fixed other issues in games like Ys I Japanese version relying on a VDP bug only present in SMS 1 consoles.

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/safari_hunt.png)

He also added other additions, like proper support for SG-1000 games and Colecovision support. All of that was still licensed under the GPLv2+.

This was a surprise because he would later be unfamously known for switching Genesis Plus GX 1.6 from the GPLv2+ to a non-commercial license.

I say “he” but according to an email exchange, this was done upon the request of Charles himself. And the emulator stayed under such a clause ever since…

However, such a move never happened with SMS Plus GX because ekeeke had discontinued the emulator before that date and so it stayed under the GPLv2+.

So the legal situation meant that i no longer have to worry about that.

At first, i was kind of excited. I was less so when my first build of it crashed horribly. Even when compiled as a 32-bits binary, it was still prone to crashes.

Ultimately, i decided to rebase my emulator around it and scrap my plans for using SMS Plus 1.3. This did require some fixing to the source code but i finally managed to have it working.

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/lemmings.png)

So what about the Z80 interpreter ? Later i dug more into it and found that the same Z80 emulator was later relicensed in MAME under the BSD-3 license. Naturally, i dropped him an email and asked him if he could also relicense the old versions under that license as well.

He told me that he won’t do that even though the license allows him to revoke the terms at anytime because he said that older projects were using those old terms and it would create a bit of chaos. He then suggested to me i should really use his MAME code (given it fixes things) and switch it to C if that was my needs.

I did try to switch it to C but it was going to take more time than i anticipated so i decided to look for another solution and i discovered that PocketSprite had also an SMS Plus port and already switched the Z80’s interpreter to another one based on EightyOne’s core. This was great, i only had to take their commit and make it compatible with SMS Plus GX.

As it turned out however, this took a bit more time than i anticipated but i finally did make it work. However, i would later discover its issues but more on that later…

**After the rebase to SMS Plus GX**

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/badapple.png)

So this was great : I had a new Z80 interpreter, the emulator supported more games (though most of them are a bit obscure) and i also somewhat fixed the sync issue by using the blocking I/O feature in Portaudio.

But at first when trying out some homebrew like Bad Apple, it would flat out crash the emulator. After more digging into it, it turns out the homebrew was so huge it would overflow the 8-bits variable reserved for the number of pages. It was 256 pages big and since an unsigned 8-bits variable could only go as far as 255, it set itself back to 0, hence the crash. This was trivial to fix and increasing it to an unsigned short fixed it, kinda : it also required PAL timings to run.

So i added it to the database to known games requiring PAL timings. However, the auto-detection by nature won’t work if you rebuild it from source or is modfied in some ways so be warned.

The fix is here : **[Git commit 80aefb94565e1cd463991cfae8331b2e8205ab37](https://github.com/gameblabla/sms_sdl/commit/80aefb94565e1cd463991cfae8331b2e8205ab37)**

This was nice and all but i later found out that the EightyOne’s Z80 core was vastly inaccurate. I only disovered this after i ran the SMS VDP test ROM : it would negatively affect the horizontal & vertical sync.

So for a while, i was back to using the old Z80’s proprietary interpreter again. Great…

But while i was trying to find a solution for this, i fixed the other issues in the frontend code as well as the sync code.

Then i left it to rot for a while.

**Pausing the development and resuming it months after…**

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/sonicedusoft.png)

Fast forward to February, RetroFW was released for the RS-97. Compared to my older firmware Useless and 97Next, it finally fixed the flaws of either and greatly simplified the porting process without requiring odd double lining. This was also the first release to have some compatibility with OpenDingux binaries.

I also got a Bittboy and i was asked to work on a port for it. Naturally i agreed to resume work on it and to look at whenever i could fix the remaining issues.

I added support for more audio apis like ALSA, pulse, OSS (used on the RS-97), SDL, in addition to Portaudio & libao. I can thank Reicast for those given that the audio specific code was actually fairly trivial. The SDL code was based on Dmitry’s.

I also finally managed to switch it to MAME’s z80 interpreter, with the several improvements it comes. I had to bring the SP fix from ekeeke version though but it’s a single liner so not a biggie. (Said trivial fix was also reported by other people elsewhere, making it a fact)

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/edusoft_2.png)

There was another issue i wanted to fix and that was Sonic’s Edusoft not booting without an official BIOS. Because requiring a copy of Sega’s BIOS was hugely inconveninant, i decided to look for fix.

It didn’t take me long to find a potential fix for it, as Bock described it in lengh : “The game breaks because it expects the vertical interruption bit to be set by default on the VDP, and wait for an interruption before writing any registers to the VDP.”

He then shows a screenshot of MEKA having VDP Register 1 set to 0x80 so i made a quick search in the VDP code and i quickly found the related code.

If the game requires that VDP Register to be set to 0xA0 (which is set by the BIOS apparently) then why was it set to 0x80 in most emulators ? Look at the available VDP documentation left me empty handed : none of them mention what default values the BIOS sets the VDP registers to. This is probably because it falls outside of what they actually cover and i couldn’t find any evidence of Charles found that it should be that value.

So naturally i tried to catch what the BIOS actually does to the VDP registers and guess what ? All of them return 0xE0 upon booting up a game. However, some of the BIOSes, including the Hang out one, do set 0x80 at some point but then it is quickly changed to 0xA0 then finally to 0xE0.

So my theory is that 0x80 is actually a mistake on Charles Mcdonald’s part and he may have got that value by assuming this would be the right value to go with. But as Sonic’s Edusoft shows, this is not the case and all the games i tried so far seem to work fine with 0xE0. (And really, why wouldn’t they ?)

[I opened a bug report on MEKA’s github repository](https://github.com/ocornut/meka/issues/43) to inquire more about this bit as there might be a reason behind it but for now, i have decided to set 0xE0 as the default VDP value for all games.

**Colecovision stuff**

![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/kevtris.png)

So SMS Plus GX can also emulate the Colecovision : support for it was added fairly recently until it was discontinued (2010).

Support for that console was never added back to Genesis Plus GX.

Now of course, this is far from being the most accurate Colecovision emulator, as it is more of a nice addition. 

It doesn’t support the Super Expansion Module (the only open source emulator to support it is BlueMSX) not to mention MegaCARTs, which again BlueMSX is also the only open source emulator to support it.

My version did not properly support it at first due to a mistake on my part, this was quickly fixed once i found out about it. A really dumb mistake that allowed Colecovision games to work.

The downside is that it requires the official Colecovision BIOS. But…. 

It turns out pretty much all the Colecovision emulators require it. In fact, no emulators so far attempted to reimplement, even ColEM.

All of them provide a copy of the Colecovision and the legal status is not clear for those. The Colecovision BIOS is not just a bootrom like the SMS’s one and actually provides routines for programmers.

These were especially useful given that you could save some precious cart space at the time. BlueMSX is provided with such a BIOS, so is ColEm it seems.

It is not trivial to replace and no one has tried to document it extensively like nocash did for the PSX for example.

So if you are on the RS-97, just put BIOS.col in .smsplus/bios and you should be able to play your Colecovision games Just make sure your Colecovision games have the .col extension, otherwise it will fail to detect them.

You can get the IPK package for the RS-97 with RetroFW here :

https://gitlab.com/gameblabla/gameblabla-releases/-/tree/master/opk/retrofw
