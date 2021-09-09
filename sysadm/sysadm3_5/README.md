машнее задание к занятию "3.5. Файловые системы"
1. Узнайте о sparse (разряженных) файлах.  
>Отличный способ экономить дисковое пространство на диске

2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?  
>root@vagrant: # touch file1  
root@vagrant:# ln file1 file2  
root@vagrant:# ls -l  
total 0  
-rw-r--r-- 2 root root 0 Sep  8 15:10 file1  
-rw-r--r-- 2 root root 0 Sep  8 15:10 file2  
root@vagrant:# chmod 777 file1  
root@vagrant:# ls -l  
total 0  
-rwxrwxrwx 2 root root 0 Sep  8 15:10 file1  
-rwxrwxrwx 2 root root 0 Sep  8 15:10 file2  
root@vagrant:# chown vagrant file1  
root@vagrant:# ls -l  
total 0  
-rwxrwxrwx 2 vagrant root 0 Sep  8 15:10 file1   
-rwxrwxrwx 2 vagrant root 0 Sep  8 15:10 file2  
Из этого примера следует что файлы, являющиеся жесткой ссылкой на объект имеют всего одни и те же права доступа как и владельца.  
3.	Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:  
>Vagrant.configure("2") do |config|  
  config.vm.box = "bento/ubuntu-20.04"  
  config.vm.provider :virtualbox do |vb|  
    lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"  
    lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"  
    vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]  
    vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]  
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]  
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]  
  end  
end  

Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.  
>root@vagrant: # lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT  
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part /boot/efi  
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /  
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]  
sdb                    8:16   0  2.5G  0 disk  
sdc                    8:32   0  2.5G  0 disk  
4.	Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.  
>root@vagrant:# lsblk  
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT  
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part /boot/efi  
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /  
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]  
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
└─sdb2                 8:18   0  511M  0 part  
sdc                    8:32   0  2.5G  0 disk  
	Создание таблицы разделов сделано через интерактивную часть fdisk /dev/sdb    
5.	Используя sfdisk, перенесите данную таблицу разделов на второй диск.
>root@vagrant:# sfdisk -d /dev/sdb|sfdisk --force /dev/sdc  
Checking that no-one is using this disk right now ... OK  
Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors  
Disk model: VBOX HARDDISK  
Units: sectors of 1 * 512 = 512 bytes  
Sector size (logical/physical): 512 bytes / 512 bytes  
I/O size (minimum/optimal): 512 bytes / 512 bytes  
 Script header accepted.  
 Script header accepted.  
 Script header accepted.  
 Script header accepted.  
 Script header accepted.  
 Script header accepted.  
 Created a new GPT disklabel (GUID: 2725A17C-DF56-DE42-A1D9-8A66579D7B70).  
/dev/sdc1: Created a new partition 1 of type 'Linux filesystem' and of size 2 GiB.  
/dev/sdc2: Created a new partition 2 of type 'Linux filesystem' and of size 511 MiB.  
/dev/sdc3: Done.  
New situation:  
Disklabel type: gpt  
Disk identifier: 2725A17C-DF56-DE42-A1D9-8A66579D7B70  
Device       Start     End Sectors  Size Type  
/dev/sdc1     2048 4196351 4194304    2G Linux filesystem  
/dev/sdc2  4196352 5242846 1046495  511M Linux filesystem  
The partition table has been altered.  
Calling ioctl() to re-read partition table.  
Syncing disks.    

Дампим таблицу разделов с первого диска и передаем на развертывание на втором, опуская проверку совеместимости.
6.	Соберите mdadm RAID1 на паре разделов 2 Гб.  
 > root@vagrant:# mdadm --create --verbose /dev/md1 -l 1 -n 2 /dev/sd{b1,c1}  
mdadm: Note: this array has metadata at the start and  
    may not be suitable as a boot device.  If you plan to  
    store '/boot' on this device please ensure that  
    your boot-loader understands md/v1.x metadata, or use  
    --metadata=0.90  
mdadm: size set to 2094080K  
Continue creating array?  
Continue creating array? (y/n) Y  
mdadm: Defaulting to version 1.2 metadata  
mdadm: array /dev/md1 started.   
root@vagrant:# lsblk  
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi  
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]  
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md1                9:1    0    2G  0 raid1  
└─sdb2                 8:18   0  511M  0 part  
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md1                9:1    0    2G  0 raid1  
└─sdc2                 8:34   0  511M  0 part  
7.	Соберите mdadm RAID0 на второй паре маленьких разделов.  
>root@vagrant:# mdadm --create --verbose /dev/md0 -l 0 -n 2 /dev/sd{b2,c2}  
mdadm: chunk size defaults to 512K  
mdadm: Defaulting to version 1.2 metadata  
mdadm: array /dev/md0 started.  
root@vagrant:# lsblk  
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi  
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]  
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md1                9:1    0    2G  0 raid1   
└─sdb2                 8:18   0  511M  0 part  
  └─md0                9:0    0 1017M  0 raid0  
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md1                9:1    0    2G  0 raid1  
└─sdc2                 8:34   0  511M  0 part  
  └─md0                9:0    0 1017M  0 raid0  

8.	Создайте 2 независимых PV на получившихся md-устройствах. 
>root@vagrant:# pvcreate /dev/md1 /dev/md0  
  Physical volume "/dev/md1" successfully created.  
  Physical volume "/dev/md0" successfully created.   
root@vagrant:# pvscan  
  PV /dev/sda5   VG vgvagrant       lvm2 [<63.50 GiB / 0    free]  
  PV /dev/md0                       lvm2 [1017.00 MiB]  
  PV /dev/md1                       lvm2 [<2.00 GiB]  
  Total: 3 [<66.49 GiB] / in use: 1 [<63.50 GiB] / in no VG: 2 [2.99 GiB]    
