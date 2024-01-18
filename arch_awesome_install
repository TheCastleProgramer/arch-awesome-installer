wifi-menu
ping www.google.com
loadkeys es 

# setup disk partitions
cfdisk.

# format disk partitions
lsblk /dev/sda
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda2

# mount disk partitions.
# root partition.
mount /dev/sda1 /mnt
# home partition.
mkdir /mnt/home
mount /dev/sda2 /mnt/home

# install the base.
pacstrap /mnt base

# generate a fstab.
genfstab -U -p /mnt >> /mnt/etc/fstab
# alter fstab using following guide: https://wiki.archlinux.org/index.php/Fstab#Field_definitions
nano /mnt/etc/fstab

# https://wiki.archlinux.org/index.php/Chroot
arch-chroot /mnt /bin/bash

# set root password
passwd

# install GRUB2
pacman -S grub
grub-mkconfig -o /boot/grub/grub.cfg
nano /boot/grub/grub.cfg # edit menu-entry listings
grub-install --recheck /dev/sda

# unmount
exit
umount /mnt/{home,}
reboot

# setup permanent wired network connection
systemctl enable dhcpcd.service
systemctl start dhcpcd.service

# hostname
hostnamectl set-hostname arch

# keymap
# https://wiki.archlinux.org/index.php/KEYMAP
ls /usr/share/kbd/keymaps
localectl set-keymap es


# locale
nano /etc/locale.gen
# Uncomment the desired locale
locale-gen
localectl set-locale LANG=es_ES.UTF-8

# timezone
ls /usr/share/zoneinfo/
timedatectl set-timezone Europe/Madrid

# enable multilib for 32bit installation on 64bit arch
# https://wiki.archlinux.org/index.php/Multilib
nano /etc/pacman.conf
# [multilib]
# Include = /etc/pacman.d/mirrorlist
pacman -Syy

# setup user
useradd -m -g users -s /bin/bash alejandro
passwd alejandro
pacman -S sudo
visudo # enable user as desired
# login as your new account

# setup basic build utilities
sudo pacman -S multilib-devel fakeroot git jshon wget make pkg-config autoconf automake patch

# packer - Arch User Repository
wget http://aur.archlinux.org/packages/pa/packer/packer.tar.gz
tar zxvf packer.tar.gz
cd packer && makepkg
sudo pacman -U packer<tab>

# xorg
sudo pacman -S xorg-server xorg-xinit xorg-server-utils mesa
sudo pacman -S xorg-twm xorg-xclock xterm

# select the correct video drivers
pacman -Ss xf86-video | less

# virtualBox guest
sudo pacman -S virtualbox-guest-utils
sudo sh -c "echo -e 'vboxguest\nvboxsf\nvboxvideo' > /etc/modules-load.d/virtualbox.conf"
sudo systemctl enable vboxservice.service

# intel graphics
sudo pacman -S xf86-video-intel libva-intel-driver

# laptop touchpad
pacman -S xf86-input-synaptics

# awesome
# https://wiki.archlinux.org/index.php/Awesome
sudo pacman -S awesome vicious
# https://wiki.archlinux.org/index.php/Xinitrc
echo "exec awesome" >> ~/.xinitrc

mkdir -p ~/.config/awesome/
cp /etc/xdg/awesome/rc.lua ~/.config/awesome/