---
title: Linux - Basic command usages
date: 2017-04-05 09:00:00 +0900
layout: post
tags: [bash]
categories: [Linux, Programming]
---

#  기초 리눅스 명령어

리눅스는 기본적으로 CLI Command Line Interfae를 기반으로 Shell 이라는 interactive 명령 실행 방법을 사용한다. Shell 에 명령은 Prompt 를 통해 입력하고 결과를 출력한다.

## Shell

csh, tsh, bash 등 다양한 종류의 쉘, 최신 리눅스 배포본은 bash 를 기본으로 제공.
쉘 프롦프트는 일반적으로 루트 사용자, 일반 사용자에 따라 `#` 과 `$` 로 표시한다.

```
qkboo@orangepiplus:~$
```

### 사용자 정보

리눅스 시스템 계정에 등록한 사용자 정보, 시스템 사용자 정보를 얻을 수 있다. 

#### w, who, whoami, finger

현재 시스템을 사용하는 사용자 정보 확인할 수 있다. 

`w`는 현재 시스템에 로그인한 터미널 정보를 화면에 출력한다.

```
$ w
 12:45:02 up 3 days, 11:47,  1 user,  load average: 0.95, 0.86, 0.87
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
qkboo    pts/2    gangtaigoh-mbp   09:54    1.00s  2.94s  0.04s w
```

`who`는 현재 시스템에 사용자 정보를 화면에 출력한다.

```
qkboo@orangepiplus:~$ who
qkboo    pts/2        2017-04-04 09:54 (gangtaigoh-mbp)
```

finger는 계정에 등록된 사용자의 프로필 정보를 출력해 준다. 

```
$ finger
LOGIN@ IDLE JCPU PCPU WHAT 20Feb12 ?xdm? 21:19 0.21s /usr/bin/gnome- 14:25 0.00s 0.01s 0.00s w
Idle Login Time Office Office Phone Mar 25 14:25
Feb 20 16:35
-
Login Name pkhinkorea
root root
Tty pts/1
*:0
```

whoami는 내가 로그인한 사용자를 보여준다. 

```
$ whoami
qkboo
```

#### passwd

현재 로그인한 사용자의 비밀번호를 변경하고 싶은 경우 `passwd` 라고 입력한다. 

```
$ passwd
Changing password for user qkboo.
Changing password for qkboo
(current) UNIX password:
New UNIX password:
Retype new UNIX password:
passwd: all authentication tokens updated successfully.
```


##  디렉토리 및 파일 이동/복사/생성/삭제 정보 보기

pwd(print name of current working directory)는 현재 자신이 있는 디렉토리의 위치를 나타 낸다.

> $ pwd

mkdir(make directory)은 디렉토리를 만드는데 사용한다.

> $ mkdir [디렉토리 명]

cd(change directory) 명령어는 디렉토리를 변경하는 명령어다.
> $ cd [경로]


ls(list segments) 명령은 현재 디렉토리의 파일 목록을 보여준다.
> $ ls (옵션) [경로]


#### 디렉토리를 이동한다.

현재 디렉토리의 위치를 확인한다.

```sh
$ pwd
/home/qkboo/
```

### 디렉토리 생성 및 이동하기

새로운 디렉토리를 생성한다.

```sh
$ mkdir test
$ cd test
$ pwd
/home/qkboo/test
```


#### 경로에 따라 이동

`cd` 명령으로 절대경로/상대경로로 이동할 수 있다

> cd [경로] : [경로]로 지정한 디렉토리로 이동한다.
> cd [./경로] : 현재 디렉토리 바로 밑은 [경로] 디렉토리로 이동한다.
> . : `.`은 현재 디렉토리를 의미한다.
> .. : `..`은 현 디렉토리 한단계 상위를 의미한다
> cd [../경로] : 현 디렉토리보다 한단계 상위 디렉토리로 이동한다.
> / : `/`는 루트 디렉토리를 의미한다.


Home 와 Root 디렉토리로 이동한다.

```sh
$ cd         # 홈디렉토리로 이동
$ cd ~       # 역시 홈 디렉토리로 이동
$ cd ~qkboo  # 해당 아이디의 홈디렉토리로 이동
```

Root 디렉토리는 파일 시스템의 최상위 위치이다. 보통 root 사용자가 파일을 생성/변경/추가/삭제할 수 있다.

```sh
$ cd /
```


#### 파일 목록보기

`ls` 옵션 값은 다음과 같고, 중복하여 사용할 수 있다.
-a : 숨겨진 파일도 출력
-l : 파일의 상세한 정보 (권한, 용량, 수정날짜)등을 볼 수 있다.
-lh: 파일의 용량을 단위와 함께 출력 -S: 크기순으로 출력
-F : 파일 종류 (파일, 디렉토리, 링크)에 따라 표시
-r: 역순으로 출력
-t: 시간 순으로 출력
-F: 파일과 디렉토리를 구별해서 출력 (디렉토리 뒤에 /가 붙음) -x: 정렬순서를 가로로 출력
-u: 최종적으로 사용된 시간 순서대로 출력

