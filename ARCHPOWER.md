# The ACTUAL SOLUTION BECAUSE YOU ARE STILL HERE AND I'M DONE WITH THE 🐰🐇 HOLES. 
### Happy 20th birthday Cheesegrater 🧀 <3

NEW PLAN? Incoming. 19/04/2025 FIRST BOOT WAS ON 04/2005. 
So I'll dive in this last hole, probably the biggest one: Arch. 

Get the right iso, flash it, you know the drill. 
[ArchPower](https://archlinuxpower.org/)

Get the PPC64 image (careful PPC64LE is not the same thing. Learned this the hardway.)
Then instead of the boot usb command use the `boot ud:,\\:tbxi` 

Use `devalias` in OpenFirmware white console thing to see your ud device. 
If it's not there you might need to set it manually `devalias ud /pci@f2000000/usb@1b/disk@1`. 
What I did was restart twice and it set itself. 

---

Once the ram risk is loaded you should get to a shell `root@archiso`
We will now have to do the actual arch install, without `archinstall` type installer. 
So let's get our hands dirty... (And want to kill ourselves a little.)

----

## Partitioning for HFS
> I had unplugged my drive because it had the debian install and kept booting to it (Weird yaboot binary points directly to it, even booting off usb would get me to the debian install)

When i plugged it back in I couldnt find it in `lsblk` or `fdisk -l`
HERE IS THE FIX WITHOUT REBOOTING!
`echo "- - -" > /sys/class/scsi_host/host0/scan` Idk, you tell me. 

![image](https://github.com/user-attachments/assets/9fcda476-fb02-4d12-a44f-d85f02054a2b)

Yuu can see here, the usb, the target. And my first partition ^^ I made 12MB instead of 32mb because I'm an idiot. Don't be an idiot. 

[ArchPowerGuide](https://github.com/kth5/archpower/wiki/Installation-%7C--NewWorld-PowerMac-with-Grub)

`mac-fdisk /dev/sdX` (X being your target) 

`i` then `y`

`c` `64` `32M` 

`bootstrap` `Apple_Bootstrap`

`w` to write (will auto create Apple_Free) 

-----

## ARCH BUT WITHOUT THE LONG GUIDES 

`sda1` is Partition map (auto generated with what we did above) 
Format HFS 
`hformat /dev/sda2`

Now format root:
`mkfs.ext4 /dev/sda3`


Mount our root part:
`mount /dev/sda3 /mnt`

Mount bootstrap (Fuck Yaboot) 
``` 
mkdir -p /mnt/boot/grub
mount /dev/sda2 /mnt/boot/grub
```

Install base system
```
pacstrap /mnt base linux-ppc64 grub hfsutils base-devel
```
Fstab
`genfstab -U /mnt > /mnt/etc/fstab`

Chroot into your root part
`arch-chroot /mnt`

Here you would do all the weird arch shenanigans.

`ip link` find your interfaces, check that one of them says `UP`
`ping google.com` should return packets. 

```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
```

Generate locales. 
`timedatectl set-timezone Europe/Paris`
`timedatectl` to check and `list-timezones` if unsure. 


Hostnames
`
echo "cheese-grater" > /etc/hostname
`

Hosts
```
127.0.0.1    localhost
::1          localhost
127.0.1.1    cheese-grater
```

Networking

```
pacman -S networkmanager
systemctl enable NetworkManager
```


Passwd

```
passwd
```

Finish the grub stuff
```
grub-mkconfig -o /boot/grub/grub.cfg
grub-install
```

Exit chroot
```
exit
umount -R /mnt
reboot
```


-------

## If you forgot something (that was not ctritical)  

Because you're an idiot with severe impatience and adhd. You forgot one step somehwere. 
Boot back to USB. 

```
mount /dev/sda3 /mnt
arch-chroot /mnt
pacman -S networkmanager
systemctl enable NetworkManager
exit
umount -R /mnt
reboot
```


Special thanks also to users on the discord of POWERARCH community who helped me! 

