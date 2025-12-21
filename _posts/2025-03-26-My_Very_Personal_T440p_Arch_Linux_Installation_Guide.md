---
layout: post
title: My Very Personal T440p Arch Linux Installation Guide
date: 2025-03-26 00:00:00 +0800
category: Tutorial
tags: [tutorial]
---
I've been using macOS for pretty much 6 years now, and Windows since I was at the young age. In between I've always been curious about Linux but never really seriously learn or actually using them on a daily basis, until recently with me hosting Minecraft Server, I decided to use Ubuntu as the system and hosted Minecraft server on it. But for my secondary laptop, ThinkPad T440p. I've decided to go with something that's technically more suitable for more experienced Linux users, which is Arch Linux.

I decided to write this guide to myself to memorize things I did during the installation, pretty much everyone who used Arch just tell people to follow the Arch Wiki instead, it's actually a really good and detailed guide but it only covers the very basics, so I've decided to gather up stuffs that I've searched through the internet and make my own "customized" guide that hopefully could also help others to install Arch Linux with DE, I'll be picking KDE as my desktop environment in this guide.

My ThinkPad T440p configurations. (As of Mar 26th, 2025)

CPU: Intel Core i7-4702MQ 4C8T 2.20 GHz

GPU: Intel HD Graphics 4600

Ram: SK Hynix 16GB DDR3L-1600 (2x 8GB)

Disk: Transcend MTS430S 256GB SSD / Kingston UV500 240GB SSD / Crucial BX500 240GB SSD

OS: Windows 10 / macOS Monterey / Arch Linux

Additional compoments:
AUO 14" B140HAN01.3 1080p IPS Display
Broadcom DW1560 Wi-Fi/Bluetooth Combo Card
Backlit Keyboard
T450 Synaptics Trackpad

Final Setup:
<img src="{{ site.baseurl }}/images/20250326_T440pArch/T440p_Arch_SS.png" width="800"/>

#  • Before Installation

