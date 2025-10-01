# Linux Архитектура и файловые системы

## Задание 1. Kernel and Module Inspection (15 баллов)
 
Продемонстрировать версию ядра вашей ОС:
```bash
neevin@test-vm:~$ uname -r
6.8.0-84-generic
```
 
Показать все загруженные модули ядра:
```bash
neevin@test-vm:~$ lsmod
Module                  Size  Used by
tls                   155648  0
binfmt_misc            24576  1
nls_iso8859_1          12288  1
joydev                 32768  0
input_leds             12288  0
virtio_input           16384  0
serio_raw              20480  0
sch_fq_codel           24576  5
dm_multipath           45056  0
msr                    12288  0
efi_pstore             12288  0
nfnetlink              20480  2
dmi_sysfs              24576  0
ip_tables              32768  0
x_tables               65536  1 ip_tables
autofs4                57344  2
btrfs                2043904  0
blake2b_generic        24576  0
raid10                 73728  0
raid456               192512  0
async_raid6_recov      20480  1 raid456
async_memcpy           16384  2 raid456,async_raid6_recov
async_pq               20480  2 raid456,async_raid6_recov
async_xor              16384  3 async_pq,raid456,async_raid6_recov
async_tx               16384  5 async_pq,async_memcpy,async_xor,raid456,async_raid6_recov
xor                    20480  2 async_xor,btrfs
raid6_pq              126976  4 async_pq,btrfs,raid456,async_raid6_recov
libcrc32c              12288  2 btrfs,raid456
raid1                  57344  0
raid0                  24576  0
crct10dif_pclmul       12288  1
crc32_pclmul           12288  0
polyval_clmulni        12288  0
polyval_generic        12288  1 polyval_clmulni
ghash_clmulni_intel    16384  0
sha256_ssse3           32768  0
sha1_ssse3             32768  0
psmouse               217088  0
aesni_intel           356352  0
crypto_simd            16384  1 aesni_intel
cryptd                 24576  2 crypto_simd,ghash_clmulni_intel
```

