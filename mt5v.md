# Build minimal linux

## Setup for Build

```bash
export ARCH=arm
export CROSS_COMPILE=arm-none-linux-gnueabihf-
```

# Beagleboard black

## Build u-boot

```bash
git clone https://source.denx.de/u-boot/u-boot.git
git checkout v2025.01
make distclean
make am335x_evm_defconfig;q

make
```

## Build Busybox

```bash
git clone git://git.busybox.net/busybox
cd busybox/
git checkout master
make clean && make defconfig && make LDFLAGS="--static"
```

## Build linux

```bash
git clone https://kernel.googlesource.com/pub/scm/linux/kernel/git/stable/linux.git
cd linux/
make omap2plus_defconfig
make -j$(nproc)
make dtbs
./scripts/config --set-str CONFIG_INTTRAMFS_SOURCE "initramfs.txt"
make -j$(nproc)
```

## Contains of extlinux.conf

Newer uboot requires(v2025.01), you to have `/extlinux/extlinux.conf` from the boot Device
Contain of extlinux.conf

```
label Linux microSD
    kernel /zImage
    FDTDIR /
    append console=ttyS0,115200n8
```

## Creating a Bootable SD Card

To create a bootable SD Card

- Create a FAT32 partiions and copy am335x-boneblack.dtb, MLO, u-boot.img, zImage
- Create `/extlinux/extlinux.conf` file in the fat32 partitions
- Create partiotion for rootfs

# Beagleboard white

Same as back but use `am335x-bone.dtb` file.

# 6sx-Sabre-SDB

## Build u-boot

```bash
export ARCH=arm
export CROSS_COMPILE=arm-none-linux-gnueabihf-
make distclean
make mx6sxsabresd_defconfig
make -j$(nproc)
```

## Flash u-boot in the FAT32 formated SD Card

```bash
sudo dd if=u-boot-dtb.imx of=/dev/mmcblk0 bs=512 seek=2
```

## Build linux

```bash
make imx_v6_v7_defconfig
scripts/config --set-str CONFIG_INITRAMFS_SOURCE "initramfs.txt"
make menuconfig
make -j$(nproc)
```

## Creating a Bootable SD Card

The following instructions are based on creating the SD card partitions and copy the files to the SD card using a Linux OS.

The recommended SD card structure is:

- No Partition (Beginning of SD Card after 1024Bytes)
- UBoot Image
- FAT32 Partition (BOOT)
- Kernel (zImage)
- Device Tree Blob (dtb)
- EXT4 Partition (rootfs)

### Prepare the SD card partitions

Set the disks label useing parted. In this case msdos

```bash
sudo parted -s /dev/<YOUR_SD_CARD> mklabel msdos
```

### Create the FAT32 and EXT4 partitions

```bash
        sudo sfdisk /dev/<YOUR_SD-CARD> <<EOF
        label: dos
        16065,128520,0x0C,*
        144585,,,-
        EOF
```

### Create the FAT32 filesystem

```bash
        sudo mkfs.fat -F32 -v -n "BOOT" /dev/<YOUR_SD_CARD>1
```

### Create the EXT4 filesystem

```bash
        sudo mkfs.ext4 -F -L "rootfs" /dev/<YOUR_SD_CARD>2
```

### Copy your files to the SD card

- UBoot is stored at the beginning of the drive but skips 1024bytes to ensure the MBR remains intact

```bash
        sudo dd if=<YOUR_u-boot.imx> of=/dev/<YOUR_SD_CARD> bs=512  seek=2
```

- Kernel and DTB

```bash
        sudo cp <YOUR_zImage> <YOUR_MOUNTED_BOOT_PARTITION>
        sudo cp <YOUR_dtb> <YOUR_MOUNTED_BOOT_PARTITION>
```

- Filesystem

```bash
        sudo tar -xvjf <YOUR_ROOTFS.tar.bz2> -C <YOUR_MOUNTED_ROOTFS_PARTITION>

```

- Sync the drive to ensure all data has been completely written before removing the SD card from the PC

```bash
    sync
```

### Example Custom SD card commands

```bash
sudo parted -s /dev/sdd mklabel msdos
sudo sfdisk /dev/sdd<<EOF
label: dos
16065,128520,0x0C,*
144585,,,-
EOF

sudo mkfs.fat -F32 -v -n "BOOT" /dev/sdd1
sudo mkfs.ext4 -F -L "rootfs" /dev/sdd2

sudo dd if=/home/user/u-boot-imx/u-boot.imx of=/dev/sdd bs=512  seek=2
sudo cp /home/user/kernel-imx/zImage /media/user/BOOT
sudo cp /home/user/kernel-imx/igep-0046.dtb /media/user/BOOT
sudo tar -xvjf /home/user/rootfs/ubuntu_0046.tar.bz2 -C /media/user/rootfs

sync
```
