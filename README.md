# g5-restore

Checking for serials. 

You can find them under the main cover. I had bought 3 from FB marketplace (for 150$): 

Cl62901ev5w dual 2.3 512 250 6600 
G853758gru2 2.0 51 160 9600

So based on this we can already tell which one is useless and which one is good :)

Luckily from the 3 I bought 2 of them were the later 2.3 Ghz releases which use regular PCIe (versus AGP connectors) which means we can upgrade most pieces.

## Checking power cables & water damage
--- 

![1e4da476-6ee0-4aed-aafc-71de666b32878145070884849698766](https://github.com/user-attachments/assets/b7df1aa5-77a9-419b-b09e-df51c4f6f867)

So this is a C19-C20 cable for the later 2005 g5's. The cases are super heavy but also cool looking! I saw some nice rebuild projects I think I might give a go:)

These can draw 30-50% more amps, while earlier versions use the common C13-C14 cables you find on many devices still to this day. 

The power supply on these is seated at the bottom and takes the full width of the base of the computer, it's a pretty crazy set-up but they can be found second hand or refurbished.It is also quite ez to retrofubrish a more modern PSU into the case. 

Better to check the cables for damage (often  present when they were plugged-in/out many times) and get new cables. I decided to get new ones cause mine looked like hell. 

## RAM
---

The original ram sticks are horrible for modern comparison, upgrade safely with backwards compatibility to:

240 Pin Dimm - 1.8v - PC2-4200 (533Mhz) - ECC Registered x4 2GiB

Bringing our ram from 512 mb (2x 256mb) to 8Gib. Which is considerable upgrade!

## Memory
---
We can use regular SATA SSDs which are backward compatible even tho we will not get the full speed, you can get them for cheap reconditionned (20$ for 512 GiB)

## GPU
---
You can also go wild and upgrade GPU using regular PCIe connectors, you can also get modifications boards with the remaining PCIe slots!

## SPECIAL THANKS
---
https://www.youtube.com/watch?v=cS58kQ10qas

Check Casey Cullen's channel out on YouTube!!!!!

## Booting
---

Hold command and option at the same time + O and F (OpenFirmware) 
You should now see a console like white terminal. 

If you see this screen:

![images](https://github.com/user-attachments/assets/6600058e-5dad-42fc-b8fb-4acb03d2706a)

Don't worry (for me it was simply because I unplugged the old drive) and the computer detects no OS.

Restart while holding the right keys!

In later systems this is easier I believe, but back then yes it's 4 keys.

**If you're struggling to see this screen here are two tips:**
- If you're using a regular keyboard its ALT and Windows key (and O + F).
- If you're seeing the regular old OS directly i recommend removing the drive altogether.

The video above explains nicely how to use:
```
dev / ls 
or
devalias 
```

You need to look for devices listed under UD, USB, DISK

You can use space or arrow down to scroll. 

![img_1334](https://github.com/user-attachments/assets/b65c0c15-e215-4bed-ad5e-bcc172a70b16)

Which helps to find all your devices (didn't want to burn a CD, it's not 2005 unfortunalty)

You can then boot from USB (I used the 3rd port down on back panel) 
``` boot usb3/disk@2:2;\\yaboot ``` 

_Note: Disk 2: partiton 2 is default_

Getting started
---
(Skip this if you want to know how to get it acutally to work LOL) 

Make sure to use a power pc compatible install: I used: https://cdimage.debian.org/cdimage/ports/12.0/powerpc/

Also make sure to plug the ethernet cable into the top port (port 0) as this will be used to make the full Debian install. 

I ignored most of the warnings for mirrors and minimal install, as I kind of knew it would be hell since I hadn't received the new RAM yet. 
Took 60 seconds for the first Init script to start...

And the mac pro g5 was screaming through half of the install. 

It even worked from a USB 3.2 which are I guess backwards compatible. 

Attempt #2
---

So I thought let's try something else...

I used the ISO from the video: Rufus to flash so it autodetects settings. 

https://fienixppc.blogspot.com/p/blog-page.html

Which is specifically designed to have a minimal Mate Desktop which would help a lot and many more base packages. 
Also is made for powerpc editions, so I hope it is better than any other port that could work.

Repeated the boot process and voilà. 

![20250121_171323](https://github.com/user-attachments/assets/ec4fa379-17c8-4a79-b715-22eca546e237)

Pressed enter and 30 minutes later we are in!
Also comes with Synaptic which is a neat package manager. 

![20250121_174349](https://github.com/user-attachments/assets/4ca7f46a-3a73-4152-b555-b15ac48908bf)


I also checked out the ressource monitor to find out:
Disk is newer SSD with 500GB with 5.5GB for the OS. 
Made basic DD test:

dd if=/dev/zero of=./testfile bs=1G count=1 oflag=direct
11.3s @ 88mb/s

Uses about 1GB RAM at rest, meaning with my new 8GB it would be perfect. (Swap was barely being used)

CPU(1 and 2) is between 3-13% in idle which is great.

I did find them to jump a lot. And let's say the patterns are interesting to say the least:

![20250121_175718](https://github.com/user-attachments/assets/9bb273a3-6add-4e8c-b2f3-cda1afd81d8d)

^^ Yes that's 100% on both cores just launching the browser.

Some other intresting patterns, is that it will prioritize loading on CPU2 and keep the CPU1 in lower half when doing heavier work. 
