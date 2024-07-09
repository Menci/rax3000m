# CMCC RAX3000M Stock ROM Backup

## NAND version

> I'll post it when I get one NAND version of RAX3000M with stock ROM.

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

### How to backup?

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