계속해서 디렉토리 ls 명령을 사용해 보자.

```sh
$ pwd
/                        # 현재 루트 디렉토리
$ ls
$ ls -l
$ ls -al
$ ls -alF
```

사용자 홈디렉토리로 이동해 사용해 보자

```sh
$ cd                     # 홈 디렉토리로 이동
$ ls -l
$ cd test
$ cd ../..
$ pwd
$ ls -l ~/test           # 상대 경로의 목록을 출력한다.
$ cd ~
$ ls -l /var/log         # 절대 경로의 목록을 출력한다.
```

디렉토리 목록을 시간 순서로 보여준다.

```sh
$ ls -trl /var/log
```


#### 와일드카드의 활용

와일드카드는 여러 파일을 한꺼번에 지정할 목적으로 사용하는 기호로, *과 ? 두 종류가 있 다. ?는 임의의 한 문자를 의미하여 단 한 글자를 대체하며, *는 임의의 문자열을 의미하여 여 러 글자를 대체해준다. 

와일드카드 문자 `*`로 특정 이름을 가진 목록만 보여준다.

```sh
$ ls -l /var/log/*.log
```


### 이동, 복사, 삭제

cp(copy)는 파일 및 디렉토리의 복사를 담당하는 명령이다.
용법 : $ cp (옵션) [원본] [복사본]

cp의 옵션은 다음과 같은 항목들이 있다.
-a : 원본파일의 속성, 링크 정보를 그대로 유지
-b : 같은 이름의 파일이 있을 때 백업파일 생성 후 복사
-f : 같은 이름의 파일이 있을 때 삭제 후 복사 (덮어쓰기)
-u: 같은 이름의 파일이 있을 때 변경날짜가 같거나 최근이면 복사하지 않음 (업데이트) -i : 같은 이름의 파일이 있으면 사용자에게 물어본다
-r: 하위 디렉토리까지 복사
-p: 권한까지 복사
-v: 복사 과정을 보여준다
 
[pkhinkorea@astro10 pkh]$ ls
a01 a02 a03 a10 a12 a123 [pkhinkorea@astro10 pkh]$ cp a01 b01 [pkhinkorea@astro10 pkh]$ ls
a01 a02 a03 a10 a12 a123 b01 [pkhinkorea@astro10 pkh]$ cp -i a02 b01 cp: overwrite `b01'? yes [pkhinkorea@astro10 pkh]$ mkdir test [pkhinkorea@astro10 pkh]$ ls
a01 a02 a03 a10 a12 a123 b01 test [pkhinkorea@astro10 pkh]$ ls test [pkhinkorea@astro10 pkh]$ cp a0? test [pkhinkorea@astro10 pkh]$ ls test
a01 a02 a03
[pkhinkorea@astro10 pkh]$ mkdir test/test00 [pkhinkorea@astro10 pkh]$ ls test
a01 a02 a03 test00
[pkhinkorea@astro10 pkh]$ cp -r test test1 [pkhinkorea@astro10 pkh]$ ls
; a01파일을 b01에 복사하라
; a02파일을 b01에 복사하되, 중복파일은 물어보라. ; test폴더를 만들어라.
; test폴더에 있는 파일 및 폴더를 나타내라 (아무 것도 없음) ; a0? 조건을 만족하는 파일을 test폴더로 복사하라
; test폴더에 있는 파일 및 폴더를 나타내라
; test아래 test00이라는 폴더를 만들어라
; test의 하위 디렉토리를 포함해서 test1로 복사하라.
-5-
Basic Linux
a01 a02 a03 a10 a12 a123 b01 test test1 [pkhinkorea@astro10 pkh]$ ls test1
a01 a02 a03 test00
mv(move) 명령어는 파일 및 디렉토리를 옮겨주는 명령어이다. 또한 파일의 이름을 다른 이 름으로 변경할 때도 쓰인다.
용법: $mv(옵션)[원본][이동할위치,혹은새파일명]
mv의 옵션은 다음과 같다.
-b: 같은 이름의 파일이 있을 때 백업파일 생성 후 이동 -f: 같은 이름의 파일이 있을 때 삭제 후 이동 (덮어쓰기) -i: 같은 이름의 파일이 있으면 사용자에게 물어본다
-v: 이동 과정을 보여준다
[pkhinkorea@astro10 pkh]$ mv b01 test; b01을 test폴더로 이동하라 [pkhinkorea@astro10 pkh]$ ls
a01 a02 a03 a10 a12 a123 test test1
[pkhinkorea@astro10 pkh]$ ls test
 a01 a02 a03 b01 test00 [pkhinkorea@astro10 pkh]$ mv a02 b02 [pkhinkorea@astro10 pkh]$ ls
a01 a03 a10 a12 a123 b02 test test1 [pkhinkorea@astro10 pkh]$ mv test1 test2 [pkhinkorea@astro10 pkh]$ ls
a01 a03 a10 a12 a123 b02 test test2
; a02란 파일을 b02란 파일로 이름을 바꿔라
; test1이란 폴더의 이름을 test2로 바꿔라
rm(remove) 명령어는 파일 및 디렉토리를 삭제할 때 사용한다.
용법 : $ rm (옵션) [파일 혹은 디렉토리명]
 rm의 옵션은 다음과 같다
-i: 삭제 여부를 확인한다. -f: 확인작업 없이 삭제
-r: 하위 디렉토리까지 삭제 -v: 삭제 과정을 보여준다
[pkhinkorea@astro10 pkh]$ rm b02 [pkhinkorea@astro10 pkh]$ ls
a01 a03 a10 a12 a123 test test2 [pkhinkorea@astro10 pkh]$ rm *
rm: cannot remove `test': Is a directory rm: cannot remove `test2': Is a directory [pkhinkorea@astro10 pkh]$ ls
test test2
; b02파일을 삭제하라
; 모든 파일을 삭제하라
; 시스템 : test는 디렉토리여서 삭제가 안 됩니다. ; 시스템 : test는 디렉토리여서 삭제가 안 됩니다.
; 디렉토리들만 남았다.

