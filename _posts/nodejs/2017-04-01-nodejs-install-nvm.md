---
title: NodeJS / nvm 기반 개발환경 설치
date: 2017-04-01 09:00:00 +0900
layout: post
tags: [nodejs tutorials, nvm, nodejs]
categories: [Linux, NodeJS]
---

Node.js를 설치하고 관리할 수 있는 Node Version Manager를 사용한 개발환경 구성에 대해 살펴본다.



## 버전관리자를 통한 Node.js 개발환경

Node.js는 커뮤니티 개발을 위주로 업그레이드가 자주 된다. 개발중인 관련 모듈이 업그레이드를 따라가지 못할 경우가 자주 발생할 수 있다. 그래서 실제 개발하는 경우에 Node.js 버전의 변경을 자유롭게 하기 위해서 버전관리자(Node Version Manager) 환경에서 개발을 권장한다. 

주요한 버전관리자에는 Nvm, Nodist 등 여러 종류가 있는데, 대부분 리눅스와 맥에서 사용 가능하다. 그리고 윈도우 환경에서는 nvm-windows, nodist를 사용할 수 있다.

 - Linux/macOS: https://github.com/creationix/nvm
 - Windows: https://github.com/marcelklehr/nodist

버전관리자를 설치후 사용 방법은 대동소이 하다. 여기서는 **nvm**을 다룬다.

Shell 은 쉘 프롬프트로 사용자 권한 및 현재 위치를 표시하는데 보통 권한별로 아래 기호를 사용한다.<br/>

- `$` : 사용자 프롬프트
- `#` : root 프롬프트
- `~` : 사용자 홈 디렉토리
- `/` : 루트 디렉토리
- `.` : 현재 디렉토리
- `..` : 이전 디렉토리


### nvm 설치

Linux / MacOS는 다음 쉘 스크립을 실행해 설치한다.

curl 이용:

```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
```

wget 이용:

```sh
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
```

*install.sh* 스크립이 설치하는 nvm은 사용자 홈디렉토리 `~/.nvm` 에 설치된다. (이후 *$NVM_HOME*이라 하겠다)

> *$NVM_HOME/install.sh* 를 실행하면 업그레이드가 진행된다.

설치후 *~/.bash_profile*, *~/.profile* 등의 프로파일에 `nvm.sh` 가 실행되도록 아래 스크립이 추가된다.

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```


#### Node.js 설치

원격 저장소의 node 목록

```sh
nvm ls-remote
nvm ls-remote v6        #v6.x 버전만 검색
```

`nvm install` 은 해당 node version를 다운로드하고 설치한다.

```sh
nvm install v5          # node version v5.x 버전중 최종버전
```

`nvm ls` 설치된 node 버전을 확인한다

```sh
$ nvm ls
   v0.9.12
   v0.11.0
   v6.9.1
current:    v6.9.1
```

`nvm use`로 사용할 node version을 지정한다 - 이것은 사용자의 `$PATH` 환경변수에 `node` 경로를 추가해준다.

```sh
nvm use 6.9.1       #v6 버전을 사용한다.
nvm use v7          #v7 버전중 최종 버번을 선택한다.
```

`node` 경로를 제거하려면,

```sh
nvm deactivate
```


로그인후 기본 node 환경으로 지정하려면

```sh
nvm alias default 6.9.1
```



#### npm

`node`를 설치후 `npm`을 업그레이드 해준다. npm 자체는 다음 같이 업그레이드 한다.

```sh
npm i -g npm
npm install npm@latest -g
```


#### Outdated module

현재 package.json 에 설치된 버전과 명시된 버전 그리고 최신 버전과 차이를 알 수 있다.

```sh
npm outdated
Package                  Current  Wanted  Latest  Location
body-parser               1.15.2  1.15.2  1.18.2  application-name
debug                      0.7.4   0.7.4   3.1.0  application-name
```

최신 버전으로 설치를 하려면 package.json을 버전코드로 변경하고 업데이트를 진행한다.

모든 패키지를 업데이트할 수 있다.

```
npm update
+ mongoose@4.13.1
added 1 package, removed 4 packages and updated 2 packages in 32.975s
```

특정 모듈만 업데이트하려면 패키지를 명시하면 된다.

```
npm update debug
```



#### lts 버전

Node.js 는 가용 버전이 장기지원을 위해서 [LTS(Long Term Support)](https://github.com/nodejs/Release) 프로그램을 진행하고 있다. 실제 운영 서버는 이런 LTS 버전을 중심으로 가동될 것이다. **nvm** 도 lts 만을 선별해서 설치하고 관리할 수 있다.

현재 LTS 버전중 최신버전만을 출력하려면,

```sh
nvm ls-remote --lts |grep Latest
```


#### 전역 패키지 통합

nvm에서 새로운 node 버전을 설치하면서 기존 node 버전에서 사용중인 패키지를 통합해서 설치 할 수 있다.  예를 들어 최신 8 버전을 설치하며 사용중인 기존 6버전 패키지를 함께 설치하려면,

```sh
nvm install v8 --reinstall-packages-from=v8.0.1
```



#### Default alias 잘 못 된 경우

새로 로그인 혹은 버전 변경시 다음 메시지 출력,

```
N/A: version "N/A -> N/A" is not yet installed.
```

이 경우 가능성은 제거한 버전이 default 로 지정되서 그런듯 하다, 아래의 경우 default가 v8.7 인데 삭제해서 없기 때문이다.

```
$ nvm ls
->      v6.11.5
        v7.10.1
