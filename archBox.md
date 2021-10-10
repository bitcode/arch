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














 






