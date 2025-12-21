---
layout: post
title: Mac Pro 2012 Final Upgrade Project.
date: 2021-05-13 20:45:00 +0800
category: Personal
tags: [personal, upgrade]
---
Before this start: I know I already made a [video](https://www.youtube.com/watch?v=VQ36UD8g7e0&list=PLN-BUs9Cvmoq_uD8xAyjY0BTSv7a4yaSw) about it in Feb 28th, 2021. But I still want to do a write up with this so let's fucking go.

Some people know I owned a Mac Pro 2012, it's been my main computer for probably 3 years+ at this point. In between I already did CPU, GPU, Ram and Disk upgrades but it's not close to what I desired. Around the end of 2020 I finally got a chance to get all the necessery parts for the missing hardware pieces for my Mac Pro. And this is what pushed me forwarded to did this "Final" upgrade for myself.

**So...What's the parts and the plans?**

First, I'm going to swap the Wi-Fi/BT card from the stock one to Apple BCM94360CD, which will enable full Continuity feature, AirDrop 2.0 feature to it.

Second thing I'm going to add is HighPoint RocketU 1144D USB 3.0 PCI-E Card. Since the last Classic Cheese Grater Mac Pro still don't ship USB 3.0 until Trash Can Mac Pro. Gladfully because of Trash Can, this card's chipset is natively supported by macOS so it's OOB.

Lastly, on the software part I'm going to install OpenCore and do a clean Catalina Install. As I'm upgrading the hardware, it's also a good opportunity to upgrade the system too.

<img src="{{ site.baseurl }}/images/20210513_MacPro/Parts.jpeg" width="300"/>

**The Fun/Pain Part, Hardwares.**

The first thing I'm going to remove is the stock AirPort card, but...Things doesn't went smoothly as always. There's a screw that are so tighten that I couldn't get it removed. Took me 2 days to get them out, and as you can see I broke the standoff, also proven that it was really stuck.

<img src="{{ site.baseurl }}/images/20210513_MacPro/Stuck_Screw.jpg" width="300"/>
<img src="{{ site.baseurl }}/images/20210513_MacPro/Removed.jpg" width="300"/>
<img src="{{ site.baseurl }}/images/20210513_MacPro/OG_APC.jpg" width="300"/>

After removed APC, I decided to move on to USB 3.0 Card first, the process is much easier and painless, this is also part of the reason how I liked this Mac Pro's design, you barely need to use screw driver to upgrade some components.

<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0908.jpeg" width="300"/>

Back to install BCM94360CD in, took me some time to connect the neccessery antenna and BT cords and try to tighten the screws, thankfully that one screw is enough to hold it in its place, was a bit afraid that it won't hold in well.

<img src="{{ site.baseurl }}/images/20210513_MacPro/New_Card_Installed.jpeg" width="300"/>

Now the hardware part is finished, let's moved to the software part. :)

<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0895.jpeg" width="300"/>

Lastly, I got a 1TB Seagate HDD after came back from vacation (Same date as publish the video.) so that also adds into this upgrade project, No pictures taken tho. :P

**Software part.**

Now this is where the fun part start, I won't go too indepth as there's a more detailed guide available which I'll link below.

My Mac Pro have 3 Disk inserted, 1 being SSD and 2 HDD, 1 is Data and the other are used for Time Machine, I'll put the necessery files on my Data HDD, Wipe both SSD and TM HDD to do the OC Pre-Config, here's the steps:

Boot Mojave Installer -> Wipe SSD and HDD 2 -> Install Mojave on HDD 2 -> Config OC from HDD 2 to SSD & Create Catalina USB -> Install Catalina on SSD -> Setup

<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0932.jpeg" width="300"/>
<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0936.jpeg" width="300"/>
<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0941.jpeg" width="300"/>
<img src="{{ site.baseurl }}/images/20210513_MacPro/IMG_0944.jpeg" width="300"/>

**Useful links.**

Here you can find the detailed guide of installing OC on your Mac Pro, Also huge shoutout to Martin LO, he literally made an OC Package for our beloved Mac Pro, I'm using his OC Package and haven't have any single issue so far, You can also check out his YouTube, he made some useful videos about configuring OC packages for Mac Pro. :)

[cMP Upgrade Guide/Sticky Discussion](https://forums.macrumors.com/threads/cmp-classicmacpro-4-1-5-1-upgrade-guide-sticky-discussion.2099092/)

[OpenCore on the Mac Pro](https://forums.macrumors.com/threads/opencore-on-the-mac-pro.2207814/)

[Martin LO's YouTube](https://www.youtube.com/user/h9826790/videos)

[Martin LO's OC Package](https://forums.macrumors.com/threads/activate-amd-hardware-acceleration.2180095/page-53?post=28255048#post-28255048)

**My Mac Pro Configuations. (As of May 13th, 2021)**

Mac Pro 2012 Single CPU

CPU: Intel Xeon X5675 6C12T 3.06 GHz

GPU: EVGA GTX 680 2GB (Mac EFI Flashed)

Ram: Samsung 16GB DDR3-1333 ECC Reg

Disk: Kingston UV500 240GB SSD (OS) / WD Black 1TB 7.2K RPM HDD (Data) / Seagate SkyHawk 1TB 7.2K RPM HDD (500GB each for Video/VMs and Time Machine)

OS: macOS 10.15.7 Catalina

Additional compoments:

BCM94360CD Wi-Fi/BT Combo Card

HighPoint RocketU 1144D USB 3.0 PCI-E Card