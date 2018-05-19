---
title: Linux - ssh-sshfs
date: 2016-01-05 09:00:00 +0900
layout: single
tags: ["linux", "sshfs", "ssh"]
categories: [Linux, Programming]
toc: true
sidebar:
  nav: "linux"
---

**ssh** 사용 팁과 **sshfs** 이용 방법에 대해서 정리한다.

## ssh

터미널에서 ssh를 사용하는데 이용하는 구성과 설정을 정리했다.

### 비밀키 이용

ssh를 사용하는 클라이언트에서 `ssh-keygen` 으로 *비밀키*와 *공개키*를 생성하고, 접속하는 서버 계정 밑에 클라이언트 공개키를 저장하면 ssh 접속시 비밀번호 응답 없이 처리되어 로그인 할 수 있다.

#### 1. ssh 클라이언트

클라이언트에서 개인 비밀키를 생성한다. `ssh-keygen` 명령은 기본적으로 비밀키와 공개키 파일을 사용자 홈디렉토리 *~/.ssh* 폴더에, 기본 파일이름 *id_rsa.pub*, *id_rsa.prb* 파일로 저장한다.

```sh
(CLIENT)$ ssh-keygen -t rsa -b 4096 -C "USER@localhost"
```

#### 2. ssh 서버

서버에도 클라이언트와 동일하게 `ssh-keygen` 명령으로 비밀키와 공개키를 생성한다.

```sh
(SERVER)$ ssh-keygen -t rsa -b 4096 -C "USER@server"
```


#### 서버에 공개키 배포

클라이언트에 생성한 공개키 *id_rsa.pub* 파일을 업로드해서 *./ssh/authorized_keys* 파일에 추가해야 한다. 보통 `scp` 명령으로 복사해서 *authorized_keys* 파일에 더해주면 된다. 

일반적으로 `scp` 명령으로 복사하고, 서버에 `ssh` 접속해서 업로드한 공개키 파일을 *authorized_keys* 파일에 더해준다.

*1. 클라이언트에서 복사하기:*

```sh
scp ~/.ssh/id_rsa.pub USER_ID@HOST_NAME:~/client.pub
```


ssh로 서버에 로그인한다.

*2.서버 authorized_keys 붙여넣기:*

```sh
ssh userid@SERVER
(SERVER) $ cat client.pub >> .ssh/authorized_keys; rm client.pub
```


위 2 과정을 아래 명령 한 줄로 복사->붙여넣기를 동시에 할 수 있다.

*클라이언트:*

```sh
cat ~/.ssh/id_rsa.pub | ssh <USERNAME>@<IP-ADDRESS> 'cat >> .ssh/authorized_keys'
```


이제 해당 서버로 로그인해 본다.


### ssh config 사용하기

사용자를 위한 ssh 구성을 하려면 `~.ssh/config` 설정 파일을 이용한다.

#### keep alive session

ssh 접속시 옵션을 주어 세션 유지 시간을 지정할 수 있다.

#### ServerAliveInterval 사용

접속시 `ServerAliveInterval=TICK`를 사용하면 TICK초 마다 한번씩 ServerAliveInterval를 보낸다.

옵션을 직접 사용하거나 ~/.ssh/config 설정 파일에 지정해 둘 수 있다.

ssh 접속시 `-o` 옵션으로 지정한다.

```
ssh -o ServerAliveInterval=10 192.168.0.1
```


#### **~/.ssh/config** 이용

사용자의 ssh 설정 파일은 ~/.ssh/config 이다.

```
# For all hosts
ServerAliveInterval 20
# For a selection of hosts
Host 192.168.0.1 192.168.1.1
  ServerAliveInterval 20
```

시스템 전체에 적용한다면 **/etc/ssh_config** 에 (혹은 데비안 계열은 /etc/ssh/ssh_config) 지정해도 된다.

<br>
## sshfs

