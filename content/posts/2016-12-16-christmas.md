---
author: "Gameblabla"
date: 2016-12-16
linktitle: DellardOS 1.0 Rainbow Dash - Some history
title: DellardOS 1.0 Rainbow Dash - Some history
tags: ['dellardos', 'linux']
categories: ['Projects']
weight: 10
---
![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/dellardosscr1_0_o.png)
DellardOS, the systemd-free Devuan distribution, is being resurrected !
After so many months neglecting it, i have decided to revive it and fix some of its shortcomings.
Unfortunately, requirements for the Live CD version has raised again due to some unresolved issues.

A minimal text installer version is planned in the near future.

**After DellardOS became dormant…**

The Linux desktop landscape is rapidly changing :
- The growing monster known as systemd was adopted by many major distributions
- Mesa has already reached OpenGL 4.5 compliance
- Geforce Kepler GPUs are the last ones that do not require any firmware blobs

The custom distro trend seems to be dying off as well…
but it’s far from dead of course.

Due to numerous reasons, i stopped working on DellardOS and it became dormant.
However… This happened :
<a href="http://betanews.com/2016/10/07/systemd-vulnerability-linux-crash/">http://betanews.com/2016/10/07/systemd-vulnerability-linux-crash/</a>

A vulnerability in systemd allowed hackers to take down (DDOS) any linux distributions with systemd.
Actually, they eventually discovered it was a local-root exploit. * facepalm *

As you can see, the headline seems to be suggest the culprit was Linux, rather than systemd.
Yum, victim-bashing at its finest…

In comparison, the only huge sysvinit vulnerability i know of dates all the way back to 2003.

systemd has grown so much, eating everything on his way, that it has now become a single point of failure, in theory and now in practice.

A lot of people have raised flags at systemd, on how it tries to become many things and replace them,
but sadly, they don’t seem to have learned anything, quite the opposite in fact.

Now Linux users have to suffer from privacy issues, arbitrary wait times, an unnecessary complex design, increasing CPU/Memory usage…
all of that for “supposedly” better boot times.

I actually measured the boot times on Ubuntu comparing Upstart and Systemd :
**Turns out systemd was much slower, especially with that stupid arbitrary wait delay on encrypted disks.** (1m and 30s !!!)

This isn’t progress, it’s worse than a giant step backward : it’s atrocious.

**My experience with systemd-distros**

Before i eventually decided to pick up DellardOS again, i had to look at this systemd-situation again.
Ubuntu had switched to systemd in 16.04 so i installed that on my computer.
Systemd is the worst in Ubuntu, worst than it ever was on Debian…

I had nothing but problems and issues with it.
Installing an encryption partition without a swap partition will result in the installation being aborted

**WHY ALL OF THIS ?**

Then after installing it, i had to suffer from horrible boot times : rather than checking if the partitions was properly
unmounted, systemd instead waits for 1m and 30s.
This delay is absolutely arbitrary, it does absolute nothingness while this happen!

Needless to say the experience was less than good so i wanted to get rid of it as fast as possible.
I installed upstart-sysv and removed as much systemd as i could.
In the end, i had to build my own desktop because almost everything in the repo is now dependent on systemd…

Right after removing systemd and switching to upstart,
i rebooted my computer and i noticed the boot times were now much better.
So needless to say i don’t regret removing systemd, not even a bit.

Another bad experience with systemd was Manjaro and this was at a time
when the OpenRC version was not redistributed.

After installing it, i had absolutely no network and i tried it on all of my computers.
To install OpenRC, i needed to have an internet connection and download the equivalent of 200Mb of packages.

Needless to say, this was not going to go smoothly but then i discovered systemd’s ugly face in Manjaro :
Since systemd introduces a dependency on applications compiled against it,
Manjaro devs were forced to distribute two versions of the same software !
This results in a huge confusing mess and makes it even less “user-friendly”.

Manjaro is awful, i did not enjoy using it.

**And now, DellardOS…**

I had previously released some older versions of **DellardOS** that were already systemd-free at the time… but they had several issues.

For one, no online repo was available for it meaning that each i make an update, i had to release an ISO with my slow connection.
For two, the way it was built is broken and it did not rely on deb packages at all.

In version 0.4 for example, there was a bug that prevented **DellardOS** from being updated.
![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/dellardosscr2_0_o.png)
Well…
I’m happy to announce that this and more are now fixed in **DellardOS** version 1.0 “**Rainbow Dash**” !
This version has one aim : Be stable while being fairly minimalist.

You still get the fast and lightweight JWM window manager with its custom menu specific to DellardOS.
One major “feature” is that DellardOS finally has it’s own signed repository, which means that it can now
update DellardOS-specific features over the internet, something i was finally was to do.

The menu received also some improvements and you can now access any of your debian installed apps,
just like you would when you first install JWM from the debian repository.

Another “nice” thing introduced is Firejail support for web browsers.
If you start any of the 4 web browsers from the DellardOS menu,
it will launch them with firejail and i can confirm it does limit privileges for apps.

This should make it harder for hackers to break out of the sandbox.

It’s no QubesOS but it’s a nice thing have, especially for browsers.
![](https://github.com/gameblabla/gameblabla.github.io/raw/simp/blog/images/scr2_0_o.png)
Another thing added was KSM support, which should result in theory in decreased memory usage.
However, it might also be bad for security as KSM don’t go along well with ASLR. (Debian doesn’t have ASLR by default)

I will add the ability to disable it from the menu in the near future.

Not all is good though, i had to increase the minimal requirements to 160 MB of RAM because bootcd would not complete the installation if it does not have enough memory.

Unfortunately, the only workaround is to either install from another computer or increase the RAM available. (not always possible, granted)
I might do a text-only installer version so we can work around this.

**WARNING:

DELLARDOS WILL DELETE EVERYTHING ON YOUR COMPUTER.**
**IF YOU HAVE SEVERAL HARDDRIVES, DISCONNECT THE ONE YOU DON’T WANT DELLARDOS TO BE INSTALLED TO !**

You can download DellardOS here:
<a href="https://dellardos.gameblabla.nl/downloads.htm">https://dellardos.gameblabla.nl/downloads.htm</a>
