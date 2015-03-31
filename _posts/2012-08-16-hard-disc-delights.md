---
layout: post
title: Hard Disc Delights
categories: misc
---

After many a battle with Seagate about my *Green* drives, I've won; I
had been trying to use four 2TB Barracuda Green Drives
([ST2000DL003](http://www.seagate.com/internal-hard-drives/desktop-hard-drives/barracuda-green/))
in a software RAID5 for my fileserver but instead I would get drives
popping out of the array at random, as far as I could tell they were
actually shutting off, this was not a software issue.
Seagate initially denied that they supported any kind of RAID, but
after I argued with them about how they couldn't *not* support RAID
given that it was just block writes, a hard drive shouldn't know the
difference (or at least not care). They caved to this point, but then
claimed that it must be my power supply; since my power supply was
quite old by this point, I didn't chase the point any further, because
I couldn't prove them wrong.

Fast-foward to two weeks ago: I got sick of the noise of my old case
and power supply and decided to splash on new ones, at this point I
tried RAIDing the drives again, to the same effect. I reopened my
ticket with Seagate, stating that it wasn't the power supply and that
as such the drives we're fit for purpose, they agreed to exchange them
for different models. Victory!

The new drives arrived today and it's looking like the problem is
solved; installation went smoothly and copying over data is in
progress, they're providing *much* better write speeds in RAID than
the Green drives did, by about a factor of two.

I thought this was worth posting, I've seen several people online
complaining about similar problems. I felt like posting this and
letting people know that other people have encountered the same and
you can sort it out with Seagate was worthwhile. Here's hoping this
helps someone else!
