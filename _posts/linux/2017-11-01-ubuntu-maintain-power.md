## Ubuntu - 시스템 전원관리

우분투 시스템을 명령으로 잠자기, 깨우기가 가능하다. 컴퓨터의 BIOS에서 Wake On Lan이 활성화 되어야한다.

### WakeOnLan

wakeonlan을 활성화 하려면 이더넷 인터페이스를 화인한다.

```terminal
$ ifconfig
enp5s0    Link encap:Ethernet  HWaddr 0f:1a:92:51:70:a9
          inet addr:192.168.0.10  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe20::6595:e3fd:ad6:10f1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:85121 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11677 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:25916710 (25.9 MB)  TX bytes:1481803 (1.4 MB)
```

계속 ethtool 로 WakeOnLan이 활성화 되었는지 확인한다.

```terminal
$ sudo ethtool enp5s0
[sudo] password for USERNAME:
Settings for enp5s0:
  Supported ports: [ TP MII ]
  Supported link modes:   10baseT/Half 10baseT/Full
                          100baseT/Half 100baseT/Full
  Supported pause frame use: No
  Supports auto-negotiation: Yes
  Advertised link modes:  10baseT/Half 10baseT/Full
                          100baseT/Half 100baseT/Full
  Advertised pause frame use: Symmetric Receive-only
  Advertised auto-negotiation: Yes
  Link partner advertised link modes:  10baseT/Half 10baseT/Full
                                       100baseT/Half 100baseT/Full
  Link partner advertised pause frame use: Symmetric Receive-only
  Link partner advertised auto-negotiation: Yes
  Speed: 100Mb/s
  Duplex: Full
  Port: MII
  PHYAD: 0
  Transceiver: internal
  Auto-negotiation: on
  Supports Wake-on: pumbg
  Wake-on: g
  Current message level: 0x00000033 (51)
             drv probe ifdown ifup
  Link detected: yes
```

내용중에 *Supports Wake-on: <letters>* 항목이 WakeOnLan 지원을 확인할 수 있다. <letters> 문자에 **g** 가 있으면 *Magic Packet™*을  지원한다. 그러나 *d* 가 포함되지 않았으면 아래 명령으로 WoL을 활성화 해야 한다. 

```terminal
$ sudo ethtool -s enp5s0 wol g
```

이 명령은 대부분의 시스템에서 재시동이 필요하다.

시스템이 *ifupdown* 으로 구성되어 있으면 /etc/network/interfaces 에 아래 같이 

```terminal
auto lo
iface lo inet loopback
# The primary network interface

auto eth0
iface eth0 inet static
        address 10.0.0.1
        netmask 255.255.255.0
        gateway 10.0.0.138
        up ethtool -s eth0 wol g
```


https://askubuntu.com/questions/764158/how-to-enable-wake-on-lan-wol-in-ubuntu-16-04


### Power management

우분투 14 버전과 이후 버전의 시스템 전력 관리가 다르다.


#### Ubuntu 14.04 이전

```terminal
$ sudo apt install powermanagement-interface
```

pmi 명령 혹은 pm-*** 명령을 사용해서 컴퓨터를 재울 수 있다.

```terminal
pmi action suspend
pmi action hibernate
```

노트북에서 다음 명령도 실행된다.

```terminal
sudo pm-suspend
sudo pm-hibernate
```


#### Ubuntu 15 / 16 이상

대상 컴퓨터를 잠자기 모드 ( suspend)

```terminal
$ sudo systemctl suspend
```

최대절전모드인 Hibernation은 pm-hibernate 명령을 사용한다.

```terminal
$ sudo pm-hibernate
```

#### Wake-up 명령

네트워크에 연결된 장치를 깨우기 위해서 `wakeonlan` 명령을 사용할 수 있다.

> wakeonlan MAC_ADDRESS


```terminal
$ wakeonlan 00:1d:92:51:70:d8
Sending magic packet to 255.255.255.255:9 with 00:1d:92:51:70:d8
```


> mac에서는 **homebrew** 로 `wakeonlan` 명령을 설치해서 사용할 수 있다.


### 모니터 다루기

노트북 화면을 끄거나, 외부 모니터 표시를 잠시 멈출 수 있다.

#### console 에서

To turn off monitor in console, the command is the following:

```sh
sudo vbetool dpms off    # turn off
sudo vbetool dpms on     # turn on
```

To regain control of the console on pressing enter key, I suggest

sudo sh -c 'vbetool dpms off; read ans; vbetool dpms on'



#### X windows 상태에서

https://askubuntu.com/questions/253818/manually-turn-off-monitor

데스크탑 윈두우 터미널에서 `xrandr` 을 실행한다.

```sh
~$ xrandr
Screen 0: minimum 8 x 8, current 1280 x 800, maximum 32767 x 32767
LVDS1 connected primary 1280x800+0+0 (normal left inverted right x axis y axis) 331mm x 207mm
   1280x800       59.9*+
   1024x768       60.0
   800x600        60.3     56.2
   640x480        59.9
   640x400        60.0
VGA1 disconnected (normal left inverted right x axis y axis)
VIRTUAL1 disconnected (normal left inverted right x axis y axis)
```

in terminal(your laptop screen is something like LVDS1, and your external monitor is some thing like VGA).

##### turn off laptop screen

```sh
xrandr --output LVDS1 --off  # turn off
xrandr --output LVDS1 --on   # turn on
```


If you need to turn on the laptop screen::

노트북 화면 크기를 조정한다.

```sh
xrandr --output LVDS --mode 1280x800
```

외부 모니터를 끄려면 

```sh
xrandr --output VGA --off
```



## 참조

- [WakeOnLan](https://help.ubuntu.com/community/WakeOnLan)
