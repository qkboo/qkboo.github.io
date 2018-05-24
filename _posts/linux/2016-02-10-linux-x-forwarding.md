---
title: "Linux - X Forwarding"
date: 2016-02-10 09:10:00 +0900
layout: post
tags: ["linux", "debian", "ubuntu", "odroid", "armbian", "raspberry pi", "라즈베리파이", "orange pi", "오렌지파이", "banana pi", "바나나파이", "ssh", "X11"]
categories: ["Linux"]
---

SBC 보드 (raspberry pi, odroid c2 등)를 Terminal 기반으로 사용하려고 할 때 GUI에서 Programming을 확인해야 할 경우 X11, VNC 등을 이용할 수 있다. 여기서는 X Forwarding 기법을 정리하고 있다.

[^1]: Single Board Computer

## X11 Forwarding

**X11**은 유닉스/리눅스의 전통적 데스크탑 프로토콜로 GUI 데스크탑 환경을 X11 Protocol을 사용해서 로컬 혹은 원격지 컴퓨터에서 이용할 수 있게 설계되어 있다.

### X Windows: X ming

윈도우즈에서 [X ming](http://www.straightrunning.com/XmingNotes/pixming.php) 환경을 구축하면 X window system을 사용할 수 있다.


### X client: MobaXterm

윈도우 플랫폼은 다양한 X window 제품들이 있다. 최근 재미있게 사용해본 것으로 [MobaXterm](mobaxterm.mobatek.net)이 있는데, 이 제품은 유료 제품으로 Trial 을 제공하고 있다. 특징적으로  Xserver와 SSH client를 내장하고 있어서 손쉽게 리눅스 제품과 연결해 사용할 수 있다.

![MobaXterm 이미지](http://mobaxterm.mobatek.net/img/slider/RDP.png)
<figcaption>[그림. MobaXterm 이미지]</figcaption>

ssh를 이용해 X server에 접속하는 것 만으로도 X client 동작을 수행하고, 탭으로 구분한 ssh client 관리가 장점이며, sftp 를 이용해 서버측 파일을 브라우징 할 수 있는 장점이 있다.
 - 단, 기업용은 유료이므로 주의가 필요하다.


<br>
### macOS/Linux 데스크탑

X Window를 지원하는 리눅스 데스크탑 혹은 맥오에스에서 `ssh`로 X11 Forwarding 사용할 수 있다.

![SSH와 X11 Tunnel 구조](/images/linux/rpi-ssh-tunnel.png)
<figcaption>[그림. SSH와 X11 Tunnel 구조]</figcaption>

X forwarding을 활성화 해서 ssh를 연결하려면 `ssh -X` 옵션을 추가해서 접속한다. 

#### Mac에서 X11설정

**Mac OS X**는 OX X 10.5 이후 부터 X11을 포함하고 있지 않는다. Apple은 OS X의 X11을 더 개발하고 지원하기 위한 조직적인 노력으로 **XQuartz 프로젝트**를 만들었습니다. 
 - https://support.apple.com/ko-kr/HT201341
 - http://xquartz.macosforge.org
 
XQuartz 를 설치해서 지원을 받아야 한다. xquartz 사이트에서 다운받아 설치한다.

![macOS X: Download Xquartz](/images/linux/mac-xquartz-download.png)
<figcaption>[그림. macOS X: Download Xquartz]</figcaption>


설치후 맥에서 로그아웃후 로그인 하면 X11 관련 설정과 응용프로그램을 XQuartz 기반으로 사용할 수 있다.

#### X11 Forwarding 설정

맥에서는 `sshd` 설정이 */private/etc/ssh/sshd_config* 파일에 있다. 여기에 X11 client 요청을 허용하도록 *X11Forwarding*을 허용해야 한다.

```terminal
user@mac~$ sudo vi /private/etc/sshd_config
```

내용중에서 **X11Forwarding**를 찾아 다음 같이 허용으로 저장한다

```
X11Forwarding yes
```

마지막으로 Mac에서 외부에서 요청한 X11 연결을 허용해 준다.

```terminal
user@mac~$ xhost +
access control disabled, clients can connect from any host
```


#### X11 이용

이제 리눅스/맥 클라이언트에서 원격 서버에 -X 옵션을 이용해서 로그인 한다.

```terminal
user@mac~$ ssh -X pi@192.168.1.10
```


이제 원격지에서 X11 응용 프로그램을 실행해 보겠습니다. 다음은 X11 terminal 프로그램을 실행하면 Mac의 화면에 X11 terminal이 실행됩니다.

```terminal
pi@raspberrypi ~ $ netsurf-gtk
   OR
pi@raspberrypi ~ $ lxterminal
```

라즈베리파이의 xterminal 프로그램이 Mac 실행됩니다.

![Ssh와 X Forwarding](/images/linux/ssh-x11-pi-xterm.png)
<figcaption>[그림. Ssh와 X Forwarding]</figcaption>


만약 다음 에러를 만나면 접속을 종료하고 Mac의 xhost 접속 허용을 다시 해주어야 한다. 

> (lxterminal:20700): Gtk-WARNING **: cannot open display: localhost:10.0

혹은 원격 서버에 로그인 없이 직접 실행할 수 있습니다

```
user@mac~$ ssh -X -f pi@192.168.1.205 lxterminal
pi@192.168.1.205's password:
```


#### 기타 X11 App

xterminal 이외의 X11 앱을 사용하려면 다음 패키지를 설치해 준다.

```terminal
~ $ sudo apt install libnss3
~ $ sudo apt install x11-apps
```

