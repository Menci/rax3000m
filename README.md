# CMCC RAX3000M Stock ROM Backup (and debrick guide)

## Enable SSH on Stock ROM

1. Backup configuration via web interface.
2. `openssl aes-256-cbc -d -pbkdf2 -k $CmDc#RaX30O0M@\!$ -in ../cfg_export_config_file.conf -out - | tar -zxvf -`
3. Delete root password in `etc/shadow`. Enable Dropbear in `etc/config/dropbear`.
4. `sudo tar -zcvf - etc | openssl aes-256-cbc -pbkdf2 -k $CmDc#RaX30O0M@\!$ -out ../cfg_export_config_file_new.conf`
5. Restore with new configuration via web interface.

The `cfg_export_config_file.conf` and `cfg_export_config_file_new.conf` files are uploaded to the `cfg_export_config_file` directory. However, you are encouraged to patch your own config to get SSH access.

## NAND version

### Partition Info

```
root@RAX3000M:~# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                14.5M     14.5M         0 100% /rom
tmpfs                   240.6M    384.0K    240.3M   0% /tmp
/dev/ubi0_4               5.7M    224.0K      5.2M   4% /overlay
overlayfs:/overlay        5.7M    224.0K      5.2M   4% /
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/mtdblock7            6.4M      6.4M         0 100% /mnt/mtdblock7
/dev/mtdblock8            6.4M      6.4M         0 100% /mnt/mtdblock8
ubi1_0                   28.8M     28.0K     27.3M   0% /plugin
/dev/mtdblock7            6.4M      6.4M         0 100% /tmp/cmcc/framework
root@RAX3000M:~# blkid
/dev/ubi1_0: UUID="2752f4c9-b06e-428d-a9ee-0f3539a634b8" TYPE="ubifs"
/dev/ubi0_1: TYPE="squashfs"
/dev/ubi0_3: TYPE="squashfs"
/dev/ubi0_4: UUID="cb2b9763-e412-42f1-9098-8723fc85fa14" TYPE="ubifs"
/dev/mtdblock5: UUID="704210855" TYPE="ubi"
/dev/mtdblock6: UUID="1433066931" TYPE="ubi"
/dev/mtdblock7: TYPE="squashfs"
/dev/mtdblock8: TYPE="squashfs"
/dev/ubiblock0_1: TYPE="squashfs"
root@RAX3000M:~# mtdinfo
Count of MTD devices:           9
Present MTD devices:            mtd0, mtd1, mtd2, mtd3, mtd4, mtd5, mtd6, mtd7, mtd8
Sysfs interface supported:      yes
root@RAX3000M:~# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 08000000 00020000 "spi0.0"
mtd1: 00100000 00020000 "BL2"
mtd2: 00080000 00020000 "u-boot-env"
mtd3: 00200000 00020000 "Factory"
mtd4: 00200000 00020000 "FIP"
mtd5: 03d00000 00020000 "ubi"
mtd6: 02500000 00020000 "plugins"
mtd7: 00800000 00020000 "fwk"
mtd8: 00800000 00020000 "fwk2"
```

### How did the backup

```bash
ssh 192.168.10.1 "cat /dev/mtd0 | gzip" > "mtd0-spi0.0.img.gz"
ssh 192.168.10.1 "cat /dev/mtd1 | gzip" > "mtd1-BL2.img.gz"
ssh 192.168.10.1 "cat /dev/mtd2 | gzip" > "mtd2-u-boot-env.img.gz"
ssh 192.168.10.1 "cat /dev/mtd3 | gzip" > "mtd3-Factory.img.gz"
ssh 192.168.10.1 "cat /dev/mtd4 | gzip" > "mtd4-FIP.img.gz"
ssh 192.168.10.1 "cat /dev/mtd5 | gzip" > "mtd5-ubi.img.gz"
ssh 192.168.10.1 "cat /dev/mtd6 | gzip" > "mtd6-plugins.img.gz"
ssh 192.168.10.1 "cat /dev/mtd7 | gzip" > "mtd7-fwk.img.gz"
ssh 192.168.10.1 "cat /dev/mtd8 | gzip" > "mtd8-fwk2.img.gz"
```

