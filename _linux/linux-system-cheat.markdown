## Cheat Linux System



### GRUB 복구

우분투 부트로더인 GRUB가 잘못되어 복구하려면 해당 배포본의 Live CD, Live USb 등으로 부팅해서 'Try Ubuntu' 에서 터미널을 통해 복구 절차를 시작한다.

https://askubuntu.com/questions/88384/how-can-i-repair-grub-how-to-get-ubuntu-back-after-installing-windows


https://help.ubuntu.com/community/Grub2

https://wiki.ubuntu.com/Grub2#Recover

## Keyboard

### Capslock과 Control key 교체하기


> 키맵 이용해서 다음 명령:
>  - 
> ```
> $setxkbmap -layout us -option ctrl:nocaps
> ```
> 
> ```
> sudo vi /etc/default/keyboard
> 
> XKBOPTIONS="ctrl:nocaps"
> ```
> 
> ```
> sudo dpkg-reconfigure keyboard-configuration
> ```
> 
> 

## dd


### 블록 크기

디스크에 데이터를 쓸려면 디스크의 기본 블록 크기인 512B 보다 큰게 좋다. 또한 쓰기 속도를 증가시키기 위해서 디스크의 물리적 지형에 맞는 크기를 사용하는 것이 좋다. fdisk 같은 유틸리티로 정보를 확인할 수 있고 혹은 sysfs 정보를 확인할 수 있다.

```
/sys/block/sdX/size
/sys/block/sdX/queue/physical_block_size
/sys/block/sdX/queue/logical_block_size
/sys/block/sdX/sdXY/alignment_offset
/sys/block/sdX/sdXY/start
/sys/block/sdX/sdXY/size
```


### 디스크 지우기

dd에 데이터를 쓸려면 디스크의 기본 블록 크기인 512B 보다 커야 한다. 또한 쓰기 속도를 증가시키기 위해서 디스크의 물리적 지형에 맞는 크기를 사용하는 것이 좋다.


```
$ sudo dd bs=8k if=/dev/urandom of=/dev/rdisk2
```

fdisk 정보로 확인

```
$ sudo fdisk -l /dev/sdb
Disk /dev/sdb: 59.6 GiB, 64021856256 bytes, 125042688 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 441CD64A-70D1-4A40-ACA8-05CAB62C5C89

Device       Start       End   Sectors  Size Type
/dev/sdb1     2048   8390655   8388608    4G Linux swap
/dev/sdb2  8390656 125042654 116651999 55.6G Linux LVM
```

fdisk 결과의 Sector size는 전체 용량과 섹터 크기를 계산하면 논릭적인 섹터의 크기를 확인할 수 있다. 

```
echo $((64021856256/125042688))
512
```

`Sector size (logical/physical): 512 bytes / 512 bytes` 에서 물리 크기를 확인할 수 있다.


#### 성능

dd 를 사용해서 1024 바이트를 1000000 블록에 걸쳐 쓰기를 수행한다 - 1GB

```
$ time sudo dd bs=1024 count=1000000 if=/dev/zero of=1GB_file
```

dd 를 사용해서 1GB 크기 파일을 1024 바이트씩 읽기를 한다.

```
$ time sudo dd bs=1024 if=1GB_file of=/dev/null
```


##### 참조 
 - http://askubuntu.com/questions/511467/how-can-i-completely-erase-all-data-on-a-micro-sd-card


## Terminal  Reset

실수로 바이너리 파일을 cat 하거나 하여 글자들이 깨질때.. 터미널 리셋하는 방법

```
$ reset
$ tput sgr0
$ setterm -reset
$ setterm -initialize
$ Ctrl + V, Ctrl + O
```



## System Info


```
cat /etc/os-release     # Raspbian
uname -a
cat /etc/issue
```


### Hardware Info

```
lscpu
lshw
lspci
lsusb
lsblk // block devices
lspcmcia
```

```
cat /proc/cpuinfo
```

// BIOS안의 시스템 정보
//dmidecode

//memory
$free -m


### Architecture

