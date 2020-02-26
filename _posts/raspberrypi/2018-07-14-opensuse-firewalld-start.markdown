---
title: "Getting Started `firewalld`"
date: 2018-07-14 01:00:00 +0900
layout: post
tags: [Raspberry Pi 3, Raspbian, openSUSE, Armbian, Ubuntu, Linux, firewall, firewalld, 리눅스, 우분투, 라즈베리파이, 라즈비안, 방화벽, 파이어월]
categories: [Raspberry Pi, Linux]
---

RedHat, CentOS, Fedora 배포본 등에서 표준 방화벽 인터페이스로 제공되는 최신 **FirewallD** 사용을 시작해 보자. `firewalld` 패키지 설치는 각 배포본의 방법으로 설치하면 된다.

여기서는 OpenSUSE, Armbian 배포본을 설치한 시스템에서 `firewalld` 방화벽을 구성하고 설정하는 과정을 요약 정리했다.

## Getting Started - FirewallD

sudo systemctl enable firewalld
sudo reboot

We can verify that the service is running and reachable by typing:

sudo firewall-cmd --state


firewalld는 Zone 을 기반으로 서비스, 포트, IP 주소 등을 다양한 규칙으로 혼합해 사용할 수 있다.

기본으로 지원하는 zone은 다음 같다.

```terminal
$ sudo firewall-cmd --get-zones
block dmz drop external home internal public trusted work
```

그리고 permanent로 활성화된 Zone은

```terminal
$ sudo firewall-cmd --get-active-zones
drop
  sources: ipset:blacklist
trusted
  sources: 192.168.0.0/24 220.121.170.0/24
```


### Public Zone에 Http, Ssh 서비스

http, https 를 공개 서비스를 지원하는 기본 public zone에 추가한다.

```terminal
$ sudo firewall-cmd --add-service=ssh
$ sudo firewall-cmd --add-service=http
$ sudo firewall-cmd --add-service=https
```


혹은 zone을 지정해서 서비스를 추가할 수 있다.

```terminal
$ sudo firewall-cmd --zone=public --add-service=ssh
$ sudo firewall-cmd --zone=public --add-service=http
$ sudo firewall-cmd --zone=public --add-service=https
```


```terminal
$ sudo firewall-cmd --zone=public --remove-service=ssh
```

public zone에 대한 정보를 출력한다.

```terminal
$ sudo firewall-cmd --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: ssh dhcpv6-client http https
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```


### Trusted Zone 관리


```terminal
$ sudo firewall-cmd --zone=trusted --add-service=ssh 
```


trusted zone의 IP 범위를 소스로 지정할 수 있다.

```terminal
$ sudo firewall-cmd --zone=trusted --add-service=dns
$ sudo firewall-cmd --zone=trusted --add-source=192.168.1.0/24
```


```terminal
firewall-cmd --zone=zone-name --remove-source=<source>
```


Trusted Zone의 정보를 출력한다.

```terminal
$ sudo firewall-cmd --zone=trusted --list-all
```


### Port

포트에 따라 는 프로토콜을 지정해 줄 수 있다. 지정되지 않으면 기본 TCP 포트가 구성된다.

```
firewall-cmd --remove-port=port-number/port-type
```

대체 http 포트인 8080을 추가해 보자.

```terminal
sudo firewall-cmd --zone=public --add-port=8080/tcp
```

mongodb 같은 내부 데이터베이스 포트를 *trusted* zone에 추가해 준다.

```terminal
sudo firewall-cmd --zone=trusted --add-port=27017/tcp
```

포트 범위를 지정해 구성할 수 있다.

```terminal
sudo firewall-cmd --zone=public --add-port=4990-4999/udp
```

포트를 제거할 수 있다.

```terminal
sudo firewall-cmd --zone=zone-name --remove-source-port=<port-name>/<tcp|udp|sctp|dccp>
```


Zone에 구성된 포트 정보를 출력할 수 있다.

```terminal
sudo firewall-cmd --zone=public --list-ports
sudo firewall-cmd --zone=trusted --list-ports
```

여기까지 `firewall-cmd` 로 설정한 내용은 실행중(런타임) 방화벽으로 구성되어 사용된다. 재시동 등의 이벤트가 발생하면 내용이 사라지게 된다. 영구적으로 보존하기 위해서는 `--permanent` 옵션을 사용해야 한다.

### Permenent

Zone 에 구성한 서비스, 포트, 소스, 프로토콜 등에 대한 설정을 영구적으로 보존해려면 `--permanent` 옵션을 사용해야 한다. 영구적인 구성을 설정을 보존하려면 아래 두가지 방법을 사용한다.

먼저 `firewall-cmd` 로 실행중 제한 내용을 구성하고, `--runtime-to-permanent` 를 실행해 준다.

```
firewall-cmd --runtime-to-permanent
```

다른 방법을 `firewall-cmd` 에 `--permanent` 옵션을 주고 실행하면 영구 보존되어 저장된다.

```terminal
firewall-cmd --permanent <other options>
```


### reload

방화벽을 갱신한다

```terminal
firewall-cmd --reload
firewall-cmd --state
firewall-cmd --list-all                #public
firewall-cmd --permanent --list-all    #permanent
```


<br>


## 참조


[Firewalld configuration and usage](https://www.hogarthuk.com/?q=node/9)

- [RedHat: Getting started with firewalld](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-using_firewalls#sec-Getting_started_with_firewalld)

- [How to set up firewalld on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)