[pkhinkorea@astro10 pkh]$ rm –rf * ; 디렉토리도 강제로 삭제하고 묻지 마세요 [pkhinkorea@astro10 pkh]$ ls ; 아무 것도 남지 않음


### 파일 보기
1) cat (catenate)
주로 파일을 보는데 사용한다.
용법: $cat(옵션)(>혹은>>)[파일명]
cat > (파일명) 으로 입력시에는 간단한 새 파일 역시 제작이 가능하다. (이 때 종료는 +)
[pkhinkorea@astro10 pkh]$ cat star.txt cat: star.txt: No such file or directory [pkhinkorea@astro10 pkh]$ cat > star.txt show me the money
black sheep wall
[pkhinkorea@astro10 pkh]$ cat star.txt show me the money
black sheep wall
옵션으로는
-n : 행 번호를 출력한다
-b : 빈 행에는 번호를 출력하지 않는다 -e : 각 행 끝에 $를 단다
  - 10 -
ctrl
 c
Basic Linux
-s : 연속 되는 빈 행을 하나로 출력한다.
[pkhinkorea@astro10 pkh]$ cat 1 show me the money 2
3
4 black sheep wall [pkhinkorea@astro10 pkh]$ cat
1 show me the money$ $
2 black sheep wall$
2) more
–n star.txt
–nbes star.txt
; star.txt 파일을 행번호와 함께 출력하라.
; star.txt.파일을 행번호와 함께 출력하되, 빈행의 번호는 빼고, 연 속되는 빈행은 하나로 출력하라. 그리고 각 행의 끝에는 $를 달아라.
more 역시 텍스트 파일을 출력해주는 명령어이다.
용법 : $ more (옵션) [파일명]
옵션은 아래와 같다.
-c : 내용을 보여주기 전에 화면을 청소
-lines :한번에보여줄줄수를정함
-d : 스페이스나 q키를 누르라는 프롬프트를 출력
 [+숫자] : 지정한
[pkhinkorea@astro10 [pkhinkorea@astro10 [pkhinkorea@astro10 [pkhinkorea@astro10 show me the money
black sheep wall marine --More--(31%) [pkhinkorea@astro10
3) head와 tail
행부터 보여줌
pkh]$ cat terran.txt >> star.txt pkh]$ cat zerg.txt >> star.txt pkh]$ cat protoss.txt >> star.txt pkh]$ more –5 star.txt
pkh]$ more +5 star.txt
; 한 번에 다섯 줄 씩 보이게 출력하라.
; 5번째 행부터 출력하라.
head는 파일의 시작부분을, tail은 끝부분을 보여준다.
용법 : $ head/tail (옵션) [파일명]
옵션으로는
 - 11 -