default -> v8.7.0 (-> N/A)
node -> stable (-> v7.10.1) (default)
stable -> 7.10 (-> v7.10.1) (default)
iojs -> N/A (default)
lts/* -> lts/boron (-> v6.11.5)
lts/argon -> v4.8.5 (-> N/A)
lts/boron -> v6.11.5
```

그래서 default 를 설치된 버전으로 변경해서 지정해 주면 위 메시지가 나오지 않는다.


<br/>
<br/>
### nodist

Nodist 는 윈도우즈 환경에서 Nvm과 비슷하게 nodejs의 버전을 관리할 수 있다.
 - https://github.com/marcelklehr/nodist
 - 윈도우즈 인스톨러로 다운로드 가능.
 - git 혹은 zip 으로 다운로드 가능.

#### 인스톨러로 설치시

[nodist releases](https://github.com/marcelklehr/nodist/releases) 에서 nodist 인스톨러를 다운받아 설치한다.

#### git으로 설치

윈도우즈 `git`으로 clone을 해오고, 윈도우의 환경설정 변수를 설정한다.

```
C:\>git clone git://github.com/marcelklehr/nodist.git
C:\>setx /M PATH "C:\users\thinkbee\nodist\bin;%PATH%"
C:\>setx /M NODIST_PREFIX "C:\users\thinkbee\nodist"
```

이제 `cmd`에서 `nodist`의 업데이트를 실행해주고 사용한다.

```
C:\>nodist selfupdate
```

기타 다른 환경에서 설치 및 사용법은 [nodist](https://github.com/marcelklehr/nodist) 에서 확인한다.

---


### npm

`npm` 은 nodejs 패키지 관리자로 선택한 모듈을 설치, 갱신 및 삭제할 수 있고, 스크립팅을 통해 프로세스 관리까지 할 수 있다.

#### npm upgrade

`node`를 설치후 `npm`을 업그레이드 해준다. npm 자체는 다음 같이 업그레이드 한다.

```sh
npm i -g npm
npm install npm@latest -g
```


#### Outdated module

현재 package.json 에 설치된 버전과 명시된 버전 그리고 최신 버전과 차이를 알 수 있다.

```sh
npm outdated
Package                  Current  Wanted  Latest  Location
body-parser               1.15.2  1.15.2  1.18.2  application-name
debug                      0.7.4   0.7.4   3.1.0  application-name
```

최신 버전으로 설치를 하려면 package.json을 버전코드로 변경하고 업데이트를 진행한다.
혹은 모든 패키지를 업데이트할 수 있다.

```
npm update
+ mongoose@4.13.1
added 1 package, removed 4 packages and updated 2 packages in 32.975s
```


#### `npm in` vs. `npm up`

install 과 update는 package.json 에 명시된 버전에 대응해 실행된다.
 - npm install installs all modules that are listed on package.json file and their dependencies.
 - npm update updates all packages in the node_modules directory and their dependencies.
 - npm install express installs only the express module and its dependencies.
 - npm update express updates the express module and its dependencies.

[stackoverflow: npm install vs update](https://stackoverflow.com/questions/12478679/npm-install-vs-update-whats-the-difference) 에 따르면 버전 관리에 따른 차이를 보인다.

개별 모듈만 업그레이드 하려면 `install @latest` 같이 사용한다. 다음 같이 npm outdated 로 프로젝트 버전 상태를 확인해 보면

```sh
$ npm outdated
Package                     Current   Wanted      Latest  Location
agenda                        0.9.0    0.9.1       1.0.3  www_app
body-parser                  1.15.2   1.15.2      1.18.2  www_app
charset                       1.0.0    1.0.1       1.0.1  www_app
cheerio                      0.22.0   0.22.0  1.0.0-rc.2  www_app
cookie-parser                 1.3.5    1.3.5       1.4.3  www_app
debug                         2.2.0    2.2.0       3.1.0  www_app
ejs                           2.5.7    2.5.8       2.5.8  www_app
express                      4.14.1   4.14.1      4.16.3  www_app
iconv                         2.2.1    2.3.0       2.3.0  www_app
mongoose                     4.13.2  4.13.12      5.0.13  www_app
morgan                        1.7.0    1.7.0       1.9.0  www_app
multer                        1.2.0    1.3.0       1.3.0  www_app
passport                      0.2.2    0.2.2       0.4.0  www_app
passport-local-mongoose       1.3.0    1.3.0       5.0.0  www_app
pug                      2.0.0-rc.4    2.0.3       2.0.3  www_app
request                      2.83.0   2.85.0      2.85.0  www_app
serve-favicon                 2.3.0    2.3.2       2.5.0  www_app
```

`i` 명령으로 현재 package.json에 명시된 버전이 설치된다.

```
$ npm i mongoose
+ mongoose@4.13.12
updated 7 packages in 17.037s
```

@latest 제한자로 최신 버전으로 업그레이드 할 수 있다.

```
$ npm i mongoose@latest
+ mongoose@5.0.13
```


어쨌든 버전 갱신을 목적으로 하면 `npm i` 를 일반적으로 써도 무방하다. 

### Process management

다음 []() 글로 좀더 깊은 내용으로 옮겼다.