```
$cat /etc/issue
```

Ubuntu 에서

```
$subo lsb_release -a
```

```
$ dpkg --print-architecture      // 
armhf
```

```
$ uname -a
i686, i386은 32bit, x86_64는 64bit. armv7은 32bit, armv8은 64bit
$ uname -m    // machine inf
```

$ uname -i     // hw platform
$ uname -p     // processor

$ arch       // uname -m 과 동일

32bit 64bit 체크
$ getconf LONG_BIT       // 시스템 구성 질의

$ file /bin/ls
ELF 32-bit LSB executable, ARM, EABI5 version 1 ..

ARM
arm 7 is 32 bit. 
ARMv8-A, October 2011, 64-bit address space and 64-bit arithmetic 지원
$ uname -m    // machine inf
i686                // 32bit
x86_64           // 64bit
armv7l            // 32bit
armv8             // 64bit

설정
// Time zone
```
$ sudo dpkg-reconfigure tzdata
```


## find

```
find ./ -name 'Debug' -exec rm -rf {} \;
```


### 파일 삭제

???? 같이 지워지지 안는 파일 같은 경우  inode로 삭제할 수 있다.

```sh
~$ ls -ali
~$ ls -ali
total 2068
42467329 drwxr-xr-x 11 qkboo qkboo    4096 Jun 20 22:54 .
       2 drwxr-xr-x 10 qkboo qkboo    4096 Jun  1 12:56 ..
42467482 drwxr-xr-x  2 qkboo qkboo   12288 Jul  2  2016 .Picasa3Temp
42598444 drwxrwxrwx  3 qkboo qkboo    4096 Mar 29 23:45 ??????
42475521 drwxr-xr-x  4 qkboo qkboo    4096 Jun 20 22:52 Design_Assets
42467936 drwxr-xr-x 14 qkboo qkboo    4096 Mar  4 16:39 Incoming

~$ find . -inum 42598444 -exec rm {} \;
```



## rsync

http://www.joinc.co.kr/w/Site/Tip/Rsync

> rsync -avzh source destination
> -a : 심볼릭 링크, 속성, 퍼미션, 소유권 등 보존
> -v : 자세한 정보출력
> -z : 전송시 압축
> -r : 하위디렉토리포함
> -e ssh : ssh를 이용한 rsync 동기화
> --stats : 결과출력


### 기본 옵션

```sh
rsync -av source/ destination/
```

#### include와 exclude

이 옵션을 이용해서 대상 파일을 추가하거나 제외 할 수 있다.


```sh
$ rsync -avz  --exclude 'data' id@192.168.56.101:/home/backups ./
```

별표(*)도 사용할 수 있다.


```sh
$ rsync -avz  --exclude '*.cache' id@192.168.56.101:/home/backups ./
```


#### 증분 백업

수정/변경된 내용만 동기화한다.