Basic Linux
-숫자 : 처음(끝)부터 보여줄 줄 수를 지정한다.
[pkhinkorea@astro10 show me the money
[pkhinkorea@astro10 zergling
marine
scv
pkh]$ head –3 star.txt
pkh]$ tail –3 star.txt
; star.txt 파일의 시작 세 줄을 출력한다.
; star.txt 파일의 끝 세 줄을 출력한다.
Tip 2. 파이프라인(pipeline) | (shift+\)는 프로그램간 결과를 전송하는 명령어로, 두 명령 어를 복합해서 명령을 내릴 수 있다. 앞의 명령어에서 나오는 출력을, 뒤의 명령어로 받아서 실행한다. 가령, cat –n star.txt | tail –2 이라고 입력하면, 앞의 cat명령어대로 줄번호를 적은 파일을 출력...할 것을 다시 입력으로 받아 tail의 명령을 수행하여, 뒤에서부터 2번째 행 까지 나타내게 된다.
[pkhinkorea@astro10 pkh]$ cat -n star.txt | tail -2 19 marine
20 scv
또한 리다이렉션(redirection) > 는 파일간의 결과 전송을 의미한다. 프로그램을 돌려 나온 결과를 화면에 출력하는 것이 아니라, 리다이렉션 다음에 나오는 파일명에 저장을 해달라는 것을 의미한다. 어펜드(append)는 리다이렉션의 파일 뒤에 붙이기 기능이 추가 된 것이다. 리다이렉션은 기존 같은 이름의 파일이 있을 경우, 실행이 되지 않거나, 혹은 지우고 새 파일 을 만들지만, 어펜드는 기존 파일 뒤편에 합병해준다.
[pkhinkorea@astro10 pkh]$ cat terran.txt marine
scv
[pkhinkorea@astro10 pkh]$ cat protoss.txt zealot
probe
[pkhinkorea@astro10 pkh]$ cat zerg.txt
drone
zergling
[pkhinkorea@astro10 pkh]$ cat zerg.txt > protoss.txt [pkhinkorea@astro10 pkh]$ cat protoss.txt
drone
zergling
[pkhinkorea@astro10 pkh]$ cat terran.txt >> protoss.txt [pkhinkorea@astro10 pkh]$ cat protoss.txt
drone
zergling
marine
scv
[pkhinkorea@astro10 pkh]$ ls *txt
; terran.txt를 출력하라
; protoss.txt를 출력하라.
; zerg.txt를 출력하라.
; zerg.txt를 protoss.txt에 덮어 씌워라
; protoss.txt를 출력하라.
; 기존 내용이 없어지고, zerg.txt내용과 같
은 것을 볼 수 있다.
; terran.txt 파일을 protoss.txt파일에 합병하라. ; protoss.txt 파일을 출력하라
; 기존의 protoss.txt 파일에, terran.txt
; 파일이 합병된 것을 확인 할 수 있다.
; txt로 끝나는 파일들을 출력하라
- 12 -
Basic Linux
protoss.txt star.txt terran.txt zerg.txt [pkhinkorea@astro10 pkh]$ ls *txt > starcraft.txt
[pkhinkorea@astro10 pkh]$ cat starcraft.txt protoss.txt
star.txt
terran.txt
zerg.txt
III. 검색하기
1) which
명령어를 검색하는 프로그램이다.
용법 : $ which [명령어]
[pkhinkorea@astro10 pkh]$ which xgterm /usr/local/bin/xgterm
2) grep(global regular expression print)
; txt로 끝나는 파일을 출력하는 대신, starcraft.txt라는 파일을 만들고 그 안에 저장하라.
 ; xgterm 실행 파일이 어딨는지 찾아라.
파일 내용을 뒤져, 해당하는 단어나 문자를 찾는다.
용법 : $ grep (옵션) [단어] [파일명]
옵션은 아래와 같다.
-c : 파일이 아닌, 총 숫자를 출력
-C 숫자 : 일치하는 행에서 위와 아래로 지정한 숫자만큼의 행을 추가로 더 보여준다. -A 숫자 : 일치하는 행에서 아래로 지정한 숫자만큼의 행을 추가로 더 보여준다.
-i : 대소문자를 구분하지 않는다.
-n : 파일 내 행 번호를 함께 출력한다.
-v : 지정한 문구와 일치하지 않는 것들을 보여준다.
-r : 하위 디렉토리까지 검색
-b : 블록 번호 표시
[pkhinkorea@astro10 pkh]$ grep scv * protoss.txt:scv
star.txt:scv
star.txt:scv
...
 - 13 -
