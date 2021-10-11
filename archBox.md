# Pre-Installation

`lsblk` - for finding out which devices are present

`gdisk /dev/{device}` - to get drive ready for installation

`n` for new partition.

# First partition will be EFI

leave first sector default
EFI size partition will be `+550M`
code for EFI partition is `ef00`

### Second Partition will be SWAP

SWAP size partition will be `+1G`
code for SWAP partition is `8200`

### Third Patition will be root

root size parittion is up to the User
code for root partition is `8300` ( Linux filesystem )

### Fourth Partition will be /home 

all defaults will be fine

write changes with `w`

### Formatting

We will be using `ext4` with the following commands

EFI Partition  	
`mkfs.vfat /dev/nvme1n1p1`

SWAP Partition 
`mkswap /dev/nvme1n1p2`

Activate SWAP Partition
`swapon /dev/nvme1n1p2`

Root Partition 
`mkfs.ext4 /dev/nvme1n1p3` 

Same for /home partition
`mkfs.ext4 /dev/nvme1n1p4`

###Mount Partition

root partition goes on /mnt 
`mount /dev/nvme1n1p3 /mnt`

create directories & mount boot
`mkdir -p /mnt/boot/efi`

then mount the boot drive 
`mount /dev/nvme1n1p1 /mnt/boot/efi`

create directory for the /home
`mkdir /mnt/home`

then mount the /home direcotry
`mount /dev/nvme1n1p4 /mnt/home`

we don't need to mount SWAP partition

now `lsblk` to double check your mount points

### Install Base Packages into the root mount ( /mnt )

`pacstrap /mnt base linux linux-firmware git vim intel-ucode`

## Generate FileSystem Table 

`genfstab -U /mnt >> /mnt/etc/fstab`

## Go into installation 

`arch-chroot /mnt`

look at fstab file with 
`cat /etc/fstab` 

creat user
`useradd --create-home bit`
`passwd bit`
`usermod -aG wheel bit`
`visudo`

## Download and run install script

`git clone https://github.com/bitcode/arch.git`

reboot

# Post Installs

### DWM

`cd /usr/src/`
`git clone git://git.suckless.org/dwm`
`cd dwm`
`sudo make clean install`

### ST

`cd /usr/src/`
`git clone git://git.suckless.org/st`
`cd st`
`sudo make clean install`

### rofi ( install is in basePacks )

rofi config is in: `~/.config/rofi/config.rasi`
more info @ `https://wiki.archlinux.org/title/Rofi#Configuration`

to add rofi to dwm lets edit config.h
`static const char *roficmd[] = { "rofi", "-show", "drun", "-show-icons", NULL };`
and
`{ MODKEY, XK_d, spawn, {.v = roficmd } },`

get google chrome from AUR
`git clone https://aur.archlinux.org/google-chrome.git`
make the package with
`makepkg --syncdeps`
install downloaded file with 
`pacman -U file_name`

# Install Blackarch on top of Arch

BlackArch Linux is compatible with existing/normal Arch installations. It acts as an unofficial user repository. Below you will find instructions on how to install BlackArch in this manner.

# Run https://blackarch.org/strap.sh as root and follow the instructions.

$ curl -O https://blackarch.org/strap.sh
# Verify the SHA1 sum

$ echo 46f035c31d758c077cce8f16cf9381e8def937bb strap.sh | sha1sum -c
# Set execute bit

$ chmod +x strap.sh
# Run strap.sh

$ sudo ./strap.sh
# Enable multilib following https://wiki.archlinux.org/index.php/Official_repositories#Enabling_multilib and run:

$ sudo pacman -Syu
You may now install tools from the blackarch repository.
# To list all of the available tools, run

$ sudo pacman -Sgg | grep blackarch | cut -d' ' -f2 | sort -u
# To install all of the tools, run

$ sudo pacman -S blackarch
# To install a category of tools, run

$ sudo pacman -S blackarch-<category>
# To see the blackarch categories, run

$ sudo pacman -Sg | grep blackarch
# Note - it maybe be necessary to overwrite certain packages when installing blackarch tools. If
# you experience "failed to commit transaction" errors, use the --needed and --overwrite switches
# For example:

$ sudo pacman -Syyu --needed blackarch --overwrite='*'

# Install Nerd Font

`git clone https://github.com/ryanoasis/nerd-fonts.git`
`cd nerd-font`
`./install.sh`

# 







 






