# Mac-G5-PowerPC64-Full-Restore

For Debian based > Fienix OS  [Here](https://github.com/h8d13/Mac-G5-2005-Restore/blob/main/DEBIAN-FienixOS-PPC64.md)

For Arch > More work, but cooler. [Here](https://github.com/h8d13/Mac-G5-2005-Restore/blob/main/ARCH-PPC64-G5.md)

For Adelie/Alpine, more packages. [Here](https://github.com/h8d13/Mac-G5-2005-Restore/blob/main/ADELIE-ALPINE-PPC64-G5.md)

Check for serials: 

You can find them under the main cover. I had bought 3 from FB marketplace (for 150$): 

```
Cl62901ev5w dual 2.3 512 250 6600 
G853758gru2 2.0 51 160 9600
```

So based on this we can already tell which one is useless and which one we want to mod :)

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

RAM speeds are low compared to today's standards but it's not felt that mutch using simple apps and code. 

## Memory
---
We can use regular SATA SSDs which are backward compatible even tho we will not get the full speed, you can get them for cheap reconditionned (20$ for 512 GiB)

This is probably the coolest upgrade as we can just throw out the old sata, throw in our new ssd and a nvme over PCIe for storage. 

## GPU
---
You can also go wild and upgrade GPU using regular PCIe connectors, you can also get modifications boards with the remaining PCIe slots!

I'm trying to order some spare parts to see how to get NVMes to work and perhaps get more write speed ? 


### More ugrades using 2 PCIe modboards

First one for a NVMe drive, made sure it was empty and MBR/DOS. 

sudo fdisk /dev/nvme0n1
then n, then p
then enter, enter. 
w to write changes.

Simply have to mount it now. 

Remade the dd test ```dd if=/dev/zero of=./testfile bs=1G count=10 oflag=direct```
But made it much longer using the count=10. And got around 800mb/s. 

### 2,5 GB/s using PCIe mod board
This is self-explanatory. 

---------------------

Here is a one hour documentary before apple became kinda garbage. 
[65Scribe](https://www.youtube.com/watch?v=czZpD8Frlg8)