Basic Linux
[pkhinkorea@astro10 pkh]$ grep -nC 1 scv star.txt 5-marine
6:scv
7-drone
-- 9-marine 10:scv 11-drone ...
3) find
find는 크기나, 수정된 날짜 등 지정한 조건에 맞는 파일들을 찾아 준다.
용법 : $ find (경로) [조건]
조건은 다음과 같다.
-name [파일명] : 이름으로 검색한다.
-user [아이디] : 소유자로 검색한다.
-amin -숫자 : 숫자 분 이내에 접근한 파일을 검색한다.
-atime -숫자 : 숫자 일 이내에 접근한 파일을 검색한다.
-ctime -숫자 : 숫자 일 이내에 상태가 변경된 파일을 검색한다.
-mtime -숫자 : 숫자 일 이내에 수정한 파일을 검색한다.
-perm [조건] : 권한이 조건에 맞는 파일을 찾는다.
-size [조건] : 파일 크기가 일치하는 것을 찾는다. 이 때 숫자 뒤에 +를 붙이면 해당 크기 이
상을, -를 붙이면 해당크기 이하를 의미한다. 또한, 숫자뒤에 k를 붙이면 kb, m을 붙이면 메가 바이트를 의미한다.
-newer [파일명] : 해당파일보다 이후에 갱신된 모든 파일을 검색한다.
-exer [명령] {} \; : 원하는 검색 조건에 맞는 파일들을, 주어진 명령으로 실행한다.
$ find -name *fits -size -5M ./alpTrES-2-698_I.fits
$ find /home/pkhinkorea -amin -50 -name *txt /home/pkhinkorea/pkh/protoss.txt
$ find /home/pkhinkorea -atime -5 -name *txt /home/pkhinkorea/pkh/zerg.txt
/home/pkhinkorea/pkh/terran.txt
/home/pkhinkorea/pkh/star.txt
/home/pkhinkorea/pkh/protoss.txt
/home/pkhinkorea/pkh/starcraft.txt
$ find /home/pkhinkorea -atime -5 -name *txt -exec ls -la {} \; -rw-r--r-- 1 pkhinkorea master 15 Mar 7 20:57 /home/pkhinkorea/pkh/zerg.txt -rw-r--r-- 1 pkhinkorea master 63 Mar 8 21:49 /home/pkhinkorea/pkh/terran.txt -rw-r--r-- 1 pkhinkorea master 141 Mar 8 21:50 /home/pkhinkorea/pkh/star.txt -rw-r--r-- 1 pkhinkorea master 26 Mar 7 20:58 /home/pkhinkorea/pkh/protoss.txt -rw-r--r-- 1 pkhinkorea master 41 Mar 8 21:44 /home/pkhinkorea/pkh/starcraft.txt
 - 14 -
Basic Linux
IV. 압축
Linux에서 압축을 담당하는 명령어는 tar와 gzip(GNU zip)이다. tar는 파일을 하나로 모으되 압축은 하지 않고, gzip은 파일을 모을 수는 없지만 압축을 담당한다. 따라서 여러 파일을 압 축할 때는 tar로 파일을 묶고 gzip으로 tar파일을 압축하게 된다. tar의 옵션에서 gzip 실행여 부를 지정할 수 있기 때문에, gzip이 단독으로 쓰일 경우는 많지 않다.
tar의 옵션은 다음과 같다
-c: 묶음 파일 생성
-x: 묶음 파일 해제
-v: 작업내용 출력
-p: 권한을 그대로 유지
-z: gzip형식으로 압축 혹은 압축해제 -f: 사용할 파일의 tar지정
[pkhinkorea@astro10 pkh]$ ls
protoss.txt star.txt starcraft.txt terran.txt zerg.txt [pkhinkorea@astro10 pkh]$ tar -czvf blizzard.tar.gz *txt
protoss.txt
star.txt
starcraft.txt
terran.txt
zerg.txt
[pkhinkorea@astro10 pkh]$ ls
blizzard.tar.gz protoss.txt star.txt starcraft.txt terran.txt zerg.txt [pkhinkorea@astro10 pkh]$ rm *txt
[pkhinkorea@astro10 pkh]$ ls
blizzard.tar.gz
[pkhinkorea@astro10 pkh]$ tar -xzvf blizzard.tar.gz
protoss.txt
star.txt
starcraft.txt
terran.txt
zerg.txt
[pkhinkorea@astro10 pkh]$ ls
blizzard.tar.gz protoss.txt star.txt starcraft.txt terran.txt zerg.txt
 용법: $gzip[파일명] ;압축할때 $ gunzip [파일명] ; 압축 풀 때
 용법 : $ tar –c[옵션] [압축파일명] [압축할파일 혹은 디렉토리] ; 압축 할 때 $ tar –x[옵션] [압축파일명] [압축할파일 혹은 디렉토리] ; 압축 풀 때