원격 호스트에서 작업중인 소스등을 편집하는데 터미널로 접속해 vim, nano 같은 편집 도구를 이용할 수 있지만, 개발 컴퓨터에서 손에 익은 GUI 개발 도구를에서 개발하고 편집해서, 원격 호스트에서 실행하는 방법을 선호해서 *sshfs*를 이용하고 있다. 

보통 Sublime Text, TextMate 등의 에디터에서 파이썬 등의 프로그래밍 코드를을 작성하고 sshfs를 이용해 원격 디렉토리에 저장하는 방법을 사한다.


### 설치

#### Ubuntu/Debian

```bash
$sudo apt install sshfs
```


#### Mac OS X

Mac OS X Fuse 설치

- http://osxfuse.github.io

sshfs 설치후 재시동 필요.


#### Window
다음 설치 파일을 받아 설치한다.
https://win-sshfs.googlecode.com/files/win-sshfs-0.0.1.5-setup.exe


### Mac OS X 사용
Mac OS X에서는 OSXFuse를 사용해서 사용자 계정에서 sshfs를 이용한다. 그래서 sudo 명령을 사용하지 않는다.

```bash
$ sshfs USER_ID@xxx.xxx.xxx.xxx:/ /Volume/remote
```


sudo 명령을 이용해서 마운트할 경우 마운트 포인트를 찾지 못해서 다음 같은 에러가 난다.
```
$ ls
ls: odoomodules: No such file or directory
```


#### Ubunto/Debian

```bash
sudo sshfs USER_ID@xxx.xxx.xxx.xxx:/ /Volume/remote
```

사용후 언마운트는 다음과 같다.
```bash
$sudo umount /Volume/remote
```




#### 마운트 고정: Mac OS X

https://amaral.northwestern.edu/resources/guides/mounting-remote-folder-os-x-over-ssh


파일 /etc/fstab
```bash
sshfs#USER_ID@xxx.xxx.xxx.xxx:/ /Volume/remote
```


##### DS_Store 파일
OS X는 파일을 다룰 때 .DS_Store 파일로 폴더를 지저분하게 한다. 이것을 비활성화 할 수 있다.
마운트할 때 **noappledouble** 옵션을 사용한다.

```bash
$ mkdir ~/example
$ sshfs user@host:/example ~/example -oauto_cache,reconnect,defer_permissions,negative_vncache,noappledouble,volname=Example
```

