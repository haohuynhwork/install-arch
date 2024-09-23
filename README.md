### Install Arch Linux basic
1. Connect to internet
    - `ip addr show`
    - `iwctl sation wlan0 get-networks`
    - `iwctl station wlan0 connect "Network name"`

2. Start ssh
    - `systemctl start sshd`

3. Add mirrors from https://archlinux.org/mirrorlist
    - `curl 'mirror_list_link' > /etc/pacman.d/mirrorlist`
    - `vim /etc/pacman.d/mirrorlist`

4. Makeformats and mount
    - `fdisk -l, lsblk`
    - `mkfs.fat -F32 /dev/...`
    - `mkfs.ext4 /dev/nvme0n1p7`
    - `mount -m /dev/nvme0n1p7 /mnt`

5. Install Arch linux base packages
    - `pacstrap -i /mnt base`

6. Generate the /etc/fstab file:
    - `genfstab -U -p /mnt >> /mnt/etc/fstab`

7. Chroot into installed system
    - `arch-chroot /mnt`

8. Linux
    - `pacman -S linux linux-headers linux-lts linux-lts-headers`

9. Firmware
    - `pacman -S linux-firmware`

10. Graphics drivers

    `lspci` to get what type of GPU you have
    
    - Intel

        `pacman -S mesa intel-media-driver`

    - Amd

        `pacman -S mesa libva-mesa-driver`

    - Nvidia

        `pacman -S nvidia nvidia-utila nvidia-lts`


11. Essensial packages
    - `pacman -S base-devel dosfstools mtools efibootmgr`
        <br>`grub os-prober networkmanager sudo nano openssh`
    - `pacman -S plasma-meta plasma-workspace konsole kwrite dolphin ark`
        `okular gwenview spectacle kwalletmanager partitionmanager`

12. Change root passwd
    - `passwd`

13. Add user
    - `useradd -mg users -G wheel owen`
    - `passwd owen`
    - `nano /etc/sudoers`

14. Enable ssh
    - `systemctl enable ssh`

15. Generate locale
    - `nano /etc/locale.gen`
    - `locale-gen`

16. Mount efi
    - `mount -m /dev/nvme0n1p1 /efi`

17. Install grub
    - `nano /etc/default/grub`
    - `grub-install --recheck --target=x86_64-efi --bootloader-id=grub`
        <br>`--efi-directory=/efi`
    - `os-prober`
    - `grub-mkconfig -o /boot/grub/grub.cfg`

18. Enabel display manager
    - `systemctl enable sddm`

19. Enable network manager
    - `systemctl enable NetworkManager`

20. Reboot computer
    - `exit`
    - `umount -a`
    - `reboot`

### Config

1. Empty password
    - `$y$j9T$9iAd/BXe87wH7aYE5CCTA1$E0zAsUOkbnpYVb4rjO7vlABm/eXWCETbQIgd7jpHDD8`

2. Set time
    - `timedatectl set-local-rtc 1`
3. Enable system request
    - `sysrq`
    -  `# echo "kernel.sysrq = 1" >> /etc/sysctl.d/99-sysctl.conf`
4. Mount ntfs-3g partition
    - `ntfs-3g`
    - `sudo blkid`
    - `nano /etc/fstab`
      
        ```
        UUID="12345678" /run/media/owen/Disk ntfs-3g defaults, hide_hid_files,gid=984,uid=1000 0 0
        ```

5. Install gtk file chooser

    `sudo pacman -S xdg-desktop-portal-gtk`
    - `nano ~/.config/xdg-desktop-portal/portals.conf`

        ```
        [preferred]
        default=gtk
        ```
    or
    - `~/.config/systemd/user/xdg-desktop-portal.service.d/override.conf`

        ```
        [Service]
        Environment="XDG_CURRENT_DESKTOP=GNOME"
        ```

6. Additional packages
    - `sudo pacman -S appmenu-gtk-module ibus ibus-bamboo libreoffice-still`
    - `sudo pacman -S gimp inkscape python-tinycss2`