- 15 -
Basic Linux
V. 권한
1) 권한
권한은 해당 파일에 대한 읽기(r, 4).쓰기(w, 2).실행(x, 1) 여부를 결정한다. 디렉토리나 파 일의 권한 설정 여부는 ls –l을 입력할 때 제일 앞에 적히는 10자리의 기호로 알 수 있다.
10자리의 기호는
[디렉토리(d),파일 구분자(-) 1자리]+[소유자권한 3자리]+[그룹권한 3자리]+[전체권한 3자리]로 구성되어 있다.
가령 –rwxrwxr-x라고 표기된 것은, 해당 파일(-)은 소유자는 읽기.쓰기.실행이 전부 가 능(rwx)하며, 마찬가지로 그룹 역시 전부 가능(rwx)하다. 하지만 그 외의 사람들은 읽고 실행 은 가능하지만, 수정은 불가능(r-x)하다라는 것을 의미한다.
drw-r-----라고 표기된 것은, 해당 디렉토리(d)는 소유자는 읽기.쓰기가 가능하지만(rw-) 그룹은 읽기만 가능하며 (r--), 그 외의 사람들은 접근조차 허용하지 않는다는 의미(---)다.
tip 3. 폴더의 경우 만들 때 –m 옵션값을 붙여주면 시작부터 권한을 설정해 줄 수 있다.
가령, 소유주는 읽고(4) 쓰고(+2) 실행(+1)이 가능하고, 그룹은 읽고(4), 실행(+1)만 가능 하며, 그 외의 사람은 읽기(4)만 가능한 test라는 폴더를 만들고 싶을 경우 아래와 같이 입력 한다.
$mkdir –m 754 test
2) chmod(change mode)
파일 및 디렉토리의 권한을 설정하는 명령어이다.
용법 : $ chmod [권한값] [파일명]
[pkhinkorea@astro10 pkh]$ ls protoss.txt terran.txt zerg.txt [pkhinkorea@astro10 pkh]$ ll total 12
-rw-r--r-- 1 pkhinkorea master 26 Mar 7 20:58 protoss.txt -rw-r--r-- 1 pkhinkorea master 63 Mar 8 21:49 terran.txt -rw-r--r-- 1 pkhinkorea master 15 Mar 7 20:57 zerg.txt
현재, protoss.txt, terran.txt, zerg.txt 라는 세 개의 파일이 있고, 이 파일들의 권한은 소유주 는 읽기.쓰기, 그룹 및 그 외의 사람들은 읽기 전용으로 되어 있다. protoss.txt파일에 대해


소유주의 쓰기 권한을 제해, 읽기(4)만 가능하게 만들어보자.

```sh
$ chmod 444 protoss.txt
```
이후 vi로 protoss.txt 파일을 수정하려고 하면, read only라고 적혀 있고 수정이 되지 않는 것을 알 수 있다. 모든 파일을 모든 사용자에 대해 읽고(4) 쓰기(2)를 자유롭게 열어보자.
[pkhinkorea@astro10 pkh]$ chmod 666 *txt

#### chown(change owner)

디렉토리나 파일의 권한을 다른 사용자나 그룹에 넘겨주는 명령어이다.

> 용법 : $ chown (옵션) [소유자(:그룹)] [파일 혹은 디렉토리명]
> -R : 모든 하위 디렉토리 및 파일의 권한을 함께 변경한다.
> -f : 권한 변경 실패 시, 에러 메시지를 출력하지 않는다.
> -v : 파일에 대해 변경한 정보나, 변경되지 않은 정보를 출력한다.
> 
iraf설치 시, /iraf/iraf/와 그 하위 폴더를 iraf 그룹의, iraf유저에게 권한을 넘겨주는 데, 그 때 명령은 아래와 같다.

```sh
$ sudo chown –R iraf:iraf /iraf
```

### 링크
링크에는 ln(link) 명령어가 사용된다.

#### 하드링크
하드링크는 동일한 데이터 블록을 참조하는 서로 다른 파일을 만드는 것이다. (디렉토리는 하 드링크가 불가능) 원본의 이동이나 삭제와 상관없이 링크된 파일은 그대로 남아 있다.
  용법 : $ ln [원본 파일] [하드링크 파일]
[pkhinkorea@astro10 pkh]$ ls
protoss.txt terran.txt zerg.txt
[pkhinkorea@astro10 pkh]$ mkdir linktest [pkhinkorea@astro10 pkh]$ cd linktest [pkhinkorea@astro10 linktest]$ ln ../protoss.txt hardlink.txt
; 한 폴더 위의 protoss.txt와 이 폴더의 hardlink.txt사이 하드링크를 생성

[pkhinkorea@astro10 linktest]$ ls
hardlink.txt
[pkhinkorea@astro10 linktest]$ cat ../protoss.txt drone
zergling
marine
scv
[pkhinkorea@astro10 linktest]$ cat hardlink.txt drone
zergling marine scv
; 두 파일 사이의 내용은 동일
cp를 통해 만든 파일과의 차이점으로는, 하드링크로 생성된 파일은 하드 공간을 차지하지 않 으며, 원본 수정 시 하드링크도 역시 수정된다.