## eMMC version

### Partition Info

```
root@RAX3000M:~# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                14.5M     14.5M         0 100% /rom
tmpfs                   240.7M      1.6M    239.0M   1% /tmp
/dev/mmcblk0p8          254.0M     85.4M    168.6M  34% /overlay
overlayfs:/overlay      254.0M     85.4M    168.6M  34% /
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/mmcblk0p10           6.4M      6.4M         0 100% /mnt/mmcblk0p11
/dev/mmcblk0p11           6.4M      6.4M         0 100% /mnt/mmcblk0p11
/dev/mmcblk0p12          55.9G     52.0M     53.0G   0% /mnt/mmcblk0p12
/dev/mmcblk0p9           58.0M      1.3M     52.2M   2% /mnt/mmcblk0p9
/dev/mmcblk0p12          55.9G     52.0M     53.0G   0% /extend
/dev/mmcblk0p9           58.0M      1.3M     52.2M   2% /plugin
/dev/mmcblk0p10           6.4M      6.4M         0 100% /tmp/cmcc/framework
root@RAX3000M:~# blkid
/dev/mmcblk0p1: PARTLABEL="u-boot-env" PARTUUID="19a4763a-6b19-4a4b-a0c4-8cc34f4c2ab9"
/dev/mmcblk0p2: PARTLABEL="factory" PARTUUID="8142c1b2-1697-41d9-b1bf-a88d76c7213f"
/dev/mmcblk0p3: PARTLABEL="fip" PARTUUID="18de6587-4f17-4e08-a6c9-d9d3d424f4c5"
/dev/mmcblk0p4: PARTLABEL="kernel" PARTUUID="971f7556-ef1a-44cd-8b28-0cf8100b9c7e"
/dev/mmcblk0p5: TYPE="squashfs" PARTLABEL="rootfs" PARTUUID="309a3e76-270b-41b2-b5d5-ed8154e7542b"
/dev/mmcblk0p6: PARTLABEL="kernel2" PARTUUID="9c17fbc2-79aa-4600-80ce-989ef9c95909"
/dev/mmcblk0p7: PARTLABEL="rootfs2" PARTUUID="f19609c8-f7d3-4ac6-b93e-7fd9fad4b4af"
/dev/mmcblk0p8: LABEL="rootfs_data" UUID="aeec7dfd-27d3-443f-a5de-923b3815d550" BLOCK_SIZE="4096" TYPE="f2fs" PARTLABEL="rootfs_data" PARTUUID="a4a43b93-f17d-43e2-b7a7-df0bdf610c77"
/dev/mmcblk0p9: LABEL="plugins" UUID="ba156fe4-9009-4712-97dd-6b18eadfd054" BLOCK_SIZE="1024" TYPE="ext4" PARTLABEL="plugins" PARTUUID="518c1031-c234-4d49-8301-02e7ebe31231"
/dev/mmcblk0p10: TYPE="squashfs" PARTLABEL="fwk" PARTUUID="6e2bd585-7b0b-45b5-a8a1-4cf5436b1f73"
/dev/mmcblk0p11: TYPE="squashfs" PARTLABEL="fwk2" PARTUUID="fd8708ae-59c7-4ed5-a467-54bfe357cb48"
/dev/mmcblk0p12: LABEL="extend" UUID="f39febd9-4c71-4ec9-bd5d-7f1d6d81856a" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="data" PARTUUID="3c058515-54c3-452f-9b87-7a4f957b5cd1"
root@RAX3000M:~# fdisk -l /dev/mmcblk0
Found valid GPT with protective MBR; using GPT

Disk /dev/mmcblk0: 122126336 sectors, 2288M
Logical sector size: 512
Disk identifier (GUID): 2bd17853-102b-4500-aa1a-8a21d4d7984d
Partition table holds up to 128 entries
First usable sector is 34, last usable sector is 120800000

Number  Start (sector)    End (sector)  Size Name
     1            8192            9215  512K u-boot-env
     2            9216           13311 2048K factory
     3           13312           17407 2048K fip
     4           17408           82943 32.0M kernel
     5           82944          214015 64.0M rootfs
     6          214016          279551 32.0M kernel2
     7          279552          410623 64.0M rootfs2
     8          410624          934911  256M rootfs_data
     9          934912         1065983 64.0M plugins
    10         1065984         1098751 16.0M fwk
    11         1098752         1131519 16.0M fwk2
    12         1131520       120800000 57.0G data
```

