# The last? Rabbit hole. Adélie/Alpine PPC64

So I swore ArchPower was the final stop after the weird debian based fienixOS. 

Obviously I knew there was still one stop...
That being MUSL based distros: Adélie and Alpine LINUX.

Thanks to ActionRetro (altho he cheated and used a Disc instead of USB in 2025).

So again boot of the USB: `boot ud,:\\:tbxi`

---

## Prepare HFS

First and only thing you will need is to properly format partitions. We can use them sam approach exactly as on arch power as it's the format that is expected by OpenFiremware:

```
mac-fdisk /dev/sdX (X being your target, I will refer to it as sda from now on)

i then y

c 64 32M

Apple_Bootstrap

w
y
q
``` 

To do so you'll have to `su` then password: `live`

---

After installing we still have one trick to do.

`dir pci9/k2-sata-root@c/k2-data@0/disk@0:,\`

this should show grub executables if not play around finding out with `dev / ls`

https://youtu.be/zwLclq5LxIE?si=gvFruUtNJsJrTs4Q&t=384

Then `setenv boot-device pci9/k2-sata-root@c/k2-data@0/disk@0:,\grub`

And finally: `boot pci9/k2-sata-root@c/k2-data@0/disk@0:,\grub`

Trick: use arrow up and mofify your command.

If you got the NVIDIA model, you'll have to follow this part [too](https://github.com/h8d13/Mac-G5-2005-Restore/blob/main/README.md#env)

---

Now why MUSL based distros? Ported more apps !

---

This time the PC only reported 6GiB of RAM of the installed 10 from the previous Arch run. But honestly I couldn't give less of shit anymore.

<img width="1920" height="1080" alt="Screenshot 2025-11-28 21-38-10" src="https://github.com/user-attachments/assets/f484b155-23a4-4da4-ba41-f7db1d9cd85b" />
