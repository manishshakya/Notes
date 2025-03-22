#embedded-linux
#minimal-embedded-linux

# Minimal Embedded Linux

From Jarkko Sakkinen <jarkko@kernel.org>

```bash
#!/usr/bin/env bash

set -e

make defconfig
scripts/config --set-str CONFIG_INITRAMFS_SOURCE "initramfs.txt"
yes '' | make oldconfig

cat > initramfs.txt << EOF
dir /dev 755 0 0
nod /dev/console 644 0 0 c 5 1
nod /dev/loop0 644 0 0 b 7 0
dir /bin 755 1000 1000
slink /bin/sh busybox 777 0 0
file /bin/busybox initramfs/busybox 755 0 0
dir /proc 755 0 0
dir /sys 755 0 0
dir /mnt 755 0 0
file /init initramfs/init.sh 755 0 0
EOF

mkdir initramfs

curl -sSf https://dl-cdn.alpinelinux.org/alpine/edge/main/x86_64/busybox-static-1.36.1-r25.apk | tar zx --strip-components 1
cp busybox.static initramfs/busybox

cat > initramfs/init.sh << EOF
#!/bin/sh
mount -t proc none /proc
mount -t sysfs none /sys
sh
EOF
```