###  심볼릭 링크
심볼릭 링크는, 윈도우의 바로가기 아이콘과 비슷한 역할을 수행한다. 하드링크와 달리, 디렉 토리에도 적용이 가능하기 때문에 긴 디렉토리로 이동할 때 유용하며, 원본 파일의 이동이나 삭제에 영향을 받는다.
용법 : $ ln –s [원본 파일이나 디렉토리] [링크걸 파일이나 디렉토리]
 [pkhinkorea@astro10 pkh]$ mkdir 1 [pkhinkorea@astro10 pkh]$ mkdir 1/2 [pkhinkorea@astro10 pkh]$ mkdir 1/2/3 [pkhinkorea@astro10 pkh]$ mkdir 1/2/3/4 [pkhinkorea@astro10 pkh]$ cp *txt 1/2/3/4 [pkhinkorea@astro10 pkh]$ ln -s 1/2/3/4 0 [pkhinkorea@astro10 pkh]$ ls
0 1 protoss.txt terran.txt zerg.txt [pkhinkorea@astro10 pkh]$ ls 0 protoss.txt terran.txt zerg.txt

## 디스크 및 폴더 크기

파일 및 디렉토리의 사용량을 확인할 때는 du(disk usage)라는 명령어를 사용한다.
> 용법 : $ du (옵션) (파일이나 디렉토리)
> -h : 단위를 표시한다.
> -s : 간단히 합산된 용량만 보여준다


```
$ mkdir -p 1/2/3/4/

$ du ./1/2/3/4
4 ./1/2/3/4

$ du -sk /home               # home 디렉토리의 용량을 KB 단위로
7561876 /home
```


`df`(disk free)는 사용 중인 파일 시스템의 전체 용량, 사용한 용량, 사용 가능한 용량, 사용률 등을 보여주는 명령이다.
> 용법: $df(옵션)
> 옵션은 아래와 같다
> -a : 크기가 0인 파일 시스템까지 나타낸다.
> -i : 용량을 block 단위가 아닌, inode 단위로 나타낸다.
> -T : 파일 시스템의 종류와 함께 표시한다.
> -t [파일시스템] : 뒤에 적은 파일 시스템만 나타낸다.
> -x [파일시스템] : 뒤에 적은 파일 시스템을 제외하고 나타낸다. -h : 단위를 표시한다.


```sh
$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/root       15273212 10699284   3901600  74% /
devtmpfs          469532        0    469532   0% /dev
tmpfs             473864        0    473864   0% /dev/shm
tmpfs             473864    12368    461496   3% /run
tmpfs               5120        4      5116   1% /run/lock
tmpfs             473864        0    473864   0% /sys/fs/cgroup
/dev/mmcblk0p1     61384    21512     39872  36% /boot
tmpfs              94776        0     94776   0% /run/user/1001
```

## clear, history

현재 터미널의 창을 지울 때는 clear라는 명령어를, 기존 자신이 입력했던 명령어를 보고자 할 때는 history라는 명령어를 입력하면 된다.
용법 : $ history (숫자) ; 숫자만큼의 명령어 내역을 출력
3) ps(process status), kill
ps는 작동중인 프로세스를 나타낸다.
용법: $ps(옵션)
옵션값은 아래와 같다.
e: 모든 프로세스를 표시
f : 전체 경로로 프로세스를 표시
l : 긴 포맷으로 출력
u : 실행한 유저 및 실행 시간을 표시
a : 다른 유저의 프로세스도 표시
x : 터미널 제어 없이 프로세스 현황 보기
kill은 실행 중인 특정 프로세스를 종료하는데 쓰인다.
용법 : $ kill –9 [PID 번호]
$ kill –l을 입력하면 kill 명령어의 기타 옵션들을 볼 수 있는데, 이 중 9번째 옵션이 프로세 스를 종료하는 옵션이다. 따라서 ps로 확인한 kill –9 이후 ps로 확인한 PID 번호를 입력하면 해당 프로세스가 강제 종료된다.
[khpark@beatrice ~]$ ps
   PID TTY
11024 pts/20
11123 pts/20
11144 pts/20
[khpark@beatrice ~]$ kill -9 11123
TIME CMD 00:00:00 bash
00:00:00 xgterm 00:00:00 ps

## 리눅스 서버 접속 및 파일 전송 방법

1) 리눅스-> 리눅스 (ex 천문 전산 실습실에서 astro10에 접속 시도)
1 접속하기

$ ssh [아이디]@[서버]

