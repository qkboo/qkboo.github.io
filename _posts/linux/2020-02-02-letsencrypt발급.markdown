---
title: HTTPS를 위한 공인인증서 - Let's Encrypt 발급
date: 2020-02-02 18:00:00 +0900
layout: post
tags: [nginx, https, apache]
categories: Linux
---

> 2020-06-02: 매뉴얼 방식 수정<br>
> 2020-02-02: 최초 작성
> {:.right-history}

Nginx 서버에서 HTTPS 사용할 수 있는 공인인증서를 발급해 설치하려고 한다.

- 여기서는 Lets Encrypt 무료 공인인증서 발급을 다룬다.
- letsecrypt 공인인증서는 3개월 정도 기간만 사용 가능하고 갱신해야 한다.
- **_단독 도메인을 호스팅하는 개인 서버에서 Nginx_**에 적용해 본다.

인증서는 **개별 도메인** 혹은 **와일드카드 인증서** 로 도메인 안의 모든 호스트를 포함하는 두 종류로 발급이 가능하다.

# Let's Encrypt

Let's Encrypt 는 [https://letsencrypt.org](https://letsencrypt.org) 에서 여러 인터넷 관련 업체의 후원을 받아 운영하고 있다.

<img src='https://image.status.io/ZsUlrVAwD16p.png' width='200'>

## LetsEncrypt 발급

발급을 위해 리눅스/맥 쉘 기반의 Certbot 유틸리티를 사용하거나 웹 기반으로 가능하다. 여기서는 Certbot 를 사용해서 Let’s Encrypt 와일드 카드 인증서를 발급해 보겠다.

인증서를 발급 받기위해서는 Let’s Encrypt 인증서를 자동으로 생성하고 관리하는 패키지인 certbot 를 먼저 설치해야 한다.

### 우분투/데비안에서 설치

우분투/데이안 같은 리눅스 환경에서 설치하고 구성했다. 기타 다른 오에스 배포판은 아래 사이트에서 설치 명령어를 참고하자.

- https://certbot.eff.org/

Certbot 설치:

```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
```

certbot 관련 Apache, Nginx 용 파이썬 모듈을 설치한다.

Nginx 를 운영한다면

```
$ sudo apt-get install python-certbot3-nginx   #python3
$ sudo apt-get install python-certbot-nginx
```

Apache2 를 운영한다면,

```
$ sudo apt-get install python-certbot3-apache   #python3
$ sudo apt-get install python-certbot-apache
```

---

## 와일드카드 인증서 발급

와일드카드 인증서는 도메인의 호스트 단위에서 여러 서브 도메인까지 지원하는 인증서이다. 예를 들어 `example1.kr` 도메인의 하위 도메인 `abc.example1.kr`, `1234.example1.kr` 등에 모두 적용하고자 할 때 와일드 카드 인증서를 발급하는 것이다. 이 인증서를 발급 하기 위해서는 도메인 서버를 다루거나 호스팅 기관의 도메인 관리 도구를 통해서 \*DNS 레코드\* 에 부가적인 정보를 추가해야 한다.

발급 요청시 letsencrypt 는 세 가지 방법으로 webroot와 Standalone, DNS의 3가지 방식으로 적절한 요청을 확인하는 검증 절차를 통해 발급을 인증서를 인가한다.

- webroot : 도메인 호스트 서버에 임의 Url에 접근해 검증.
- standalon:
- DNS: 도메인의 DNS 서버에 TXT 필드에 임의의 값 통해 검증.

### webroot 방식

웹루트 방식으로 진행하기 전에 먼저 nginx 를 종료해 둔다.

```sh
# systemctl stop nginx
```

다음 certbot 명령에서 수동모드로 도메인 `example1.kr` 도메인의 와일드카드 인증서와 호스트 example1.kr 에 대해 명령을 실행한다.

```sh
# certbot certonly --manual -d "*.example1.kr" -d "example1.kr"
```