Отключить автозагрузку модуля cdrom:
```bash
neevin@test-vm:~$ sudo modprobe -r cdrom
modprobe: FATAL: Module cdrom is builtin.
```
Отключить модуль не получилось :(
 

Найти и описать конфигурацию ядра (файл конфигурации, параметр CONFIG_XFS_FS):
```bash
neevin@test-vm:~$ grep CONFIG_XFS_FS /boot/config-$(uname -r)
CONFIG_XFS_FS=m
```
Означает, что поддержка файловой системы XFS в ядре настроена как модуль.

## Задание 2. Наблюдение за VFS (20 баллов)
Используйте strace для анализа команды `cat /etc/os-release > /dev/null`. Для этого запустите `strace -e trace=openat,read,write,close cat /etc/os-release > /dev/null`. Описать открываемый и читаемый файл, объяснить отсутствие записывающих вызовов в выводе.
```bash
neevin@test-vm:~$ strace -e trace=openat,read,write,close cat /etc/os-release > /dev/null
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\243\2\0\0\0\0\0"..., 832) = 832
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
read(3, "# Locale name alias data base.\n#"..., 4096) = 2996
read(3, "", 4096)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_IDENTIFICATION", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_IDENTIFICATION", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MEASUREMENT", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MEASUREMENT", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_TELEPHONE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_TELEPHONE", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_ADDRESS", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_ADDRESS", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_NAME", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_NAME", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_PAPER", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_PAPER", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MESSAGES", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MESSAGES/SYS_LC_MESSAGES", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_MONETARY", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_MONETARY", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_COLLATE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_COLLATE", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_TIME", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_TIME", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_NUMERIC", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_NUMERIC", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/locale/C.UTF-8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/C.utf8/LC_CTYPE", O_RDONLY|O_CLOEXEC) = 3
close(3)                                = 0
openat(AT_FDCWD, "/etc/os-release", O_RDONLY) = 3
read(3, "PRETTY_NAME=\"Ubuntu 24.04.3 LTS\""..., 131072) = 400
write(1, "PRETTY_NAME=\"Ubuntu 24.04.3 LTS\""..., 400) = 400
read(3, "", 131072)                     = 0
close(3)                                = 0
close(1)                                = 0
close(2)                                = 0
+++ exited with 0 +++
```

Файл /etc/os-release успешно открыт (дескриптор 3), прочитано 400 байт данных, затем файл закрыт:
```bash
openat(AT_FDCWD, "/etc/os-release", O_RDONLY) = 3
read(3, "PRETTY_NAME=\"Ubuntu 24.04.3 LTS\""..., 131072) = 400
write(1, "PRETTY_NAME=\"Ubuntu 24.04.3 LTS\""..., 400) = 400
read(3, "", 131072)                     = 0
close(3)                                = 0
```
Файл открывается только на чтение `O_RDONLY`. Вычитывается 400 байт из дескриптора 3 (файла `/etc/os-release`), потом 400 байт записываются в дескриптор 1 (`stfout`). Write вызов присутствует, поскольку это нормальная работа программы `cat` - она не знает о конечном назначении своего вывода и поэтому пишет в свой `stdout`.


## Задание 3
Изначально нет дисков:
```bash
neevin@test-vm:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda     253:0    0   20G  0 disk 
├─vda1  253:1    0 19.4G  0 part /
├─vda14 253:14   0    4M  0 part 
└─vda15 253:15   0  600M  0 part /boot/efi
```
Подключаю диск через Yandex Cloud:
![](./disk.png)

Появился второй диск. Но он называется `vdb`, а не `sdb`:
```bash
neevin@test-vm:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda     253:0    0   20G  0 disk 
├─vda1  253:1    0 19.4G  0 part /
├─vda14 253:14   0    4M  0 part 
└─vda15 253:15   0  600M  0 part /boot/efi
vdb     253:16   0    2G  0 disk 
```

Создал раздел на `/dev/vdb`, используя fdisk:
```bash
neevin@test-vm:~$ sudo fdisk /dev/vdb

Welcome to fdisk (util-linux 2.39.3).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0xb067bec3.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (8192-4194303, default 8192): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (8192-4194303, default 4194303): 

Created a new partition 1 of type 'Linux' and of size 2 GiB.

Command (m for help): t
Selected partition 1
Hex code or alias (type L to list all): 8e
Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

neevin@test-vm:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda     253:0    0   20G  0 disk 
├─vda1  253:1    0 19.4G  0 part /
├─vda14 253:14   0    4M  0 part 
└─vda15 253:15   0  600M  0 part /boot/efi
vdb     253:16   0    2G  0 disk 
└─vdb1  253:17   0    2G  0 part 
neevin@test-vm:~$ sudo fdisk -l /dev/vdb
Disk /dev/vdb: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4194304 bytes
Disklabel type: dos
Disk identifier: 0xb067bec3

Device     Boot Start     End Sectors Size Id Type
/dev/vdb1        8192 4194303 4186112   2G 8e Linux LVM
```

Создать Physical Volume (PV) на этом разделе:
```bash
neevin@test-vm:~$ sudo pvcreate /dev/vdb1
  Physical volume "/dev/vdb1" successfully created.
neevin@test-vm:~$ sudo pvdisplay
  "/dev/vdb1" is a new physical volume of "<2.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/vdb1
  VG Name               
  PV Size               <2.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               Aym9VY-etkf-tgxv-EqIl-M8uo-rMLW-CMZuBe
```

Создать Volume Group (VG) с именем vg_highload:
```bash
neevin@test-vm:~$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda     253:0    0   20G  0 disk 
├─vda1  253:1    0 19.4G  0 part /
├─vda14 253:14   0    4M  0 part 
└─vda15 253:15   0  600M  0 part /boot/efi
vdb     253:16   0    2G  0 disk 
└─vdb1  253:17   0    2G  0 part 
neevin@test-vm:~$ sudo vgcreate vg_highload /dev/vdb1
  Volume group "vg_highload" successfully created
neevin@test-vm:~$ sudo vgdisplay vg_highload
  --- Volume group ---
  VG Name               vg_highload
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1.99 GiB
  PE Size               4.00 MiB
  Total PE              510
  Alloc PE / Size       0 / 0   
  Free  PE / Size       510 / 1.99 GiB
  VG UUID               VTrSgg-Lwic-ihMY-Gpcd-lJ4G-BT2U-oRmwFY
```

Создать два Logical Volume (LV): data_lv (1200 MiB) и logs_lv (оставшееся место):
```bash
neevin@test-vm:~$ sudo lvcreate -L 1200M -n data_lv vg_highload
  Logical volume "data_lv" created.
neevin@test-vm:~$ sudo lvcreate -l 100%FREE -n logs_lv vg_highload
  Logical volume "logs_lv" created.
neevin@test-vm:~$ sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg_highload/data_lv
  LV Name                data_lv
  VG Name                vg_highload
  LV UUID                am3ioU-BUi4-j89y-Pq2S-o5oc-Eto7-I2cxIo
  LV Write Access        read/write
  LV Creation host, time test-vm, 2025-09-30 23:23:58 +0000
  LV Status              available
  # open                 0
  LV Size                1.17 GiB
  Current LE             300
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     16384
  Block device           252:0
   
  --- Logical volume ---
  LV Path                /dev/vg_highload/logs_lv
  LV Name                logs_lv
  VG Name                vg_highload
  LV UUID                zNVVam-m7BT-f4Hw-SSWx-8b3D-3tMs-5LBfQ4
  LV Write Access        read/write
  LV Creation host, time test-vm, 2025-09-30 23:24:10 +0000
  LV Status              available
  # open                 0
  LV Size                840.00 MiB
  Current LE             210
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     16384
  Block device           252:1
```

Резульатат:
```bash
neevin@test-vm:~$ lsblk
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda                     253:0    0   20G  0 disk 
├─vda1                  253:1    0 19.4G  0 part /
├─vda14                 253:14   0    4M  0 part 
└─vda15                 253:15   0  600M  0 part /boot/efi
vdb                     253:16   0    2G  0 disk 
└─vdb1                  253:17   0    2G  0 part 
  ├─vg_highload-data_lv 252:0    0  1.2G  0 lvm  
  └─vg_highload-logs_lv 252:1    0  840M  0 lvm 
```

Отформатировать data_lv как ext4 и примонтировать в /mnt/app_data:
```bash
neevin@test-vm:~$ sudo mkfs.ext4 /dev/vg_highload/data_lv
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 307200 4k blocks and 76800 inodes
Filesystem UUID: 47073719-38d2-45dd-a385-1290717c5adf
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done 

neevin@test-vm:~$ sudo mkdir -p /mnt/app_data
neevin@test-vm:~$ sudo mount /dev/vg_highload/data_lv /mnt/app_data
neevin@test-vm:~$ df -h /mnt/app_data
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/vg_highload-data_lv  1.2G   24K  1.1G   1% /mnt/app_data
neevin@test-vm:~$ ls -la /mnt/app_data
total 24
drwxr-xr-x 3 root root  4096 Sep 30 23:30 .
drwxr-xr-x 4 root root  4096 Sep 30 23:30 ..
drwx------ 2 root root 16384 Sep 30 23:30 lost+found
```

Отформатировать logs_lv как xfs и примонтировать в /mnt/app_logs:
```bash
neevin@test-vm:~$ sudo mkfs.xfs /dev/vg_highload/logs_lv
meta-data=/dev/vg_highload/logs_lv isize=512    agcount=4, agsize=53760 blks
         =                       sectsz=4096  attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=1
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=215040, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=4096  sunit=1 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
neevin@test-vm:~$ sudo mkdir -p /mnt/app_logs
neevin@test-vm:~$ sudo mount /dev/vg_highload/logs_lv /mnt/app_logs
neevin@test-vm:~$ df -h /mnt/app_logs
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/vg_highload-logs_lv  776M   48M  729M   7% /mnt/app_logs
neevin@test-vm:~$ ls -la /mnt/app_logs
total 4
drwxr-xr-x 2 root root    6 Sep 30 23:27 .
drwxr-xr-x 3 root root 4096 Sep 30 23:27 ..
```

Как результат появились точки монтирования:
```bash
neevin@test-vm:~$ lsblk
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda                     253:0    0   20G  0 disk 
├─vda1                  253:1    0 19.4G  0 part /
├─vda14                 253:14   0    4M  0 part 
└─vda15                 253:15   0  600M  0 part /boot/efi
vdb                     253:16   0    2G  0 disk 
└─vdb1                  253:17   0    2G  0 part 
  ├─vg_highload-data_lv 252:0    0  1.2G  0 lvm  /mnt/app_data
  └─vg_highload-logs_lv 252:1    0  840M  0 lvm  /mnt/app_logs
```

## Задание 4. Использование pseudo filesystem (25 баллов)

Извлечь из /proc модель CPU и объём памяти (KiB):
```bash
neevin@test-vm:~$ cat /proc/cpuinfo | grep "model name" | head -1
model name	: Intel Xeon Processor (Icelake)

neevin@test-vm:~$ cat /proc/meminfo | grep "MemTotal"
MemTotal:        4008276 kB
```

Используя `/proc/$$/status`, найдите Parent Process ID (PPid) вашего текущего shell. что означает `$$`?
```bash
cat /proc/$$/status | grep PPid
```
`$$` - это специальная переменная shell, которая содержит PID (Process ID) текущего процесса shell.


Определить настройки I/O scheduler для основного диска /dev/vda:
```bash
neevin@test-vm:~$ cat /sys/block/vda/queue/scheduler
[none] mq-deadline 
```
`[none]` - квадратные скобки означают активный I/O scheduler. `mq-deadline` - альтернативный доступный scheduler. Сейчас никакого планироващика нет. Запросы к диску обрабатываются напрямую без очереди. 

Определить размер MTU для основного сетевого интерфейса (например, eth0 или ens33).
```bash
neevin@test-vm:~$ ip link show eth0 | grep mtu
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
```
Значение  Maximum Transmission Unit в 1500 байт стандартно для Ethernet сетей.