접속을 종료할 때는 exit라고 입력하면 된다.
2 파일전송하기
$ sftp [아이디]@[서버]
이 때 ls나 cd와 같은 명령어는 서버의 컴퓨터에 적용되며, 내 컴퓨터에서 ls나 cd 등의 명령 을 내리고 싶을 때는 앞에 l자를 하나 더 붙이면 된다. (lls, lcd, lcp, lrm, ...)
파일을 받을 때는 get (파일이름), 파일을 전송할 때는 put (파일이름) 접속을 종료할 때는 exit라고 입력하면 된다.
2) 윈도우-> 리눅스 (ex 개인 컴퓨터에서 astro10에 접속 시도)
1 프로그램 설치하기
서울대학교 정보화본부 홈페이지(it4u.snu.ac.kr) 이동 후, 캠퍼스 라이선스 S/W 다운로드 항 목으로 들어간다. 11번 게시물의 Xmanager Enterprise 3.0을 설치한다.
이 사이트는 교내에서만 이용이 가능한 것을 염두해두자.
2 접속하기
설치한 프로그램 중 Xstart를 시작한다. 우측에 새로만들기 클릭 후, 세션에 적당한 이름을 입력한다. 호스트는 접속하고자는 서버, 프로토콜은 SSH, 그리고 사용자 아이디와 비밀번호를 입력한다.
astro10의 경우에는 실행명령 창에 옆에 우측 화살표를 클릭 후, 리눅스를 선택한 다음 [그림 A.1]과 같이 xterm 뒷부분만 남기고 앞부분은 삭제한다.
실행을 누르면 서버에 접속할 수 있다.
  - 21 -


## 파일 전송하기
설치한 프로그램 중 Xftp를 시작한다. [그림 A.2]와 같이 세션에 적당한 이름을 적고 호스트 에는 접속하고자는 서버를 적는다. 프로토콜은 SFTP, 인증방법은 Password 방식에, 본인의 아이디와 비밀번호를 입력한다.
3) VNC 설정하기
VNC는 처음 실행해주기 전까지 과정이 복잡하지만, 실행 후에는 ‘원격 데스크탑 연결’과 유 사하게 그래픽 형태로 서버의 화면을 띄워주기 때문에 유용하다.


서버 설정하기
여기서는 서버에 VNC가 설치되어, 기본적인 세팅이 완료되어 있는 상태에, 새로운 사용자를 추가하는 방법에 대해서 설명한다. 개인 컴퓨터를 서버로 사용하기 위해 처음부터 VNC를 설 치하길 원하면 인터넷에 검색해서 찾아보자.
우선, 서버상의 VNC에, VNC를 사용할 사용자 계정과 포트를 등록하자. su 명령어로 관리자 로 접속한 다음,
# vi /etc/sysconfig/vncserver
이후, 파일 아래에 포트와 아이디를 추가해준다. 이 때 포트는 기존 사용자들과 겹치지 않는 번호를 입력한다. 가령, 3번 포트에 khpark을 추가해주고 싶다면
VNCSERVERS="3:khpark" VNCSERVERARGS[3]="-geometry 800x600“

두 줄을 추가하자.
그 다음, 다른 터미널에서, 자기 계정 아래 .vnc 폴더를 만들고, 비밀번호를 설정하자
$ cd ~
$ mkdir .vnc $ cd .vnc
$ vncpasswd
마지막으로, 방화벽을 풀어주자. 관리자 권한으로
# iptables -I INPUT 6 -p tcp -m state --state NEW -m tcp --dport 5903 –j ACCEPT # service network restart
# /etc/init.d/vncserver start
을 입력해주자. 이 때, 포트번호는 5900+(자기가 앞서 입력한 포트번호)이다.
자, 이제 준비는 끝났다. 인터넷에서 VNC Viewer 파일을 구한 후, 실행하자. 접속하기에서, (서버명 혹은 IP) : (포트번호)를 입력해서 실행해주자.
 [그림 A.3] VNC Viewer 실행창

### 관리자 권한 활용하기
개인 컴퓨터나, 서버 관리자를 맡게 되면 프로그램 설치 등의 사유로 관리자 권한을 활용할 수 있다. 관리자 권한을 사용하는 방법으로는 su(substitute)와, sudo 두 가지가 있다.
sudo는 계정을 바꾸지 않은 상태서 관리자 권한으로 명령을 실행하는 것이다. 가령 일반 계 정으로 cat /etc/sudoers 라는 명령을 내리면 권한 문제로 기각된다. 하지만 관리자 비밀번호를 알고 있다면 sudo cat /etc/sudoers라는 명령으로 해당 파일을 열람할 수 있다. 단, 이 명령을 수행하기 이전에 /etc/sudoers 파일에 아래와 같은 부분에 계정을 추가해주어야 한다.
$ su (아이디) ; 해당 아이디로 계정을 전환. 입력하지 않으면 관리자 계정 으로 전환한다.

## Allow root to run any commands anywhere
root ALL=(ALL) wskang ALL=(ALL) lingo ALL=(ALL) khpark ALL=(ALL)
ALL
NOPASSWD: ALL
ALL
NOPASSWD: ALL
lingo라는 사용자는 sudo라는 명령을 사용할 수 있지만, 사용할 때마다 비밀번호를 확인하게 되고, wskang과 khpark 사용자는 비밀번호 확인을 생략하고 sudo를 실행 할 수 있다.
- 24 -