수동모드 진행을 시작하면 IP 로깅을 묻는다.

```sh
 - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
NOTE: The IP of this machine will be publicly logged as having requested this
certificate. If you're running certbot in manual mode on a machine that is not
your server, please ensure you're okay with that.

Are you OK with your IP being logged?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
```

이어서 다음 같이 웹 사이트 접근 URL을 통해서 인증하도록 요구한다.

```sh
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Create a file containing just this data:

AyTml9aa0IXw0RjNHERtktSxIxjFAkJvUOVis8dDRLQ.dSiF50fKKa4aji-OuIlgg62idngOi4nR6yzEHRaTLsI

And make it available on your web server at this URL:

http://example1.kr/.well-known/acme-challenge/kyTAl9aa0IXw0RjNHERtktSxIxjFAkJvUOVis8dDRLO

...
Enter

```

웹 서비스에 파일을 추가하고 내용에 데이터를 넣은후 ngixn 를 시작한다.

```sh
# systemctl start nginx
```

서버측에 검증을 위해 `http://example1.kr/.well-known/acme-challenge/kyTAl9aa0IXw0RjNHERtktSxIxjFAkJvUOVis8dDRLO` 경로에 텍스트 파일에 내용을 입력한다.

URL 준비가 되면 터미널에서 enter 를 입력하고 정상적으로 인증이 되면 아래 같이 결과를 출력해 준다.

```sh
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example1.kr/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example1.kr/privkey.pem
   Your cert will expire on 2020-09-01. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

정상적으로 진행되면 인증서와 개인키 파일이 저장된다:

- /etc/letsencrypt/live/example1.kr/fullchain.pem
- /etc/letsencrypt/live/example1.kr/privkey.pem

이 파일을 nginx 서버에 설정에 구성하면 된다.

---

### DNS 방식 와일드카드 인증서 요청

다음 certbot 명령은 도메인 `*.example1.com` 의 와일드카드 인증서를 요청한다.

```
# certbot certonly --manual --preferred-challenges dns -d "*.example1.com" -d "example1.com"

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): gangtai.goh@gmail.com

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Obtaining a new certificate
Performing the following challenges:
dns-01 challenge for example1.com

```

IP 로깅을 묻는다.

```
---

NOTE: The IP of this machine will be publicly logged as having requested this
certificate. If you're running certbot in manual mode on a machine that is not
your server, please ensure you're okay with that.

Are you OK with your IP being logged?

---

(Y)es/(N)o: y
```

IP정보 수집 이후에 DNS의 TXT 레코드에 입력할 코드가 발급된다. 이 코드를 도메인 관리 도구에서 주어지 호스트명에 TXT 레코드에 입력한다.

- 예) 아래는 `_acme-challenge.example1.kr` 라는 호스트

```
---

Please deploy a DNS TXT record under the name
\_acme-challenge.example1.kr with the following value:

aOVF_X388V8fYECoTJyArawJ95VpUffyUWqH8Q8bJAa

Before continuing, verify the record is deployed.

---

Press Enter to Continue
Waiting for verification...
Cleaning up challenges
```

**_DNS 관리하는 곳에서 서브 도메인 `_acme-challenge.example1.kr` 에 위에 출력된 해시코드를 TXT 레코드 추가해 준다._**

적용한 후 아래 같이 `dig` 같은 네임서버 명령으로 txt 레코드에 추가한 해시코드가 나오는지 확인하고 엔터로 진행한다.

```
$ dig txt _acme-challenge.example1.kr
...

;; ANSWER SECTION:
_acme-challenge.example1.kr. 180 IN	TXT	"aOVF_X388V8fYECoTJyArawJ95VpUffyUWqH8Q8bJAa"

```

그리고 발급된 도메인 인증서가 저장된 위치가 나타난다.

```
IMPORTANT NOTES:

- Congratulations! Your certificate and chain have been saved at:
  /etc/letsencrypt/live/example1.com/fullchain.pem
  Your key file has been saved at:
  /etc/letsencrypt/live/example1.com/privkey.pem
  Your cert will expire on 2020-05-23. To obtain a new or tweaked
  version of this certificate in the future, simply run certbot
  again. To non-interactively renew _all_ of your certificates, run
  "certbot renew"
- If you like Certbot, please consider supporting our work by:

  Donating to ISRG / Let's Encrypt: https://letsencrypt.org/donate
  Donating to EFF: https://eff.org/donate-le
```

정상적으로 진행되면 인증서와 개인키 파일이 저장된다:

- /etc/letsencrypt/live/example1.kr/fullchain.pem
- /etc/letsencrypt/live/example1.kr/privkey.pem

이 파일을 nginx 서버에 설정에 구성하면 된다.

---

### Let's Encrypt 자동갱신

Let’s Encrypt 인증서는 발급받으면 기간이 사용할 수 있는 기간이 3개월이다. 계속해서 HTTPS 서비스를 운영하려면 인증서가 만료되기 전에 갱신을 해야 한다.

`certbot` 명령으로 자동으로 갱신할 수 있는데 보통 크론에 등록해 두고 사용할 수 있다. root 권한이 필요하므로 root 계정의 cronttab에 아래와 같은 내용을 추가한다.

아래는 3개월 후 달의 1일에 명령을 실행을 등록했다.

```sh
0 0 1 1-12/3 * /bin/bash -l -c 'certbot renew --quiet'
```

혹은 매달 실행하려면 아래 같이 매달 1일 인증서 갱신 명령어를 실행하도록 해도 될 것 같다.

```sh
0 0 1 * * /bin/bash -l -c 'certbot renew --quiet'
```

---

## Nginx 설정

nginx에 와일드 카드 도메인을 사용할 여러 가상호스트가 구성되어 있다는 가정에서 시작한다.

### HTTPS 적용하기

와일드 카드

`nginx.conf` 파일에 SSL이 활성화 되었는지 확인한다.

```conf
##
# SSL Settings
##

ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
ssl_prefer_server_ciphers on;
```

실제 적용할 가상호스트 파일의 server 섹션에 다음 같이 인증서 파일을 활성화 한다. 가상호스트 파일이 여러개고 같은 서브 도메인이면 똑같이 적용해 주면 된다.

```conf
server {
    listen [::]:443 ssl http2 ipv6only=on;
    listen 443 ssl http2;

  	server_name www.exmaple1.com example1.com;

    ssl_certificate  /etc/letsencrypt/live/example1.kr/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/example1.kr/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;


```

### http 요청을 https에 리다이렉트

다음 구성은 `http://example1.com` 로 들어오는 모든 연결을 `https://example1.com` 으로 전환하는 구성이다.

- VirtualHost 별로 아래와 같이 Port 80 에 대한 Server설정과 443에 대한 Server설정을 분리
- Port 80 에 대해서는 301 Redirect 설정
- Nignx 재시작

```conf
server {
    listen         80;
    server_name    example1.com example2.com;
    return         301 https://$host$request_uri;
}

server {
    listen [::]:443 ssl http2 ipv6only=on;
    listen 443 ssl http2;
    server_name    example1.com example2.com;
     ...
}
```

## 참고

1. [Certbot: Debian Instruction](https://certbot.eff.org/lets-encrypt/debianbuster-nginx)
1. [Getting Started 문서](https://letsencrypt.org/ko/getting-started/)
1. [Let's Encrypt Wildcard-certificate-with-certbot](https://dev.to/nabbisen/let-s-encrypt-wildcard-certificate-with-certbot-plo)
1. [Wildcard-domain step-by-step](https://community.letsencrypt.org/t/wildcard-domain-step-by-step/58250/4)
1. [nginx ssl 설정](https://www.securesign.kr/guides/NGINX-SSL-Certificate-Install)
1. [Let's Encrypt 인증서 발급 - Anapche/Nginx](https://jootc.com/p/201901062488)