9.	Создайте общую volume-group на этих двух PV.  
>root@vagrant:# vgcreate vol_grp /dev/md1 /dev/md0  
  Volume group "vol_grp" successfully created    
  --- Volume group ---  
  VG Name               vol_grp  
  System ID  
  Format                lvm2  
  Metadata Areas        2   
  Metadata Sequence No  1     
  VG Access             read/write  
  VG Status             resizable  
  MAX LV                0  
  Cur LV                0   
  Open LV               0  
  Max PV                0  
  Cur PV                2  
  Act PV                2  
  VG Size               <2.99 GiB  
  PE Size               4.00 MiB  
  Total PE              765  
  Alloc PE / Size       0 / 0  
  Free  PE / Size       765 / <2.99 GiB  
  VG UUID               bGUj96-Gpa5-5dYK-daBf-yZJj-HWd4-wSVZda   
10.	Создайте LV размером 100 Мб, указав его расположение на PV с RAID0. 
>root@vagrant:# lvcreate -L 100M -n logical_vol vol_grp /dev/md0  
  Logical volume "logical_vol" created.  
root@vagrant:# lsblk  
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0  512M  0 part  /boot/efi  
├─sda2                      8:2    0    1K  0 part  
└─sda5                      8:5    0 63.5G  0 part  
  ├─vgvagrant-root        253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1      253:1    0  980M  0 lvm   [SWAP]  
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm  
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm   

11.	Создайте mkfs.ext4 ФС на получившемся LV.  

>root@vagrant:# mkfs.ext4 /dev/vol_grp/logical_vol  
mke2fs 1.45.5 (07-Jan-2020)  
Creating filesystem with 25600 4k blocks and 25600 inodes  
Allocating group tables: done  
Writing inode tables: done  
Creating journal (1024 blocks): done  
Writing superblocks and filesystem accounting information: done  
12.	Смонтируйте этот раздел в любую директорию, например, /tmp/new.  

>root@vagrant:# mkdir /mnt/new  
root@vagrant:# mount /dev/vol_grp/logical_vol /mnt/new  
root@vagrant:# lsblk  
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0  512M  0 part  /boot/efi  
├─sda2                      8:2    0    1K  0 part  
└─sda5                      8:5    0 63.5G  0 part  
  ├─vgvagrant-root        253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1      253:1    0  980M  0 lvm   [SWAP]  
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
13.	Поместите туда тестовый файл, например wget  https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.  
>root@vagrant:/# wget  https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /mnt/new/test.gz  
--2021-09-08 17:21:25--  https://mirror.yandex.ru/ubuntu/ls-lR.gz  
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183  
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.  
HTTP request sent, awaiting response... 200 OK  
Length: 21113289 (20M) [application/octet-stream]  
Saving to: ‘/mnt/new/test.gz’  
/mnt/new/test.gz          100%[===================================>]  20.13M   891KB/s    in 23s  
2021-09-08 17:21:48 (908 KB/s) - ‘/mnt/new/test.gz’ saved [21113289/21113289]  
14.	Прикрепите вывод lsblk.     
>root@vagrant:/# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0  512M  0 part  /boot/efi  
├─sda2                      8:2    0    1K  0 part  
└─sda5                      8:5    0 63.5G  0 part  
  ├─vgvagrant-root        253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1      253:1    0  980M  0 lvm   [SWAP]  
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
    └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
15.	Протестируйте целостность файла:  
root@vagrant:# gzip -t /tmp/new/test.gz  
root@vagrant:# echo $?  
0  


>Готово:
root@vagrant:/# gzip -t /mnt/new/test.gz  
root@vagrant:/# echo $?  
0  
16.	Используя pvmove, переместите содержимое PV с RAID0 на RAID1.  
>root@vagrant:/# pvmove /dev/md0 /dev/md1  
  /dev/md0: Moved: 60.00%  
  /dev/md0: Moved: 100.00%  
root@vagrant:/# lsblk  
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
sda                         8:0    0   64G  0 disk  
├─sda1                      8:1    0  512M  0 part  /boot/efi  
├─sda2                      8:2    0    1K  0 part  
└─sda5                      8:5    0 63.5G  0 part  
  ├─vgvagrant-root        253:0    0 62.6G  0 lvm   /  
  └─vgvagrant-swap_1      253:1    0  980M  0 lvm   [SWAP]  
sdb                         8:16   0  2.5G  0 disk  
├─sdb1                      8:17   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
│   └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
└─sdb2                      8:18   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
sdc                         8:32   0  2.5G  0 disk  
├─sdc1                      8:33   0    2G  0 part  
│ └─md1                     9:1    0    2G  0 raid1  
│   └─vol_grp-logical_vol 253:2    0  100M  0 lvm   /mnt/new  
└─sdc2                      8:34   0  511M  0 part  
  └─md0                     9:0    0 1017M  0 raid0  
17.	Сделайте --fail на устройство в вашем RAID1 md.  
>dmesgroot@vagrant:/# mdadm /dev/md1 --fail /dev/sdb1  
mdadm: set /dev/sdb1 faulty in /dev/md1  
18.	Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.    
>[ 7191.354067] md/raid1:md1: Disk failure on sdb1, disabling device.  
               md/raid1:md1: Operation continuing on 1 devices.  
19.	Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:  
root@vagrant:# gzip -t /tmp/new/test.gz  
root@vagrant:# echo $?  
0  

>root@vagrant:/# gzip -t /mnt/new/test.gz  
root@vagrant:/# echo $?  
0
20.	Погасите тестовый хост, vagrant destroy.  
>PS C:\devops\devops-netology\sysadm\vagrant> vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y  
==> default: Forcing shutdown of VM...  
==> default: Destroying VM and associated drives...  
