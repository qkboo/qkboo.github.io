---
title: "Raspberry Pi : Upgrade OpenSUSE LEAP 42.2 to 42.3"
date: 2018-07-10 09:00:00 +0900
layout: post
tags: [Raspberry Pi, Raspberry Pi 3, Linux, 리눅스, Raspbian, 라즈베리파이, 라즈비안, openSUSE, LEAP 15.0, LEAP 42.3]
categories: [Raspberry Pi, Linux]
---

Raspberry Pi 3 에 설치해 사용중이던 openSUSE LEAP 42.2의 지원이 종료되어, 2019년가지 지원하는 LEAP 42.3으로 업그레이드하는 과정을 정리했다. 

> openSUSE는 LEAP 15.0으로 최신 버전으로 배포하고 있다. 42.2에서 15.0으로 바로 업그레이드시 내가 해결 못하는 문제가 생겨서 42.3으로 업그레이드 했다.

## Upgrade OpenSUSE LEAP 42.2 to 42.3

OpenSUSE LEAP 42.2 는 지원이 종료되어 42.3으로 업그레이드를 한다.

![](/images/opensuse/opensuse-discontinued-201807.png){: width="600"}

<figcaption>[그림. 지원 중단 배포본 (OpenSUSE.com)]</figcaption>
<br>
업그레이드에 대한 과정은 [OpenSUSE System Upgrade](https://en.opensuse.org/SDB:System_upgrade) 에 나와있는데 42.3을 15.0으로 배포본 업그레이드 하는 과정과 일반 배포본에서 배포본 업그레이드가 설명되어 있어서 직접 시도하면서 정리했다.


### Upgrade 시작

먼저 42.2 저장소 URI를 42.3 으로 변경한다.

#### 42.3 Repository Uri

현재 시스템의 Repository Uri를 확인해 보고

```terminal
$sudo zypper repos --uri

Repository priorities are without effect. All enabled repositories share the same priority.

# | Alias                             | Name                              | Enabled | GPG Check | Refresh | URI
--+-----------------------------------+-----------------------------------+---------+-----------+---------+----------------------------------------------------------------------------
1 | openSUSE-Ports-Leap-42.2-Update   | openSUSE-Ports-Leap-42.2-Update   | Yes     | (r ) Yes  | Yes     | http://download.opensuse.org/ports/update/42.2/
2 | openSUSE-Ports-Leap-42.2-repo-oss | openSUSE-Ports-Leap-42.2-repo-oss | Yes     | (r ) Yes  | Yes     | http://download.opensuse.org/ports/aarch64/distribution/leap/42.2/repo/oss/
```

다른 저장소를 추가하지 않고 사용했다면 LEAP 42.2 배포본 저장소는 위와 같을 것이다. 현재 Uri인 저장소 백업하고,

```terminal
$sudo cp -Rv /etc/zypp/repos.d /etc/zypp/repos.d.Old
```


위 목록에서 *openSUSE-Ports-Leap-42.2-repo-oss*를 제거한다. 목록 번호로 제거할 수 있어서 `rr 2` 명령으로 제거한다.

```terminal
$sudo zypper rr 2
```


저장소 문자열 42.2 를 42.3 문자열로 치환한다

```terminal
$sudo sed -i s/42.2/42.3/ /etc/zypp/repos.d/*
```

여기에 앞에서 제거한 repo-oss 저장소를 LEAP 42.3을 위한 **openSUSE-Leap-42.3-repo-oss** 를 추가해 준다.

```terminal
$sudo zypper addrepo --check --refresh --name 'openSUSE-Leap-42.3-repo-oss' http://download.opensuse.org/ports/aarch64/distribution/leap/42.3/repo/oss/ repo-oss
```

새 저장소 Uri를 추가하고 배포본의 Repository Url를 출력해서 Enabled 컬럼이 활성화 되어 있는지 확인한다.

```terminal
$sudo zypper repos --uri
# | Alias       | Name                        | Enabled | GPG Check | Refresh | URI
--+-------------+-----------------------------+---------+-----------+---------+----------------------------------------------------------------------------
1 | repo-oss    | openSUSE-Leap-42.3-repo-oss | Yes     | (r ) Yes  | Yes     | http://download.opensuse.org/ports/aarch64/distribution/leap/42.3/repo/oss/
2 | repo-update | openSUSE-Leap-42.3-Update   | Yes     | (r ) Yes  | Yes     | http://download.opensuse.org/update/leap/42.3/oss/
```

만약 출력한 저장소 목록에서 Enabled 컬럼이 No 라면 다음 명령으로 활성화 한다.

```terminal
$sudo zypper modifyrepo --enable openSUSE-Leap-42.3-repo-oss
```

이제 42.3을 위한 저장소 Uri가 정리되었다.

실제 배포본 버전을 업그레이드 하기 위해서는 `dup` 명령을 내려 실행해야 하지만 역시 42.2를 15.0으로 직접 배포본 업그레이드 했을 때 문제가 발생해서 설치된 패키지를 업르레이드하고 배포본 업그레이드를 진행했다.

####  Distribution Upgrade

먼저 저장소에서 패키지 목록을 최신으로 갱신하고, package upgrade를 진행해 준다.

```terminal
zypper ref
zypper up
```

오에스에서 사용하는 패키지가 많기 때문에 패키지 업그레이드 과정은 꽤 긴 시간이 필요하다. 

과정중에 펌웨어 버전 충돌이 나오면 Yes 해준다.

```terminal
Checking for file conflicts: ............................................[error]
Detected 1 file conflict:

File /lib/firmware/brcm/brcmfmac43430-sdio.bin
  from install of
     kernel-firmware-20170530-20.1.noarch (openSUSE-Leap-42.3-Update)
  conflicts with file from package
     bcm43xx-firmware-20160301-2.1.noarch (@System)

File conflicts happen when two packages attempt to install files with the same name but different contents. If you continue, conflicting files will be replaced losing the previous content.
Continue? [yes/no] (no): yes
```

그리고 재시동을 해준다.

package upgrade를 완료한 후에 Distribution upgrade를 수행하자

```terminal
zypper dup
```



#### 재시동

업그레이드를 설치한 후에 재시동 하면 약 5분 정도 펌웨어 등을 설치하는 과정을 거친다. 로그인 프롬프트에서 *openSUSE Leap 42.3* 이 보이면 성공한 것이다.


```terminal
Welcome to openSUSE Leap 42.3 - Kernel 4.4.104-18.44-default (ttyS0).


homepi64 login:
```


#### 보안 패치

openSUSE는 upgrade와 patch 를 분리해서 제공한다. 패치는 보안 사항에 관련한 것을 제공하고 있다. `lp` 로 다운로드 가능한 패치를 확인할 수 있다.

```
# zypper lp
Retrieving repository 'openSUSE-Leap-42.3-Update' metadata ..................[done]
Building repository 'openSUSE-Leap-42.3-Update' cache .......................[done]
Loading repository data...
Reading installed packages...

Repository                | Name               | Category    | Severity  | Interactive | Status | Summary
--------------------------+--------------------+-------------+-----------+-------------+--------+-----------------------------------------------------------------------
openSUSE-Leap-42.3-Update | openSUSE-2017-1268 | security    | important | ---         | needed | Security update for webkit2gtk3
openSUSE-Leap-42.3-Update | openSUSE-2017-1425 | recommended | low       | ---         | needed | Recommended update for grub2
openSUSE-Leap-42.3-Update | openSUSE-2017-940  | security    | important | ---         | needed | Security update for subversion
openSUSE-Leap-42.3-Update | openSUSE-2018-118  | security    | important | ---         | needed | Security update for webkit2gtk3
openSUSE-Leap-42.3-Update | openSUSE-2018-234  | recommended | low       | ---         | needed | Recommended update for grub2
openSUSE-Leap-42.3-Update | openSUSE-2018-241  | recommended | moderate  | ---         | needed | Recommended update for yast2, yast2-nfs-client, yast2-services-manager
openSUSE-Leap-42.3-Update | openSUSE-2018-586  | recommended | low       | ---         | needed | Recommended update for grub2

Found 7 applicable patches:
7 patches needed (3 security patches)
```

패치는 보안 이슈에 대한 것으로 해당 보안 이슈 정보 혹은 분류 등을 확인할 수 있다. 목록에서 Name 컬럼이 발생한 보안 사항 번호로 이 번호로 조회할 수 있다.

```terminal
zypper info openSUSE-2017-462
```

패키지 내용은 CVE(Common Vulnerabilities and Exposures)로 보고된 보안 사항을 적용한 내용을 자세히 보여주고 있다.

패치 중에서 Category로 검색할 수 있다. 다음 같이 security 분류만 검색할 수 있다.

```terminal
zypper lp --category security
Loading repository data...
Reading installed packages...

Repository                | Name               | Category | Severity  | Interactive | Status | Summary
--------------------------+--------------------+----------+-----------+-------------+--------+--------------------------------
openSUSE-Leap-42.3-Update | openSUSE-2017-1268 | security | important | ---         | needed | Security update for webkit2gtk3
openSUSE-Leap-42.3-Update | openSUSE-2017-940  | security | important | ---         | needed | Security update for subversion
openSUSE-Leap-42.3-Update | openSUSE-2018-118  | security | important | ---         | needed | Security update for webkit2gtk3

Considering 3 out of 7 applicable patches:
3 patches needed (3 security patches)
```

패치를 전체를 설치하려면

```terminal
sudo zypper patch
```

혹은 패치 중에서 특정 패치만 설치할 수 있다.

```terminal
sudo zypper install patch:openSUSE-2017-1268
```


## 참조

 - [OpenSUSE System Upgrade](https://en.opensuse.org/SDB:System_upgrade)
 - [openSUSE Update & Patch](https://lukerawlins.com/opensuse-patch-vs-update/)