> [Mounting an OSX SSH Volume using FUSE and SSHFS](http://eddmann.com/posts/mounting-an-osx-ssh-volume-using-fuse-and-sshfs/)


#### ssh authentication

```bsh
sudo sshfs -o IdentityFile=~/.ssh/id_rsa USER_ID@xxx.xxx.xxx.xxx:/ /Volume/remote
```


#### 자주사용하는 sshfs 명령

```bash
$ mkdir ~/example
$ sshfs user@host:/example ~/example -oauto_cache,reconnect,defer_permissions,negative_vncache,noappledouble,volname=Example
```




### sshfs options

sshfs 구현마다 조금 다르지만 https://linux.die.net/man/1/sshfs 에서 옵션 내용을 조금 살펴보자:

- `-o reconnect` : reconnect to server
- `-o delay_connect` : delay connection to server
- `o sshfs_sync`: synchronous writes
- `-o no_readahead`: synchronous reads (no speculative readahead)
- `-o sshfs_debug`:  print some debugging information
- `-o cache=BOOL`: enable caching {yes,no} (default: yes)
- `-o cache_timeout=N`: sets timeout for caches in seconds (default: 20)
- `-o cache_X_timeout=N`: sets timeout for {stat,dir,link} cache
- `-o workaround=LIST`: colon separated list of workarounds
- `none`: no workarounds enabled
- `all`: all workarounds enabled


#### cache

느린 네트워크에서는 캐시를 끄고 사용하는게 좋겠다.

- `-o cache=YESNO`: enable caching {yes,no} (default: yes)
- `-o cache_timeout=N` : sets timeout for caches in seconds (default: 20)
- `-o cache_X_timeout=N` : sets timeout for {stat,dir,link} cache


#### 보증된 네트워크에서 암호화 없이 mount

> sshd 가 암호화를 지원하는 상황에서 안된다.

안전한 네트워크에서는 `Ciphers, Compression` 옵션을 사용 [^Blazingly fast sshfs] 하면 빠른 속도를 얻을 수 있다.

```sh
sshfs -o Ciphers=arcfour -o Compression=no server://some/folder /mnt/some_local_folder
```

 - `Ciphers=arcfour` : 빠른 암호화 메서드, 다만 안전하지 않다.
 - `Compression` : ssh 내장 압축 사용하지 않는다.

rsync 에도 사용할 수 있다.

```sh
rsync -e"ssh -c arcfour -o Compression=no" ...rest of rsync cmd...
```



### TCP Optimization


#### MTU(Maximum Transmission Unit)

네트워크 인터페이스에서 세그먼트 없이 보낼수 있는 최대 데이터그램 크기 값입니다. 만약 데이터가 MTU 값 이상이라면 여러개의 패킷으로 분할이 될 것입니다. 간단하게 보자면 MTU 는 패킷이 한번에 보낼 수 있는 최대 크기라고 볼 수 있습니다.

이더넷의 MTU 값은 일반적으로 1500 바이트이며 옛날에 모뎀을 통해 접속하던 PPPoE 연결은 1492 바이트를 가지고 있습니다.

MTU 는 각 패킷 프레임안에 최대 전송할 수 있는 값 MSS(Maximum segment size) 가 정의되어 있습니다. 그렇다면 MTU는 MSS + TCP/IP 헤더 크기가 될 것이고 반대로 MSS 는 MTU - 40  바이트가 됩니다. 40 바이트는 IP 와 TCP 헤더 20 바이트씩을 뜻합니다.


##### Linux

리눅스에서 MTU 값은 ifconfig 명령으로 확인할 수 있다.

```
$ ifconfig eth0
eth0      Link encap:Ethernet  HWaddr b8:27:eb:c8:5f:4b
          inet addr:220.121.140.239  Bcast:220.121.140.255  Mask:255.255.255.0
          inet6 addr: fe80::ba27:ebff:fec8:5f4b/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1633606 errors:0 dropped:44758 overruns:0 frame:0
          TX packets:73808 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:85875027 (81.8 MiB)  TX bytes:22615535 (21.5 MiB)
```

기본 MTU가 1500인데 이 값을 조정하려면 sudo ifconfig 명령으로 할 수 있다.

```
$ sudo ifconfig eth0 mtu 9000
```

재시동 후에도 지속적으로 MTU 값을 유지하고 싶으면 /etc/network/interfaces 에 명시하면 된다.

```
auto eth0
iface eth0 inet static
    address 192.168.0.2
    netmask 255.255.255.0
    mtu 9000
```


##### 링크

[MTU](http://www.packetinside.com/2013/02/mtumaximum-transmission-unit.html)


#### SSHFS-MUX

http://www.linux-magazine.com/Issues/2014/165/SSHFS-MUX


### Error

에러 ```mount_osxfuse: the file system is not available (255)``` 

There appears to be a problem loading the KEXT installed by the regular osxfuse Homebrew package. You can use brew cask to install the official FUSE for OS X build from their own DMG:

```
brew rm osxfuse
brew install caskroom/cask/brew-cask
brew cask install osxfuse
```


## 참조
 - [SSHFS – Installation and Performance](http://www.admin-magazine.com/HPC/Articles/Sharing-Data-with-SSHFS)
 - [SSHFS-MUX](http://www.linux-magazine.com/Issues/2014/165/SSHFS-MUX)
 - [Protocol Not Supported Error](http://goo.gl/xp6LuS)
 - [Mac OS X sshfs](https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh)
 - [Mounting an OSX SSH Volume using FUSE and SSHFS](http://eddmann.com/posts/mounting-an-osx-ssh-volume-using-fuse-and-sshfs/)



[^Blazingly fast sshfs]: https://www.smork.info/blog/2013/04/24/entry130424-163842.html