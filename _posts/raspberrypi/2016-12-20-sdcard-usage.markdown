---
title: "SD Card 포맷 및 디스크 이미지 사용하기"
date: 2016-12-20 09:00:00 +0900
layout: post
tags: [Raspberry Pi, Raspberry Pi 3, Linux, Raspbian, 라즈베리파이, 라즈비안, openSUSE, LEAP 15.0, LEAP 42.3, SD Card, micro SD Card]
categories: [Raspberry Pi, Linux]
---

SBC, PC 등의 머신에서 SD Card 사용에 필요한 사항을 정리했다.

## SD Card

SD Card는 Secure Digital의 약자로 Flash memory(비휘발성) 카드 포맷이다.

자세한 내용은 [Wikipedia: SD Card](https://ko.wikipedia.org/wiki/SD_카드) 를 확인하자.

### SD Card format

1.Windows, macOS

SD Association의 SD Formatter 를 이용한다.

2.Linux

터미널에서 `fdisk`, `parted` 명령으로 포맷할 수 있다.


#### macOS

macOS는 Disk Utility 프로그램을 명령라인 `diskutil` 명령으로 파티션, 마운트 및 포맷을 할 수 있다.

`diskutil` 명령으로 목록과 파티션 마운트를 해제하고 `dd` 명령으로 디스크 지운다.

```terminal
$ diskutil list

$ diskutil unmountDisk /dev/diskX

$ sudo dd if=/dev/urandom of=/dev/diskX bs=1000000
```


디스크 파티션은 `partitionDisk` 를 사용해서 파티션할 수 있다.

```terminal
$ sudo diskutil partitionDisk <Disk> GPT| FAT|exFAT|JHFS+ NAME SIZE
```

 - SIZE: 0b 전체.

#### Linux

Ubuntu 등은 SD Card 슬롯에 삽입된 장치는 /dev/mmcblkX 같이 장치 이름을 붙인다. USB Stick 으로 SD Card 혹은 Micro SD Card를 사용하면 /dev/sdX 디스크 이름을 사용한다.

`lsblk` 명령은 디바이스 장치가 마운트 된 곳을 출력해 준다.

```terminal
$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0 119.2G  0 disk
├─sda1        8:1    0  84.5G  0 part
└─sda6        8:6    0  28.9G  0 part /
mmcblk0     179:0    0   7.5G  0 disk
├─mmcblk0p2 179:2    0   6.8G  0 part /media/qkboo/ROOT
├─mmcblk0p3 179:3    0 486.1M  0 part
└─mmcblk0p1 179:1    0   200M  0 part /media/qkboo/EFI
```

파티션 및 포맷을 위해 마운트된 파티션을 언마운트 한다.

```terminal
$ sudo umount /dev/mmcblk0p2
$ sudo umount /dev/mmcblk0p1
```

`parted` 유틸리티로 파티션을 삭제, 생성할 수 있다. 다음은 명령라인에서 옵션을 주어 사용하는 방법이다

```terminal
$ sudo parted /dev/mmcblk0 rm 1
$ sudo parted /dev/mmcblk0 rm 2
$ sudo parted /dev/sdc mkpart primary ext3 4MiB 100%
```

결과로 *mmcblk0* 디스크 드라이브에 **mmcblk0p1** 파티션이 생성된다.

다음은 위와 같은 내용으로 fat32 파티션을 `parted` 를 Shell로 사용하는 예이다.

```terminal
sudo parted /dev/mmcblk0
(parted) mklabel msdos
(parted) mkpart primary fat32 1MiB 100%
(parted) set 1 boot on
(parted) quit
```

마지막으로 파티션을 파일시스템으로 포맷해 준다.

```terminal
$ sudo mkfs -V -t vfat /dev/mmcblk0p1
```


### Check Bad sector

터미널에서 SD Card 삽입/추출 및 점검시 섹터 에러를 확인하기 위해 `dmesg` 로 감시할 수 있다. 다음 명령은 dmesg 가 발생하면, 내용 중 마지막 15줄을 터미널로 출력해 준다.

```terminal
$ watch "dmesg | tail -15"
```


#### `hdparam`

[hdparam](https://wiki.archlinux.org/index.php/hdparm)

```terminal
$ sudo hdparm -t /dev/mmcblk0

/dev/mmcblk0:
 Timing buffered disk reads:  58 MB in  3.09 seconds =  18.80 MB/sec
```


#### badblocks

```terminal
$ sudo badblocks -n -v /dev/mmcblk0
```


### Image를 SD Card에 쓰기

각 OS에서 `dd` 같은 Disk clone 도구를 사용해서 디스크 이미지를 SD Card에 쓸 수 있다. 

#### Etcher

모든 OS  플랫폼을 지원하는 **Etcher** 사용을 권장한다. [Etcher](http://etcher.io) 을 다운받아 이미지 파일 선택해 대상 SD Card 에 GUI로 쓸 수 있다.

![](/images/opensuse/etcher-image-writing.png){:width="640"}

`dd`를 사용한다면 이러지는 **macOS**, **Linux** 섹션을 참조한다.

#### `dd`로 이미지 쓰기

디스크 내용을 `dd` 명령으로 디스크 이미지 형태로 저장할 수 있다.

```terminal
$ sudo dd if=opensuse_jeos-20171030.img of=/dev/mmcblk0 bs=4m
```

압축 이미지로 제공하는 이미지 파일은 `zcat`, `xzcat`, `gzcat` 같은 표출 출력 유틸리티를 사용해, dd 명령에 파이프로 처리할 수 있다.

```terminal
$ xzcat openSUSE-Leap42.2-ARM-JeOS-raspberrypi3.aarch64.raw.xz | sudo dd of=/dev/rdisk1 bs=4m; sync
```


이제 이미지 SD Card를 보드에 넣고 부팅한 후 시스템 설정을 시작하면 된다.

시스템 구성을 하고, 여러 설정을 한 후 백업, 복사 등을 위해, 설정한 OS 를 디스크 이미지로 복제하는 경우가 많아 이때는 `dd` 명령을 사용할 수 있다.


### Disk clone

Linux, macOS 에서 파티션 도구, `dd` 를 사용해서 디스크 전체, 혹은 파티션을 이미지로 추출해 생성하는 과정을 정리했다.


#### Ubuntu `gparted`

우분투 Live CD에서 `gparted`는 가능한 사용하지 않는 공간은 빼고 최소 크기로 줄여 줄 수 있다.


#### 압축 이용하기

`dd` 는 전체 섹터를 추출하므로 디스크 크기의 이미지가 생성된다. 그러므로 추출후 압축 혹은 생성하며 압축해 디스크 이미지로 쓴다.

```terminal
dd bs=1M count=<size_in_MBs> if=/dev/sdX | gzip -c --fast| dd of=image.gz
dd bs=1M count=<size_in_MBs> if=/dev/sdX | xz -c --fast| dd of=image.xz
```


압축한 이미지 파일을 Sd Card 등에 복원하기 위해서 `unzip`, `gunzip`, `xz -d` 압축을 해제한 결과를 파이프로 `dd`로 전달한다.

```terminal
dd if=/path/to/image.gz | gunzip -c | dd bs=1M of=/dev/sdY
```



#### macOS의 Disk Utility 이용

GUI인 Disk Utility에서 SD Card 디스크를 선택하고 macOS의 *.dmg* 이미지 파일로 저장할 수 있고, 저장시 압축 선택이 가능하다.


<br>
### 이미지 파일 파티션 확인하기

백업한 디스크 이미지 파티션을 확인하고 폴더에 마운트해서 내용을 확인할 할 수 있다.

이미지 파일의 파티션 정보를 `fdisk`, `parted` 명령으로 확인할 수 있다.


#### `fdisk` 이용

```sh
$ fdisk -lu opensuse-e20.img
Disk opensuse-e20.img: 14.9 GiB, 16022241280 bytes, 31293440 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xe0e3966b

Device            Boot    Start      End  Sectors   Size Id Type
opensuse-e20.img1          2048   411651   409604   200M  c W95 FAT32 (LBA)
opensuse-e20.img2        413696 30282525 29868830  14.2G 83 Linux
opensuse-e20.img3      30283776 31278554   994779 485.7M 82 Linux swap / Solaris
```

#### `parted` 이용

```terminal
$ parted opensuse-42.3-homepi_16GB-firewalld.img
GNU Parted 3.2
Using opensuse-42.3-homepi_16GB-firewalld.img
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p
Model:  (file)
Disk opensuse-42.3-homepi_16GB-firewalld.img: 16.0GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type     File system     Flags
 1      1049kB  211MB   210MB   primary  fat16           lba
 2      212MB   15.5GB  15.3GB  primary  ext4
 3      15.5GB  16.0GB  509MB   primary  linux-swap(v1)
```

Byte 단위로 보려면

```terminal
(parted) unit
Unit?  [compact]? B
(parted) p
Number  Start         End           Size          Type     File system     Flags
 1      1048576B      210765823B    209717248B    primary  fat16           lba
 2      211812352B    15504653311B  15292840960B  primary  ext4
 3      15505293312B  16014620159B  509326848B    primary  linux-swap(v1)
```



<br>
### 이미지 마운트

디스크 이미지는 리눅스에서 `mount` 명령으로 마운트 할 수 있다. mount 명령으로 실제 파티션이 시작하는 위치를 offset으로 지정해서 마운트 한다.

앞서 사용한 opensuse-42.3-homepi_16GB.img 이미지의 파티션 정보가 다음 같다고 하자.

```terminal
Number  Start         End           Size          Type     File system     Flags
 1      1048576B      210765823B    209717248B    primary  fat16           lba
 2      211812352B    15504653311B  15292840960B  primary  ext4
 3      15505293312B  16014620159B  509326848B    primary  linux-swap(v1)
```

첫번째 파티션 시작 위치 *1048576B* 를 오프셋으로 삼아서 root 권한으로 `mount` 명령을 사용한다.

> 섹터 정보를 Byte 단위로 변경하려면, 파티션 정보에서 1 sector 크기가 512 를 확인하고, 이미피 파일의 파티션 시작 섹터를 곱해서 offset 범위를 계산한다.

```terminal
Sector size * Start = (in the case) 512 * 8192 = 4194304 (Byte)
```

이제 마운트 명령 `mount -o loop,ro,offset` 옵션을 주고, boot와 ROOT 파티션을 마운트 해보자,

Now we have the offsets and we can use those to mount the filesystems using the loopback device:

```terminal
$ sudo mount -o loop,ro,offset=1048576 opensuse-42.3-homepi_16GB.img ~/mnt/boot/
$ sudo mount -o loop,ro,offset=211812352 opensuse-42.3-homepi_16GB.img ~/mnt/ROOT/
```

마운트가 성공하면 `df` 명령 혹은 `mount` 명령으로 마운트한 위치를 확인할 수 있다;

```terminal
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.5G     0  1.5G   0% /dev
tmpfs           300M  5.1M  295M   2% /run
/dev/sda6        29G  7.3G   20G  28% /
tmpfs           1.5G  188K  1.5G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           1.5G     0  1.5G   0% /sys/fs/cgroup
/dev/loop0      200M  4.4M  196M   3% /home/qkboo/mnt/boot
/dev/loop1      200M  4.4M  196M   3% /home/qkboo/mnt/ROOT
```

*/dev/loop0* 파일 시스템이 지정한 마운트 위치에 마운트했다는 것을 알 수 있다.

또한 `-t [fs_type]` 옵션으로 파티션의 파일 시스템을 명시할 수 있다.


#### 이미지에서 한 파티션만 마운트 하기

이미지에 있는 하나의 파티션만 마운트 할 수 있다. [^3]

앞의 이미지 파일 정보를 `/dev/loop0` 에 offset을 주고

```terminal
sudo losetup -o 4194304 /dev/loop0 sda.img
```

Now the partition resides on /dev/loop0. You can fsck it, mount it etc

```
sudo fsck -fv /dev/loop0
sudo mount /dev/loop0 /mnt
```

Unmount

```
sudo umount /mnt
sudo losetup -d /dev/loop0
```


#### Truncate

[질문, 원래 2GB 데비안 이미지를 16GB SD Card에 썼다. 나머지 14GB 공간을 별도로 파티션 나눌 수 있나?](https://raspberrypi.stackexchange.com/questions/7177/image-of-a-16gb-card-containing-unpartitioned-space-at-the-end-truncating-possi)  

먼저 이미지 파티션 정보를 얻는다.

```sh
$ fdisk -lu image.img
Disk image.img: 4096 MB, 4096000000 bytes, 8000000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000a1bc7

      Device Boot      Start         End      Blocks   Id  System
image.img1              2048     5872026     5869978    b  W95 FAT32
```

이미지 파티션의 끝 부분이 *5872026* 으로 `5872026 * 512 = 2,8Gb` 이고, 나머지는 안쓰는 공간이다.


`truncate` 도구로 현재 파티션에서 빈공간을 잘라낸다. 다만 블럭 수가 0에서 시작하므로 섹터 수에 *1*을 더해준다.[^2]

```sh
$ truncate --size=$[(5872026+1)*512] image.img
```


### Repair

프롬프트에서 다음 명령으로 해당 파티션을 점검한다.

```
# fsck -fy /dev/mmcblk0p2
# reboo
```

혹은 다음 부팅시 처리할 수 있도록 한다.

```
# touch /forcefsck
# reboot
```



## 참조

[^2]: [Truncate free space at disk image](https://raspberrypi.stackexchange.com/questions/7177/image-of-a-16gb-card-containing-unpartitioned-space-at-the-end-truncating-possi)

[^3]: [Mount single partition from image of entire disk(device)](https://askubuntu.com/questions/69363/mount-single-partition-from-image-of-entire-disk-device)



