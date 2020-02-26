---
title: "Raspberry Pi : Raspbian 설치준비"
date: 2018-06-22 09:10:00 +0900
layout: post
tags: [Raspberry Pi, Raspberry Pi 3, Linux, Raspbian, 라즈베리파이, 라즈비안]
categories: [Raspberry Pi, Linux]
---

이 글은 Raspberry Pi Programming 과정을 진행하며 강의한 자료를 바탕으로 공개강의를 위한 텍스트로 *슬라이드쉐어*, *blog.thinkbee.kr* 그리고 실제 과정을 영상 *youtube* 채널에 공개할 목적으로 작성한다.

Install Rasbperry Pi 글타래는 아래 같이 구성되며 간단한 소개, Serial console, OS 설치,  Service 구성 및 사용에 대해 작성했다.

  1. **Raspberry Pi 설치준비**
  2. **[Install Raspbian OS]({% post_url /raspberrypi/2018-06-22-raspbian-2-os_install %})**
  3. Managing Service daemon
  4. Basic OS Security for Server
  5. [Install & Configuration - Nginx, Node JS, Jupyter]({% post_url /raspberrypi/2017-10-21-opensuse-jeos-nginxjupyter %})


> 2017-10-30: swap 추가, timezone 수정
{:.right-history}

## Raspbery Pi OS 설치준비

*Raspberry Pi(라즈베리파이)*에 *Serial Console* 을 연결하고, Raspbian, Ubuntu 등을 설치한 후에 라즈베리파이를 처음 설정하고 *SSH* 구성을 활성화 하겠다.

라즈베리파이에 OS를 설치하기 위해서는 **NOOBS**를 사용하거나 **오에스 이미지**를 다운받아 SD Card에 구워서 사용한다. 라즈베리파이는 다음과 같은 OS 배포본을 사용할 수 있다. 

|      운영체제       |                       특징                           |
|-------------------|----------------------------------------------------|
| Raspbian (Debian) | Raspberry Pi + Debian의 합성어, 가장 범용 OS.|
| Arch Linux (A non-Linux Distribution)| ARM 프로세서를 지원하는 Arch Linux |
| RISC OS | ARM 프로세서를 위한 실시가(RTOS) 운영체제 |
| OpenElec | XBMC 미디어 센터 지원을 위한 운영체제 |
| RaspBMC | XBMC 미디어 센터 지원을 위한 운영체제 |
| Pidora | Pi + Fedora 운영체제 |

**Raspbian(라즈비안)**은 라즈베리파이를 위한 데비안 리눅스로 가장 대중적인 데비안 리눅스, 우분투와 같은 사용자 경험을 제공해 주고 있다.



### Raspberry Pi 설치 준비

Raspberry Pi의 GPIO에 배치된 UART Rx, Tx 포트와 시리얼 케이블의 Tx, Rx로 크로스 연결하면 시리얼 콘솔로 작동한다.


설치에 앞서 준비해야 할 것이 있다. 

 - Raspberry Pi 1, 2, 3
 - SD Memory Card: 8GB 이상, Class6 이상
 - USB to Serial Cable

#### Raspberry Pi

Raspberry Pi의 GPIO에 배치된 UART Rx, Tx 포트와 시리얼 케이블의 Tx, Rx로 크로스 연결하면 시리얼 콘솔로 작동한다.

![](/images/raspberrypi/rapi_config-serial.png ){:with="500"}


#### SD Memory Card

라즈베리파이는 SD Meory Card 카드에서 부팅 및 운영 하도록 되어 있다. 
 - **NOOBS**를 이용한다면 최소 8GB
 - 다른 버전은 최소 4GB
 - Class 4는 4MB/s 쓰기 속도, Class 10은 10MB/s 쓰기가 가능하지만 Class 10이 Class 4를 앞선다는 것을 보장하진 않는다. 라즈베리파이는 Class 6의 8GB 사용을 권장하고 있다.

<figure>그림.</figure>

SDCardFormatter

[PCIe 지원 SD Card 규격 7.0](http://v.media.daum.net/v/20180630101105544)


#### USB to Serial Cable

USB 주변기기가 대중화 되면 컴퓨터에 Serial port가 사라진 후에 *USB to TTL* 혹은 *USB to Serial* 로 Serial 케이블을 사용해야 한다. 

구글 검색을 해보면 DB-9 포트 형태, 보드 형태 및 Raspberry Pi 같은 SBC 보드의 GPIO 포트에 사용하기 좋은 Adarfruit 판매하는 핀 형태의 제품도 있다.

![](/images/raspberrypi/usb-serial-cables.png){: width="600" }

<figure>그림.USB to Serial Cable(구글 이미지 검색 캡쳐)</figure>



