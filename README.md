# g5-powerpc-restore

Checking for serials. 

You can find them under the main cover. I had bought 3 from FB marketplace (for 150$): 

```
Cl62901ev5w dual 2.3 512 250 6600 
G853758gru2 2.0 51 160 9600
```

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
```
240 Pin Dimm - 1.8v - PC2-4200 (533Mhz) - ECC Registered x4 2GiB
```
Bringing our ram from 2.5gb (2x 256mb) + 2x1gb to 10gb.
Which is considerable upgrade!
I actually kept all 4 previous RAMS and added the 4 new ones in matched pairs and didn't get any issues with the sizes. 

## Memory
---
We can use regular SATA SSDs which are backward compatible even tho we will not get the full speed, you can get them for cheap reconditionned (20$ for 512 GiB)

## GPU
---
You can also go wild and upgrade GPU using regular PCIe connectors, you can also get modifications boards with the remaining PCIe slots!

I'm trying to order some spare parts to see how to get NVMes to work and perhaps get more write speed ? 

## SPECIAL THANKS
---
https://www.youtube.com/watch?v=cS58kQ10qas

Check Casey Cullen's channel out on YouTube!!!!!

## Booting from USB using Open-Firmware
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

You can then boot from USB (I used the 3rd port down on back panel):

``` boot usb3/disk@2:2,\\yaboot ``` 
syntax might need slight adjustements based on the port you used and if you made weird partitions. 

_Note: Disk 2: partiton 2 is default_

Getting started
---
(Skip this if you want to know how to get it acutally to work LOL) 

Make sure to use a power pc compatible install, i tried debian.

Also make sure to plug the ethernet cable into the top port (port 0) as this will be used to make the full Debian install. 

I ignored most of the warnings for mirrors and minimal install, as I kind of knew it would be hell since I hadn't received the new RAM yet. 
Took 60 seconds for the first Init script to start...

And the mac pro g5 was screaming through half of the install. And the boot loader wasn't properly loaded for yaboot. Then the mirrors also didn't want to pick up on the powerpc architecture.
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

```dd if=/dev/zero of=./testfile bs=1G count=1 oflag=direct```

11.3s @ 88mb/s

Uses about 1GB RAM at rest, meaning with my new 8GB it would be perfect. (Swap was barely being used)

CPU(1 and 2) is between 3-13% in idle which is great.

I did find them to jump a lot. And let's say the patterns are interesting to say the least:

![20250121_175718](https://github.com/user-attachments/assets/9bb273a3-6add-4e8c-b2f3-cda1afd81d8d)

^^ Yes that's 100% on both cores just launching the browser.

Some other intresting patterns, is that it will prioritize loading on CPU2 and keep the CPU1 in lower half when doing heavier work. 
Now will re-try the same after installing new RAM sticks as that should help A LOT (about 4x space). 


Sorry about the horrible screenshots. 

https://www.youtube.com/watch?v=czZpD8Frlg8

Making it our own
---

How can we get updates for Fienix? 
----
Can be a little difficult due to the OS and how debian updates needs system architecture signatures. 
Get to root.
```
su
adminRootPW

cd

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <key_id>
```
(you find the key id in the error message when accessing mirrorslists)

Sources for my G5 add to synaptic (Start menu > Administration):
Find repositories in top bar. 

binary default
```
  deb http://ftp.ports.debian.org/debian-ports/ unstable main
  deb http://ftp.ports.debian.org/debian-ports/ unreleased main
```
source
```
deb-src http://ftp.debian.org/debian/ unstable main
```

You can also try sid releases which include powerpc ports:
It will also make you rename things that were badly renamed to fienix, press yes everytime you are asked. 

This will also most likely trigger a kernel update:

You will need to find your /boot folder
and /etc/yaboot.conf

uname -r to check current version

[Fienix blog post that helped](https://fienixppc.blogspot.com/p/troubleshooting-of.html)

Make Yaboot Parameter Permanent

When you find boot parameters that work, you can make them permanent by editing /etc/yaboot.conf as root: 

    Open Mate Terminal and enter "su -" and your root password
    Enter "nano /etc/yaboot.conf"
    Locate the section that begins with "image=/boot/Linux" and edit the "append" line to enter your boot parameter (or add the append line if one does not exist) so it looks similar to the below example (do not include "Linux" in the append line):  

    image=/boot/vmlinux
            label=Linux
            read-only
            initrd=/boot/initrd.img
            append="nouveau.modeset=1 video=TV-1:d video=offb:off video=nouveaufb:off"

     Press Ctrl + X to exit, and press Y to save when prompted
     In Mate Terminal, still as root, enter "ybin -v"

So i've found mine which pointed to vmlinux and vmlinux.old
This is importnat because if you're trying to upgrade you need to put the stuff to recover at the bottom properly so you don't have to restart from scratch. 

Don't forget after updating to newest paths, and setting the OLD machine!!!! 

`sudo ybin -v` do it everytime you make a change.

Then you should get a funny message "Blessing dev/sda2 with Holy Pinguin Pee"
Reload and you should be able to fully update & upgrade

If you f'ed up you can still get in shell at login by usign CTRL ALT f2 to open a shell. And change your boot back to old kernel.
Only after 50 retries of playing around with x11 settings:

sudo nano /etc/X11/xorg.conf.d/20-gpu.conf

```
Section "Device"
    Identifier "NVIDIA"
    Driver "nouveau"
    Option "NoAccel" "True"
EndSection
```

Save the file. 

```
echo "export LIBGL_ALWAYS_SOFTWARE=1" | sudo tee -a /etc/environment
```

sudo systemctl restart lightdm

You should be good to go. Make sure to reboot for the 150th time. 

You should then when you uname -a get `6.12.13` or whatever version you updated to. This process can be annoying and complex, especially working with several terminals (OF, Yaboot, and then Linux kernel updates). 
Upgraded from ` 6.0.0.6`.



---

Testing new NVME drive, made sure it was empty and MBR/DOS. 

sudo fdisk /dev/nvme0n1
then n, then p
then enter, enter. 
w to write changes.

Simply have to mount it now. 

Remade the dd test ```dd if=/dev/zero of=./testfile bs=1G count=10 oflag=direct```

But made it much longer using the count=10. And got around 800mb/s. 

---

Don't forget to change the root password :)
How fienix works is by giving you a limited profile and keeping the root clean.  

sudo passwd root

Do the same for fienix profile.

Then go to the sudoers file: 
And add:

```fienix ALL=(ALL:ALL) ALL``` 

Now you should be able to skip the `su` and use sudo instead. 
If it worked you can `sudo whoami` and it shoudl return root because of how sudo works. 
