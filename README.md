# archlinux-notes
My Arch Linux Installation Notes and Settings to make things work in one shot!

## Installation Steps
- Boot the ISO
- Run cfdisk and partition the drives. Simple installs will need a 1M BIOS Boot partition, a Linux Swap partition, and at least one Linux Filesystem partition.
- Run mkfs.ext4 and mkswap on your new partitions.
- Mount your new file system partition to /mnt.
- Run pacstrap on /mnt. This will install the base system to /mnt.
  ``` 
  pacstrap -i /mnt /mnt/home base base-devel dialog wpa_supplicant networkmanager vim gdm sddm 
  ```
  > Because we need internet access on our latest no-ethernet port machines ! 
  
- Run genfstab -U /mnt >> /mnt/etc/fstab
- Also set up any other mounts like /home, /var, or swap spaces (/dev/sdXX none swap defaults 0 0)
- Run arch-chroot /mnt
- Set a hostname in /etc/hostname. (echo myhostname > /etc/hostname)
- Run `ln -sf /usr/share/zoneinfo/Asia/Kuala_Lumpur /etc/localtime`. Use your time zone file.
- Edit /etc/locale.gen and uncomment your locale (en_US.UTF-8 in many cases.)
- Run `locale-gen`
- Edit /etc/locale.conf and put LANG=en_US.UTF-8 or the locale you used.
- Run `mkinitcpio -p linux`
- Run `passwd` to set the root user's password.
- Run `pacman -S grub-bios os-prober`. If it didn't work, run `pacman -Sy` to update the repo information on local.
- Run `grub-install /dev/sdb`. Replace /dev/sdb with the drive you want to install the bootloader on. This will overwrite the MBR. If you don't know what that implies, you need to do some more preliminary reading on GRUB, bootloaders, and the MBR.
- Run `grub-mkconfig > /boot/grub/grub.cfg`. This will generate a GRUB menu list. Because we included os-prober package it will look for Windows and other operating systems and create menu items for them (convenient, huh!)
- Exit chroot. You can press CTRL-D to do this.
- Run `umount /mnt /mnt/home`
- Run `reboot`