## Prerequisite
Bootable Arch Install USB ([Download Link](https://archlinux.org/download/)), I used [Ventoy](https://www.ventoy.net/en/index.html) personally but [Rufus](https://rufus.ie/) also works too. If you don't have Windows you can also use [balenaEtcher](https://etcher.balena.io), that program is cross-platform but I personally never used it before.
 
A working internet connection, Suggest using Ethernet since nobody knows if your Wi-Fi will work after installation. In my case Wi-Fi worked when booting into Arch USB but have to install drivers after installation.

## Get started

Check BIOS settings, although you don't need to tweak much unlike dealing with Hackintosh, but if you have Secure Boot enabled, you have to disable it beforehand because Arch USB doesn't support Secure Boot, can be setup to work with secure boot after installation if desired. But I keep it disabled since I also have Hackintosh (macOS) on the system.

### Boot into Arch Install USB

First verify if it's boot into UEFI with this command: `cat /sys/firmware/efi/fw_platform_size`
In most cases it should return: 64. If it shows: No such file or directory. You are most likely Legacy Booted.

Check internet connection with ping command: `ping -c 3 archlinux.org`

### Disk Setup

Here I'll be using fdisk to list all the disks and then use cfdisk to do the partitioning job.

Enter `fdisk -l` to list all the disks. Check the disk you wanted to install, mine will be on `/dev/sdc`.

Enter cfdisk by typing `cfdisk /dev/sdc`.

You will be greeted with cfdisk's TUI interface.

In here you'll have to setup 3 partitions, for each is EFI, Swap and OS itself.

I'll install Arch on my 240GB SSD, Here's how I'll setup my partitions:
EFI - 500MB
Swap - 20GB (Depends on your RAM and usage)
Root - The rest of the remaining storages.

If you don't know how much swap you need, you can use [SwapCalc](https://pickwicksoft.github.io/swapcalc/), it's a handy tool to calculate how much swap you might need.

Now that we are ready to setup the partitions, let's get back on PC.

In cfdisk, we first select "New" and type the size as we needed. For example if I wanted to partition with the size 512MB, type `512M`, for 120GB we type `120G`.

Move to "Type" and select size type, EFI system for EFI, Linux Swap for swap, no need to adjust root partition.

After finished the prequirements, move to "Write" to write changes, we are officially done on cfdisk, Quit it.

Now we'll need to format the partitions and enable swap:

EFI: `mkfs.fat -F32 /dev/sdc1`
Root: `mkfs.ext4 /dev/sdc3`
Swap: `mkswap /dev/sdc2` -> `swapon /dev/sdc2`

### Mount File Systems

Root: `mount /dev/sdc3 /mnt`
EFI: `mount --mkdir /dev/sdc1 /mnt/boot`

# • Installation and Configuration

Base System Installation: `pacstrap -K /mnt base linux linux-firmware`
Fstab File Generation: `genfstab -U /mnt >> /mnt/etc/fstab`
Chroot into new system: `arch-chroot /mnt`

Install sudo and nano:
`pacman -S sudo nano`

Timezone Setup:
Change the timezone to where you are. In my case, I'll set it to Taiwan, Taipei.

Timezone Set: `ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime`
Generate /etc/adjtime: `hwclock --systohc`

Localization Setup:
You can use any text editor for your preferring, I'll use nano in here.

Firstly you'll have to type `nano /etc/locale.gen` to edit locale gen file.
Secondly uncomment (Remove #) for your locale, I will uncomment both en_US.UTF-8 and zh_TW.UTF-8 UTF-8.
Save and Exit.
Do `locale-gen`.
Add LANG into /etc/locale.conf: `echo LANG=en_US.UTF-8 > /etc/locale.conf` (I use English as my primary system language across every devices so that's why it's not zh_TW.)

Network, Hosts and Hostname config:
Replace `ShazStar-T440p-Arch` of your choice.

Set Hostname: `echo ShazStar-T440p-Arch > /etc/hostname`

Add Hosts:
`echo "127.0.0.1 localhost" >> /etc/hosts
echo "::1 localhost" >> /etc/hosts
echo "127.0.1.1 ShazStar-T440p-Arch" >> /etc/hosts`

Install & Enable NetworkManager:
`pacman -S networkmanager
systemctl enable NetworkManager`

Wi-Fi Driver: `sudo pacman -S broadcom-wl` (Got Broadcom DW1560 due to Hackintosh)
Enable Bluetooth Service: `sudo systemctl enable bluetooth`

Change root user's Password: `passwd`

Add new user and give sudo permission: 
Replace `shazstar` with your username of choice.

Add new user: `useradd -mG wheel shazstar`
If you want to add new user without sudo command privileges: `useradd -m [username]`
Set Password: `passwd shazstar`

Allow Wheel Group to use Sudo Commands: 
`EDITOR=nano visudo`
Find this line `#%wheel ALL=(ALL) ALL` and uncomment it.
Save and Exit.

Bootloader Install:
In here I picked systemd-boot for my Linux bootloader.
Install: `bootctl install`
Notes: There's an issue where in systemd-boot v257 it will fail on creating boot entry. [Link for Github Issue Pages and Workaround](https://github.com/systemd/systemd/issues/36174)

Create Boot Entry and Configure bootloader:

Configure: `nano /boot/loader/loader.conf` -> uncomment everything -> add `default arch.conf` -> Save and Exit.
Extra Notes: Since I don't want the boot menu to show up, I changed my timeout from default 3 to 0 so it boot straight into Arch Linux.

Create Boot Entry (Arch Linux and Arch Linux Fallback):
Check for root partition UUID: `blkid /dev/sdc3` (Change /dev/sdc3 to your root partition)

Arch Linux: `nano /boot/loader/entries/arch.conf` and add these lines as follows:
`title    Arch Linux
linux    /vmlinuz-linux
initrd   /initramfs-linux.img
options  root=UUID="[root partition UUID]" rw`
Save and Exit.

Arch Linux Fallback: `nano /boot/loader/entries/arch-fb.conf` and add these lines as follows:
`title Arch Linux Fallback
initrd /initrams-linux-fallback.img`
Save and Exit.

Base Installation and System Configuration should be finished here by now, exit chroot and reboot system.
Command as follows: `exit` and then `reboot`

# • Post Install Setup

Since I still don't have a desktop environment yet, I'll need to finish up with the installation.

SDDM Installation & Enable: `sudo pacman -Syu sddm` ->  `sudo systemctl enable sddm`
KDE Installation: `sudo pacman -Syu plasma-meta  kde-applications-meta` and go with the default option.
Fonts of choice: Noto Fonts -> `sudo pacman -Syu noto-fonts-cjk`
After finished everything, do a reboot and you'll be greet with SDDM, then you will be able to use KDE.

Extra Setup:

Arch AUR Helper - yay (Yet another Yogurt):
`sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si`

Edit makepkg.conf and change MAKEFLAG to -j$(nproc) to make building AUR package faster by using all available CPU cores:
`sudo nano /etc/makepkg.conf` -> Find MAKEFLAGS -> Uncomment and change "-j2" to "-j$(nproc)".

Edit pacman.conf to prettify pacman output and enable parallel downloads:
`sudo nano /etc/pacman.conf` -> Find Misc -> Uncomment ParallelDownloads, Color and VerbosePkgLists -> Add ILoveCandy

Chinese Input (Ignore this if you aren't using Chinese lmao):
`sudo pacman -Syu fcitx5-im fcitx5-chinese-addons fcitx5-chewing` -> Enable it thru KDE System Settings

Miscs:
Flatpak: `sudo pacman -Syu flatpak` -> `flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo`
Firewall (UFW): `sudo pacman -Syu ufw`
Fingerprint: `sudo pacman -Syu fprintd` -> Setup in System Settings -> Users. (Can only be used once you were logged in.)
sshfs (For KDE Connect to be able to browse remote devices.): `sudo pacman -Syu sshfs`
Geoclue (For Night Light): `sudo pacman -Syu geoclue`

**Useful Links.**

[Arch Wiki](https://wiki.archlinux.org/title/Installation_guide)

[Arch Plasma Install by XxAcielxX](https://github.com/XxAcielxX/arch-plasma-install)