---
title: "Linux - Cloud Drives"
date: 2016-11-20 09:00:00 +0900
layout: single
tags: ["linux", "armbian", "debian", "google drive", "onedrive for linux", "Odroid", "Odroid c2", "raspberry pi"]
categories: ["Linux"]
toc: true
sidebar:
  nav: "linux"
---

리눅스 ( 아마 Armbian 서버에서 사용하려고 했었던 것 같다?!)에서 Cloud drive를 사용하고자 한다.

> Odroid C2 그리고 Orange-pi 시스템에서 사용할, 데스크탑 환경의 **Armbian Xenial**에서 사용하기 위해 클라우드 드라이브를 사용할 목적으로 사용했었다.

## Google Drive

Google drive는 `grive` 패키지로 제공되고 있다. 최근 (아마 2016년 이후) Google의 REST API가 바뀌어 `grive2` 패키지를 사용해야 한다. 패키지 제공이 되지 않으면 소스 빌드해서 사용했다.

### grive2 소스 기반 설치

Debian/Ubuntu/Linux Mint 에서 다음 라이브러리가 필요하다:

- yajl 2.x
- libcurl
- libstdc++
- libgcrypt
- Boost (Boost filesystem, program_options, regex, unit_test_framework and system are required)
- expat


다음 같이 `cmake` 와 필요한 라이브러리를 apt로 설치한다.

```terminal
sudo apt-get install git cmake build-essential libgcrypt11-dev libyajl-dev \
    libboost-all-dev libcurl4-openssl-dev libexpat1-dev libcppunit-dev binutils-dev pkg-config
```


### 빌드

소스 다운로드:

```terminal
git clone https://github.com/vitalif/grive2
cd grive2
```

CMake 로는 다음 같이 빌드 환경을 구성한다

```terminal
mkdir build
cd build
cmake ..
```

그리고 Make 로 다음 같이 빌드한다:

```terminal
make -j4
```

그리고 설치한다:

```terminal
sudo make install
```


### Updates

소스는 `git pull` 로 최신 소스를 얻고 다시 빌드한다:

```terminal
cd /path/to/yourGriveSourceCodeDir/grive2
git pull
cd build
cmake ..
make -j4
sudo make install
```


### Usage

인증

```terminal
$ grive -a
-----------------------
Please go to this URL and get an authentication code:

https://accounts.google.com/o/oauth2/auth?scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%....client_id=22314510474.apps.googleusercontent.com
-----------------------
Please input the authentication code here:
5/dTdVFy9xBd2cKLYvkcvJlYhwfht4IPuyJdri2Vv3sKA
Reading local directories
Reading remote server file list
```


#### 참조

 - [Grive2](http://yourcmc.ru/wiki/Grive2)



<br>
## OneDrive, for Linux

[OneDrive, for Linux](http://skilion.github.io/onedrive/)를 설치해서 사용한다.


### 설치

https://github.com/skilion/onedrive 와 같이 다운로드해서 설치하거나 apt로 unstable apt로 설치할 수 있다.

#### source 설치

git-hub 가이드에 따라,

```terminal
sudo apt-get install libcurl-dev
sudo apt-get install libsqlite3-dev
sudo wget http://master.dl.sourceforge.net/project/d-apt/files/d-apt.list -O /etc/apt/sources.list.d/d-apt.list
wget -qO - http://dlang.org/d-keyring.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install dmd-bin
```

그런데 Armbian 에서 *llibcurl-dev* 패키지를 요구해서,

```terminal
$ sudo apt-get install libcurl-dev
Reading state information... Done
Package libcurl-dev is a virtual package provided by:
  libcurl4-openssl-dev 7.38.0-4+deb8u5
  libcurl4-nss-dev 7.38.0-4+deb8u5
  libcurl4-gnutls-dev 7.38.0-4+deb8u5
You should explicitly select one to install.
```

그래서 libcurl-dev을 설치했다

```terminal
$ sudo apt-get install libcurl4-openssl-dev
$ sudo apt-get install libsqlite3-dev
```


#### 설정

```terminal
$ cat ~/.config/onedrive/config
sync_dir = "~/OneDrive"
skip_file = ".*|~*|thumbs.db|Games/*.iso"
skip_dir = ".*|Music|Movies/FullHD"
```


사용은

```
$ onedrive -h   
Usage: onedrive [OPTION]... 
no option   Sync and exit.
-m  --monitor   Keep monitoring for local and remote changes.   
--resync    Forget the last saved state, perform a full sync.   
-v  --verbose   Print more details, useful for debugging.   
-h  --help  This help information.
```

### 참조

 - [onedrive for Linux](http://skilion.github.io/onedrive/)