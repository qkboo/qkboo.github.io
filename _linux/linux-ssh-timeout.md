---
title: "Linux - sshd"
date: 2016-01-02 09:00:00 +0900
layout: single
tags: ["linux", "sshfs", "ssh"]
categories: [Linux, Programming]
toc: true
sidebar:
  nav: "linux"
---


## Ssh timeout

**ssh**를 사용시 접속 시간이 지나면 자동 끎김을 막아주는 옵션들이 있다. 

> 운영하는 서버는 보안상 `alive` 메시지를 모두 막아 두었다.
> 다만, **ssh** 접속시 `ServerAliveInterval` 을 사용해서 클라이언트가 `alive` 메시지를 서버에  있다.


### sshd 

**sshd** 데몬은 클라이언트 접속후 *sshd_config*에 구성한 설정데로 `alive` 메시지를 클라이언트에 주고 받아 접속 시간을 연장할 수 있다. 아래 그림 [^1]

![](https://sysadmincasts.com/static/extra/39-sshd-config-clientaliveinterval.gif) 


```sh
# alive 메시지 사용 결정
#TCPKeepAlive yes  # 기본 yes.

# 클라이언트가 살아있는지 확인하는 간격.
ClientAliveInterval 60  # 기본 0.
# 클라이언트에서 응답이 없을 때 메시지를 보내는 횟수
ClientAliveCountMax 3    # 확인 횟수

# Login Prompt에서 사용자 입력을 기다리는 시간을 초 단위로 입력.
LoginGraceTime 20  #( 1m: 기본 1분지정, 0은 시간제한없음)

```


### ssh 옵션

ssh 사용시 */etc/ssh/ssh_config* 구성 파일에 있는 `ServerAliveInterval` 옵션을 사용하면 **ssh** 접속시 `alive` 메시지를 서버가 클라이인트에게 주어진 시간 간격으로 보낸다.  

![](https://sysadmincasts.com/static/extra/39-ssh-config-serveraliveinterval.gif) [그림. ServerAliveInterval]


*ssh_config* 파일에 구성하거나 ssh 사용시 `-o ServerAliveInterval` 옵션을 사용하는 방법 두 가지가 있다.

#### ssh -o

ServerAliveInterval option every time you're connecting to a server by using the -o ServerAliveInterval=<time-in-second> prefix as the following example;

```sh
ssh -o ServerAliveInterval=300 user@example.com
```


## 참조
 - [sshd_config](http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man5/sshd_config.5)
 - [ssh_config](http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man5/ssh_config.5)

[^1]: [How to keep your ssh connection](https://sysadmincasts.com/episodes/39-cli-monday-how-to-keep-your-ssh-sessions-alive)