### How did the backup

```bash
ssh 192.168.10.1 "cat /dev/mmcblk0p1 | gzip" > mmcblk0p1-u-boot-env.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p2 | gzip" > mmcblk0p2-factory.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p3 | gzip" > mmcblk0p3-fip.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p4 | gzip" > mmcblk0p4-kernel.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p5 | gzip" > mmcblk0p5-rootfs.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p6 | gzip" > mmcblk0p6-kernel2.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p7 | gzip" > mmcblk0p7-rootfs2.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p8 | gzip" > mmcblk0p8-rootfs_data.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p9 | gzip" > mmcblk0p9-plugins.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p10 | gzip" > mmcblk0p10-fwk.img.gz
ssh 192.168.10.1 "cat /dev/mmcblk0p11 | gzip" > mmcblk0p11-fwk2.img.gz

ssh 192.168.10.1 "cat /dev/mmcblk0boot0 | gzip" > mmcblk0boot0-bl2.img.gz
ssh 192.168.10.1 "dd if=/dev/mmcblk0 bs=512 count=8192 | gzip" > mmcblk0-gpt.img.gz
```

## Debrick

If you flash the ROM wrong and even can't enter (or don't have) U-Boot. You can rescure your RAX3000M with a USB serial. Remove the back of the router and you'll see 4 pins on top-right. The order (from the RJ45 port's side) is GND/TX/VCC/RX. Connect to GND/TX/RX with a USB serial. Run [mtk_uartboot](https://github.com/981213/mtk_uartboot) and connect power:

```
# mtk_uartboot \
    -a \
    -s /dev/tty.usbserial-1120 \
    -p mtk_uartbook_payload/mt7981-ddr4-bl2.bin \
    -f openwrt-23.05.3-mediatek-filogic-cmcc_rax3000m-nand-bl31-uboot.fip \
    --bl2-load-baudrate 460800
Using serial port: /dev/tty.usbserial-1120
Handshake...
hw code: 0x7981
hw sub code: 0x8a00
hw ver: 0xca00
sw ver: 0x1
Baud rate set to 460800
sending payload to 0x201000...
Checksum: 0x48b7
Setting baudrate back to 115200
Jumping to 0x201000 in aarch64...
Waiting for BL2. Message below:
==================================
NOTICE:  BL2: v2.10.0   (release):v2.4-rc0-5845-gbacca82a8-dirty
NOTICE:  BL2: Built : 20:20:25, Feb  2 2024
NOTICE:  WDT: Cold boot
NOTICE:  WDT: disabled
NOTICE:  EMI: Using DDR4 settings
NOTICE:  EMI: Detected DRAM size: 512MB
NOTICE:  EMI: complex R/W mem test passed
NOTICE:  CPU: MT7981 (1302MHz)
NOTICE:  Starting UART download handshake ...
==================================
BL2 UART DL version: 0x10
Baudrate set to: 460800
FIP sent.
==================================
NOTICE:  Received FIP 0xf5a99 @ 0x40400000 ...
==================================
```

> The file `mt7981-ddr4-bl2.bin` is from [here](https://www.cnblogs.com/p123/p/18046679) ([archive](https://archive.is/FpsqX)).

If everything goes well, your router will load the recovery image from TFTP.
