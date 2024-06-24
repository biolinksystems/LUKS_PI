e2fsck -f /dev/mmcblk0p2  
resize2fs -fM /dev/mmcblk0p2  
dd bs=4k count=1397823 if=/dev/mmcblk0p2 |sha1sum 
fdisk -l /dev/sda  
dd bs=4k count=1397823 if=/dev/mmcblk0p2 of=/dev/sda1  
dd bs=4k count=1397823 if=/dev/sda1 |sha1sum 

cryptsetup --cipher aes-cbc-essiv:sha256 luksFormat /dev/mmcblk0p2
cryptsetup luksOpen /dev/mmcblk0p2 sdcard  
dd bs=4k count=1397823 if=/dev/sda1 of=/dev/mapper/sdcard 
dd bs=4k count=1397823 if=/dev/mapper/sdcard |sha1sum 
e2fsck -f /dev/mapper/sdcard
resize2fs -f /dev/mapper/sdcard
