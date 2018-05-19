---
title: Linux - find usages
date: 2017-04-05 09:00:00 +0900
layout: single
tags: ["linux"]
categories: [Linux, Programming]
toc: true
sidebar:
  nav: "linux"
---

`find` 명령에서 자주 사용하는 쓰임새를 요약했다.

> 2018-05: rm, i-node 내용 추가


## find 명령

### find 명령 요약

주어진 이름으로 찾아 화면에 출력한다. `-name` 은 대소문자 구분한 이름을 준다.

```terminal
find ./ -name '*.xml' -print
```

주어지는 이름의 패턴은 *\*?* 를 사용할 수 있다.


찾은 결과를 받아 명령의 입력으로 실행할 수 있다. 다음은 현재 디렉토리 밑에서 .c 파일을 찾아 `md5sum` 으로 해시 값을 출력한다. `-iname`은 대소문자 구분을 하지 않는다.


```terminal
find -iname "*.c" -exec md5sum {} \;
d41d8cd98f00b204e9800998ecf8427e  ./mycprogram.c
```


검색시 탐색 깊이는 `-maxdepth` 혹은 `-mindepth`를 사용할 수 있다.

```terminal
find -maxdepth 2 -iname "*.c" -exec md5sum {} \;
```


어떤 파일을 제외한 것만 찾을 수 있다:

```terminal
find -maxdepth 1 -not -iname "mycprogram.c"
```


파일의 퍼미션으로 찾을 수 있다.

```terminal
find . -perm -g=r -type f -exec ls -l {} \;
find . -perm g=r -type f -exec ls -l {} \;
find . -perm 040 -type f -exec ls -l {} \;
```

찾은 후 삭제하기:

```terminal
find ./ -name 'Debug' -exec rm -rf {} \;
```


find 명령으로 i-node를 통해서 지우기:

아래 처럼 특수문자로 "~" or "a b c" 등의 이상한 파일이 있을 경우 inode를 확인해 삭제에 유용하다.

```terminal
$ ls -i
$ 32471 a b c  
$ find . -inum 32471 -exec rm -rf {} ';'
$ find . -inum 32471 -exec rm -rf {} \;
```



### 회피문자 파일 이름 삭제하기

*????* 같이 지워지지 안는 파일 같은 경우도 i-node로 삭제할 수 있다.

```terminal
~$ ls -ali
~$ ls -ali
total 2068
42467329 drwxr-xr-x 11 qkboo qkboo    4096 Jun 20 22:54 .
       2 drwxr-xr-x 10 qkboo qkboo    4096 Jun  1 12:56 ..
42467482 drwxr-xr-x  2 qkboo qkboo   12288 Jul  2  2016 .Picasa3Temp
42598444 drwxrwxrwx  3 qkboo qkboo    4096 Mar 29 23:45 ??????
42475521 drwxr-xr-x  4 qkboo qkboo    4096 Jun 20 22:52 Design_Assets
42467936 drwxr-xr-x 14 qkboo qkboo    4096 Mar  4 16:39 Incoming

~$ find . -inum 42598444 -exec rm {} \;
```



### 파일 형식으로 검색

옵션 `-type` 은 파일 형식으로 찾을 수 있다. 파일 형식은:

>  b       block special
>  c       character special
>  d       directory
>  f       regular file
>  l       symbolic link
>  p       FIFO
>  s       socket


일반 파일

```terminal
find . -type f
```

소켓 형식의 파일

```terminal
find . -type s
```


디렉토리 형식

```terminal
find . -type d
```

숨겨진 파일만 검색도 가능하다.

```terminal
find . -type f -name ".*"
```

역시 숨겨진 디렉토리만 찾을 수 도 있다.

```terminal
find -type d -name ".*"
```


### 파일 크기로 검색

옵션 `-size` 를 사용해서 파일의 크기로 찾을 수 있다. 

아래는 어떤 크기 보다 크거나, 작은 파일을 찾아 준다.

```terminal
find -size +100M     # 보다 큰 파일
find -size -100M     # 보다 작은 파일
find -size 100M      # 같은 크기의 파일
```

다음 같이 응용해 볼 수 있다. `100MB` 보다 큰 파일을 찾아 삭제한다:

```terminal
find / -type f -name *.zip -size +100M -exec rm -i {} \;
```


### 파일의 수정된 시간을 기준

모든 파일의 수정된 시간 정보를 알 수 있다. test_1.txt의 시간을 기준으로 검색해 보자.

```terminal
ls -lrt test_1.txt
-rw-r--r-- 1 gtko gtko 0 2011-02-01 02:26 test_1.txt
```

옵션 `-newer` 에 대상 파일을 주면 해당 파일을 생성한 날짜 이후의 결과만을 표시하게 된다.

```terminal
find -newer test_1.txt
.
./dir2
./dir2/file2
./dir2/file3
```


#### 자주 사용할 만한 find 명령

유용한 find 명령들 alias로 만들어 사용하기도 한다.

a.out 인 파일 지우기

```terminal
alias rmao="find . -iname a.out -exec rm {} \;"
```

c프로그램의 core 파일

```terminal
alias rmc="find . -iname core -exec rm {} \;"
```

큰 파일 삭제...

```terminal
alias rm100m="find / -type f -name *.tar -size +100M -exec rm -i {} \;"
alias rm1g="find / -type f -name *.tar -size +1G -exec rm -i {} \;"
alias rm2g="find / -type f -name *.tar -size +2G -exec rm -i {} \;"
alias rm5g="find / -type f -name *.tar -size +5G -exec rm -i {} \;"
```


### iconv 와 결합

`iconv`로 파일 인코딩을 변환할 수 있는데, 많은 파일을 한번에 처리하기 위해서 find와 결합해 찾은 모든 파일의 파일 인코딩을 변환할 수 있다.

다음은 .c 파일을 찾아 인코딩을 euc-kr에서 utf-8로 변환하는 명령이다.

```terminal
find ./ -name '*.c' -exec iconv -feuc-kr -tutf-8 {} -o {} \;
```

find 를 사용하지 않는다면, 디렉토리 안에 있는 모든 파일의 인코딩을 변환하고자 할 때는 shell 조건문과 섞어서 사용할 수 있다.

```terminal
$ for F in './*.sql'; do iconv -c -feuc-kr -tutf-8 $F -o $F; done
```


## 참조

[Linux find command examples](http://www.thegeekstuff.com/2009/03/15-practical-linux-find-command-examples/)