> rsync -avzh moniwiki/ /tmp/backups/
> -h, --human-readable :  output numbers in a human-readable format
> -u :  --update update only (don't overwrite newer files)
> --delete : 서버동기화후 원본에서 파일이 삭제되면 백업에서도 파일을 삭제
> --remove-source-files : 




```sh
$ rsync -av /home/ /backup/home/ # 원본 증분 백업
$ rsync -av --delete /home/ /backup/home/ #원본 증분 백업, 원본 파일 삭제시 사본에서도 삭제
```


### 동기화 옵션

#### delete 옵션

목적지에 파일이나 디렉토리가 존재할 경우 삭제하고 싶을 때 `--delete` 옵션을 사용한다.

```sh
rsync -avz --delete id@192.168.56.101:/home/backups ./
```


#### 원본 삭제 옵션

`--remove-source-files`를 이용하면, 전송이 끝난 후 원본파일을 삭제한다.


```sh
$rsync --remove-source-files -zvh backup.tar /tmp/backups/
```



#### ssh 통한 백업

rsync 에는 ssh 를 이용하여 원격서버에 접속하여 동기화를 하는 기능이 있습니다.

```
rsync -azrtv --delete --stats -e "ssh -i /root/.ssh/개인키" 원본서버계정@원본서버주소:원본경로/ /백업경로/
```

#### find와 결합

```
find . -type f -mtime -3  | rsync -avz --files-from=- /soucepc /data/backup
```


## faillog

/var/log/faillog 는 계정의 로그인 실폐 횟수 정보를 바이너리 파일로 저장한다. 다음 C struct 구조의 정보가 바이너리로 저장되고 있다:

```c
struct    faillog {
    short   fail_cnt;
    short   fail_max;
    char    fail_line[12];
    time_t  fail_time;
    long    fail_locktime;
};
```

직접 읽을 수 없기 때문에 `faillog` 명령을 사용한다.

```sh
$ faillog -u pi        # pi 계정
$ faillog -a
Login       Failures Maximum Latest                   On

root            0        0   01/01/70 00:00:00 +0000
daemon          0        0   01/01/70 00:00:00 +0000
```

최근 3일의 로그인 실패를 찾으려면 :

```sh
$ faillog -t 3 -u pi
```



## fsck

fsck(for file system consistency check) 명령은 파일 시스템을 조사하여 손상된 파일을 출력해 주며 사용자에게 그것을 복구할 것인지를 질의
fsck 수행은 시스템마다 약간의 차이가 있지만 대부분 다음과 같은 5개 항목에 대하여 검사 
Blocks and sizes, Pathname, Connectivity, Reference count, Free List 

### 

```
$ sudo fsck –t ext2 /dev/hdb
```


#### Attempt to repair damaged blocks


```
$ sudo fsck –a
```


#### Repair damaged blocks interactively

```sh
$ sudo fsck -r <drive>
```



### force

#### Forcing the check
You can also force fsck at boot time by passing `fsck.mode=force`, as a kernel parameter. This will check every filesystem you have on the machine.


#### /forcefsck

Create a file called forcefsck:

```
# touch /forcefsck
```

Now, reboot the system:

```
# reboot
```

Frce fsck on next boot using shutdown command (may not work on many modern distros)

The -F option force fsck on reboot, login as root and type the following command to reboot and run fsck:

```
# shutdown -rF now
```

[링크](https://www.cyberciti.biz/faq/linux-force-fsck-on-the-next-reboot-or-boot-sequence/)



#### Changing the check frequency

By default, fsck checks a filesystem every 30 boots (counted individually for each partition). To change the frequency of checking, run:

```sh
$ sudo tune2fs -c 20 /dev/sda1
```




### exFAT

리눅스테어 외부 USB 디스크를 *exFAT*로 포맷하고 사용한다면, *exfat-fuse*와 *exfat-utils*를 설치해 준다.

```sh
$ sudo apt install exfat-fuse exfat-utils
```

그리고 대부분 최신 리눅스 데스크탑은 USB 디스크를 더블클릭하면 자동마운트 해준다.

터미널에서는

```sh
$ sudo mkdir /media/my_usb
$ sudo mount -t exfat /dev/sdb1 /media/my_usb
$ sudo umount /dev/sdb1
```





### vnc server

https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-14-04

http://www.whatwant.com/840


$ sudo apt-get install gnome-panel tightvncserver


첫 실행을 해서 기본 Config 등의 구성을 하도록 하면 되는데, sudo 없이 계정 권한으로 실행하여도 된다. 계정 권한으로 실행을 하면 해당 계정으로 환경 설정을 한다.

```
$ vncserver
```


실행할 때에 해상도를 미리 정해줘야 한다.

 

$ vncserver -geometry 1024x768



기본 창 관리자 변경

 기본 생성된 xstartup 파일에는 내가 원하는 대로 환경 설정이 되어 있지 않다. 가장 먼저 실행된 vnc4server를 종료부터 하고 xstartup 파일을 수정하자.

```
$ vncserver -kill :1
$ cp ~/.vnc/xstartup ~/.vnc/xstartup.old

$ nano ~/.vnc/xstartup
```




### 참조

https://wiki.archlinux.org/index.php/fsck

