# Arch PowerPC_64 - Mac G5

## ACTUAL SOLUTION BECAUSE YOU ARE STILL HERE AND I'M DONE WITH THE 🐰🐇 HOLES. 
### Happy 20th birthday Cheesegrater 🧀 <3
19/04/2025 FIRST BOOT WAS ON 04/2005. 

I fell asleep to this exact [YoutubeVid](https://youtu.be/K1T1eMyPIC4?si=yQbIAN3bgLJTLKfX&t=656)
Now we are here. Trying to revive the G5(PCIe version) one more time. Last one I promise🤞.  

----

So I'll dive in this last hole, probably the biggest one: **Arch.**
> Get the right iso, flash it, you know the drill. 
[ArchPower](https://archlinuxpower.org/)

> Get the PPC64 image (careful PPC64LE is not the same thing. Learned this the hardway.)
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

[ArchPowerGuide](https://github.com/kth5/archpower/wiki/Installation-%7C--NewWorld-PowerMac-with-Grub)

You can see here, the usb, the target. And my first partition ^^ I made 12MB instead of 32mb because I'm an idiot. Don't be an idiot. 

`mac-fdisk /dev/sdX` (X being your target, I will refer to it as `sda` from now on) 

`i` then `y`

`c` `64` `32M` 

`bootstrap` `Apple_Bootstrap` then 

`w` to write (will auto create Apple_Free for the rest of the disk) 

-----

## ARCH BUT WITHOUT THE LONG GUIDES 

`sda1` is Partition map (auto generated with what we did above) 

Meaning we have all we need (no swap, but fuck it, can do that later)

---

Format HFS 
`hformat /dev/sda2`

Now format root:
`mkfs.ext4 /dev/sda3`

Mount our root part (to /mnt):
`mount /dev/sda3 /mnt`

Mount bootstrap GRUB (Fuck Yaboot) 
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

---

Chroot into your root part (this basically is like going into the system) 
`arch-chroot /mnt`

Here you would do all the weird arch shenanigans. 

[ArchFullInstallationWiki](https://wiki.archlinux.org/title/Installation_guide)

`ip link` find your interfaces, check that one of them says `UP`

`ping google.com` should return packets. 

If not might be due to time: 

```
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
```

Locales / Time

`timedatectl set-timezone Europe/Paris`
`timedatectl` to check and `list-timezones` if unsure. 

`nano /etc/locale.gen` uncomment `#en_US.UTF-8 UTF8` or equivalent

then run `locale-gen` this should set the file in `/etc/locale.conf`

> Careful there is also a en_US iso 92831 this will corrupt most apps. 

Hostnames
`
echo "cheese-grater" > /etc/hostname
`

Hosts `nano /etc/hosts`

Then add: 
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

## If you forgot something (that was not critical)  

Because you're an idiot with severe impatience and adhd. You forgot one step somehwere. 
Boot back to USB. 

```
lsblk
mount /dev/sda3 /mnt
arch-chroot /mnt
pacman -S networkmanager
systemctl enable NetworkManager
exit
umount -R /mnt
reboot
```

Special thanks also to users on the discord of POWERARCH community who helped me! 

----

If you made it here probably means you are doing the same thing and perhaps you'll run into some issues:

### Create a user.

`useradd -m -G wheel joe` and `passwd joe`
Set adifferent pw than root. 

`sudo pacman -S xfce4 xfce4-goodies xorg`

``` 
pacman -S sudo
EDITOR=nano visudo
```
Uncomment the `#%wheel ALL=(ALL) ALL` line

```
sudo pacman -S lightdm lightdm-gtk-greeter
sudo systemctl enable lightdm
```

Browser > `pacman -S arcticfox` works perfectly fine with simple sites like github or even reddit.

![Screenshot 2025-04-20 174019](https://github.com/user-attachments/assets/8e2c8795-2576-43fc-a864-a20224019ae7)

That's it folks. 
