# Pi4B-4G; 硬盘1T 安装日志 

## 分区 gpt引导  
02 pi4b, kde22 本地运行  manjaro kde  (ssh ； real-vnc)  
03 pi4work, pi4664 黑卡8G  带ui  稳定终端  
04 arm64， mj终端, 绿8G   
05 u20s, 兰8G   ubuntu server 2020
06 opui  
07 op64  

## 分区大小
220m   指向2  
220G  pi4b       kde22  
180G  pi4work    pi4664  
160G  arm64     mj终端   最新  
140G  u20s    
100G  opui  
130G  op64   

## 操作parted
sudo parted /dev/sda  
--mktable msdos  
mklabel gpt  
  yes  
mkpart primary fat32 0% 220M  
mkpart primary ext4 220M 220G   
mkpart primary ext4 220G 400G  
mkpart primary ext4 400G 560G  
mkpart primary ext4 560G 700G  
mkpart primary ext4 700G 800G  
mkpart primary ext4 800G -1  
 set 1 boot on  
  
  
 print  
 Ctrl + C  
  
硬盘分区    220M   
硬盘分区    220G  180G  160G    
硬盘分区    140G  100G  130G   

## 分区格式化 
sudo mkfs.vfat -n BOOT22 -F 32 /dev/sda1 
  
sudo mkfs.ext4 /dev/sda2  
sudo e2label /dev/sda2 PI4B  
  
sudo mkfs.ext4 /dev/sda3  
sudo e2label /dev/sda3 PI4WORK  
  
sudo mkfs.ext4 /dev/sda4  
sudo e2label /dev/sda4 ARM64  
  
sudo mkfs.ext4 /dev/sda5  
sudo e2label /dev/sda5 U20S  
  
sudo mkfs.ext4 /dev/sda6  
sudo e2label /dev/sda6 OF4UI  
  
sudo mkfs.ext4 /dev/sda7  
sudo e2label /dev/sda7 OF64  
  
## 查看分区  
  
lsblk -f  
sda                                                                                     
├─sda1      vfat   FAT32 BOOT22     EA57-188E                                           
├─sda2      ext4   1.0   PI4B       660f39e1-2df0-48c2-a0cb-9cd7d2ce8c02                
├─sda3      ext4   1.0   PI4WORK    ddd7cb3f-c270-43d2-ae46-ab6ca0dc0bc7                
├─sda4      ext4   1.0   ARM64      100c2f1b-8bb7-47c1-b01c-352c2c5fc21f                
├─sda5      ext4   1.0   U20S       2881eba4-9671-46ff-a479-60d499b5a3bd                
├─sda6      ext4   1.0   OF4UI      18cb3feb-6675-4424-9d9e-3809cd87f31b                
└─sda7      ext4   1.0   OF64       6dfa15bc-7060-4945-ad89-799ed9996aa3  
mmcblk0                                                                                 
├─mmcblk0p1 vfat   FAT16 BOOT_MNJRO B288-86B2                             157.1M    26% /boot
└─mmcblk0p2 ext4   1.0   ROOT_MNJRO 12e5e452-8cdf-44ba-835a-4e7271e485f7    7.9G    40% /

sudo blkid
/dev/mmcblk0p1: SEC_TYPE="msdos" LABEL_FATBOOT="BOOT_MNJRO" LABEL="BOOT_MNJRO" UUID="B288-86B2" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="2a586bd1-01"
/dev/mmcblk0p2: LABEL="ROOT_MNJRO" UUID="12e5e452-8cdf-44ba-835a-4e7271e485f7" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="2a586bd1-02"
/dev/sda1: LABEL_FATBOOT="BOOT22" LABEL="BOOT22" UUID="EA57-188E" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="primary" PARTUUID="76797cb4-c1b5-4c23-bee7-02f3807dc889"
/dev/sda2: LABEL="PI4B" UUID="660f39e1-2df0-48c2-a0cb-9cd7d2ce8c02" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="5a0daad5-e84e-4f32-8fba-497c636d5395"
/dev/sda3: LABEL="PI4WORK" UUID="ddd7cb3f-c270-43d2-ae46-ab6ca0dc0bc7" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="256a1479-602a-48ad-8821-56c153f1dd58"
/dev/sda4: LABEL="ARM64" UUID="100c2f1b-8bb7-47c1-b01c-352c2c5fc21f" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="11379a98-aca1-45af-bd56-09c34559c02a"
/dev/sda5: LABEL="U20S" UUID="2881eba4-9671-46ff-a479-60d499b5a3bd" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="7d7d1cec-80d1-4734-8b45-1162938f6c55"
/dev/sda6: LABEL="OF4UI" UUID="18cb3feb-6675-4424-9d9e-3809cd87f31b" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="b8320ea9-b23b-42a4-91bc-dfcda00d1ac5"
/dev/sda7: LABEL="OF64" UUID="6dfa15bc-7060-4945-ad89-799ed9996aa3" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="c3cc1d98-90e7-447e-94fb-0bcb6f2801f9"
/dev/zram0: LABEL="zram0" UUID="ca8e4f15-2825-4ac9-9849-267a035c3b27" TYPE="swap"  


  
ls -l /dev/disk/by-partuuid/  

lrwxrwxrwx 1 root root 10  4月 11 01:22 11379a98-aca1-45af-bd56-09c34559c02a -> ../../sda4  
lrwxrwxrwx 1 root root 10  4月 11 01:22 256a1479-602a-48ad-8821-56c153f1dd58 -> ../../sda3  
lrwxrwxrwx 1 root root 15  3月 16 01:47 2a586bd1-01 -> ../../mmcblk0p1  
lrwxrwxrwx 1 root root 15  3月 16 01:47 2a586bd1-02 -> ../../mmcblk0p2  
lrwxrwxrwx 1 root root 10  4月 11 01:22 5a0daad5-e84e-4f32-8fba-497c636d5395 -> ../../sda2  
lrwxrwxrwx 1 root root 10  4月 11 01:21 76797cb4-c1b5-4c23-bee7-02f3807dc889 -> ../../sda1  
lrwxrwxrwx 1 root root 10  4月 11 01:23 7d7d1cec-80d1-4734-8b45-1162938f6c55 -> ../../sda5  
lrwxrwxrwx 1 root root 10  4月 11 01:23 b8320ea9-b23b-42a4-91bc-dfcda00d1ac5 -> ../../sda6  
lrwxrwxrwx 1 root root 10  4月 11 01:24 c3cc1d98-90e7-447e-94fb-0bcb6f2801f9 -> ../../sda7  


