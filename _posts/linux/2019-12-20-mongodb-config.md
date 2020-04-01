---
title: MongoDB 구성 설정과 사용자 인증 사용
date: 2019-12-20 12:00:00 +0900
layout: post
description: "mongdb community edition 에서 사용하는 mongod.conf 를 구성하고 설정한 내용을 정리했다."
tags:
  [
    linux,
    mongodb,
    armbian,
    odroid-c2,
    ubuntu,
    debian,
    arm64,
    amd64,
    "우분투",
    "데이안",
  ]
categories:
  - Linux
  - Database
---

MongoDB에 관련 글

{% include_relative _inc_mongodb_series.md %}

---

<br>

# Mongo Database 설정과 사용자 인증 사용


이 문서는 **MongoDB Community Edition** 를 리눅스 플랫폼에서 시작과 운영에 필요한 구성 파일 `mongod.conf` 의 설정을 다루고 있다. 이를 통해 데이터베이스 서비스를 동작시키고, 인증한 사용자로서 클라이언트 프로그램으로 접속하기 위한 **기본 인증방식을 구성**하는 것을 정리하고 있다.

1. [Mongo Database 설정](#Mongo-Database-설정)
2. 기본인증 구성

> 이 글의 내용은 MongoDB Community Edition 3.6 이후 4.2 까지 사용이 가능하다.

<br/>

## Mongo Database 설정

우분투/데비안 계열은 설치시 `mongod` 데몬이 데이터베이스 운영에 필요한 여러 구성에 대한 설정을 위해서 `/etc/mongod.conf` 구성 파일을 사용한다. 구성 파일에 데이터베이스 구성에 필요한 여러 설정을 지정할 수 있다.

- mongodb 사용자와 디렉토리 퍼미션 확인
- 외부 접속 허용 여부
- mongo client 접속 테스트
- mongodb 인증

이제 실제 파일에 구성 내용을 설정하는데, 여기서는 mongod 의 기본인증 방법과 데이터 저장소에 대해서만 다룬다.

### mongod 설정

MongoDB의 `systemd` 서비스는 보통 데이터베이스 구성 파일 `/etc/mongod.conf` 을 참조한다. mongod 서비스 상태를 확인해 보면 `--config /etc/mongod.conf` 옵션으로 구성 파일을 지시하고 있다.

```bash
$ systemctl status mongod.service
* mongod.service - MongoDB Database Server
   Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-12-19 15:38:14 UTC; 3 weeks 6 days ago
     Docs: https://docs.mongodb.org/manual
 Main PID: 2051 (mongod)
   Memory: 191.3M
   CGroup: /system.slice/mongod.service
           `-2051 /usr/bin/mongod --config /etc/mongod.conf
```

mongod.conf 파일을 살펴보자,

#### mongod.conf 파일

먼저 `/etc/mongod.conf` 파일에 **인증**을 제외한 **데이터 디렉토리**, **bindIp**, **로그** 부분만 설정한다.

 - storage: 데이터 베이스 파일 저장 위치 및 방법
     - **dbPath** : 데이터베이스 스토리지 위치
 - systemlog: 로그 파일 및 로그 조작 방법
 - net: 네트워크 관련
    - **bindIp**: 서버 외에서 mongo 클라이언트가 접근하려면 IP 를 입력한다.

먼저 **인증을 비활성화** 한 구성을 하는데, 다음은 기본적인 mongod.conf 를 구성한 내용이다.

```conf
storage:
  dbPath: /data/mongodata/
  journal:
    enabled: true

systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

processManagement:
   fork: true

net:
  port: 27017
  bindIp: 127.0.0.1,192.168.0.2
```

이제 `mongo.conf` 구성 파일이 준비되었으면 systemctl 로 mongod 데몬을 재시작한다..


```sh
$ sudo systemctl restart mongod.service
$ sudo systemctl status mongod.service
```

간단한 구성 설정, 아직은 인증이 비활성화된 상태를 마련 했으면 사용자 인증을 통한 원격 접속을 살펴보자,

---

<br>


## mongo 기본인증

보통 mongo 클라이언트 등에서 MongoDatabase 를 사용하기 위해서 클라이언트를 위한 인증을 제공해야 한다. 클라이언트를 위한 인증은 관리용 데이터베이스에서 데이터베이스 사용자를 등록하고 클라이언트가 등록한 사용자로 데이터베이스에 접속하는 방법을 사용한다.

인증을 추가하기 위해서 mongod의 `mongo.conf` 설정 파일에 접근 제어를 하지 않는 상태에서 **mongo** 클라이언트로 접속해서 기본인증을 위한 데이터베이스 사용자를 추가해 준다.


### Admin 사용자 생성

`mongod`에 인증이 비활성화 상태에서 mongo 클라이언트로 접속에 성공하면 **`>`** 프롬프트가 나온 후 use 명령으로 _admin_ 데이터베이스로 전환한다.

```bash
$ mongo
>
> use admin
switched to db admin
>
```

1). _admin_ 데이터베이스에서 관리자 role을 가진 사용자를 추가하고, 2). 사용할 데이터베이스의 사용자와 접근 제어를 추가해서 사용하기 위해서 작업한다.

#### user administrator

_admin_ 데이터베이스에 `userAdmin role` 혹은 `userAdminAnyDatabase role`을 가진 사용자 만든다. 

다음은 _admin_ 데이터베이스에서 사용자 **`admin`**  계정를 생성하고 있다.

```js
> use admin
switched to db admin
> 
> db.createUser(
  {
    user:'admin',
    pwd:'****',
    roles:['userAdminAnyDatabase']
  }
)
Successfully added user: { "user" : "admin", "roles" : [ "userAdminAnyDatabase" ] }
>
> db.getUsers()
```

사용자 패스워드 변경은 `.changeUserPassword()` 메서드를 사용한다.

```js
> db.changeUserPassword("accountUser", "SOh3TbYhx8ypJPxmt1oOfL")
```

사용자의 `role` 변경은 `.grantRolesToUser()` 메서드를 사용한다.

```js
> db.grantRolesToUser( 'admin', [{role: 'userAdmin', db:'admin'}])
```

혹은 `updateUser()` 를 사용할 수 있다:

```
db.updateUser( "appClient01", ...
```

현재 데이터베이스의 사용자를 출력한다.

```js
> db.getUsers()
[
  {
    "_id" : "admin.admin",
    "user" : "admin",
    "db" : "admin",
    "roles" : [
      {
        "role" : "userAdmin",
        "db" : "admin"
      },
      {
        "role" : "userAdminAnyDatabase",
        "db" : "admin"
      }
    ]
  }
]
```

admin 데이터베이스에 새로 생성한 admin 사용자로 로그인을 해보자. 그러기 위해서는 admin 데이터베이스에 인증 로그인을 한 후에 데이터 베이스를 생성하거나 사용할 수 있다.

<br>

## 인증 활성화

**`mongod.conf`** 파일 안의 security 섹션에서 `security.authorization` 을 활성화 한다

```conf
security:
  authorization: enabled
```

> 인증 활성화는 MongoDB v2.4이전에는 인증모드를 명령행의 `--auth` 옵션을 사용하고, v2.6 이후에서 _mongod.conf_ 파일 사용할 때 `security.authorization` 를 활성화 하고 있다.

systemd 로 서비스를 재시작 한다.

```terminal
$ sudo systemctl restart mongod.service
$ sudo systemctl status mongod.service
```

이렇게 인증모드로 데이터베이스를 시작하면 인증 정보로 로그인해야 한다. 아래 같이 인증 정보없이 로그인 하면 데이터베이스 사용시 에러를 만난다.

```terminal
$ mongo
> show dbs;
Tue Sep 27 23:22:40.683 listDatabases failed:{ "ok" : 0, "errmsg" : "unauthorized" } at src/mongo/shell/mongo.js:46
>
> show users
Tue Sep 27 23:22:44.667 error: { "$err" : "not authorized for query on test.system.users", "code" : 16550 } at src/mongo/shell/query.js:128
```

이제 인증 모드에서 데이터베이스에 접속해야 한다. 

### 인증모드로 접속

데이터베이스 시스템에 접근제어가 활성화 되서 인증모드로 `mongo` 클라이언트 접속할 때는 옵션으로 `-u <username>, -p <password>` 와 `--authenticationDatabase <database>` 를 지정해 주어야 한다.

앞서 생성해둔 admin 계정으로 접속해 보자,

```sh
$ mongo --port 27017 -u "admin" -p "****" --authenticationDatabase "admin"
MongoDB shell version v4.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.2
>
```

MongoDatabae는 데이터베이스 마다 인증한 사용자를 통해 조작을 하게 된다. 그래서 인증모드로 접속 되었지만, 어떤 데이터베이스에 비인증된 사용자로 접근하는 경우는 에러가 발생한다. 예를 들어서 admin 사용자로 인증 로그한 mongodb shell에서  아래 같이 비인증인 student 데이터베이스에 접근하면 에러가 난다:

```terminal
>
> use students
switched to db students
> show users
2018-06-25T18:30:38.174+0900 E QUERY    [thread1] Error: not authorized on students to execute command { usersInfo: 1.0, $db: "students" } :
```

#### mongo 비인증 접근시

위와 같이 로컬에서 mongo 클라이언트 접속하는 것 외에 외부에서 mongodb로 접근시 authentication을 적용한 상태라면 다음과 같은 URL로 접근할 수 있다:

> "username:password@HOST_NAME/mydb"

그러나 외부접근시 클라이언트 버전과 서버의 Credential 버전이 맞지 않은 경우 다음 같이 실패 메시지를 확인할 수 있다.

```
> 2016-05-16T00:53:10.338+0900 I ACCESS [conn2] Failed to authenticate student@student with mechanism MONGODB-CR: AuthenticationFailed: MONGODB-CR credentials missing in the user document
> 2016-05-16T00:53:10.352+0900 I NETWORK [conn2] end connection 220.121.140.59:51634 (0 connections now open)
```

<br/>
<br/>

## `mongod` 명령 사용

보통 디버깅 목적으로 사용하기 위해서 `systemd`가 아닌 명령행에서 `mongod` 명령으로 로 MongoDB를 시작해 설정 파일 등이 제대로 동작하는지 확인할 수 있다. 

> 다만 데이터베이스가 생성하는 데이터와 로그 파일의 접근권한이 실행한 사용자 계정으로 처리되어 퍼미션 문제가 발생할 수 있다.


#### mongoDB v2.6 이후

앞서 시작한 명령행 **mongodb**를 종료하고 명령라인에서 재시작 `--auth` 옵션을 붙여 시작한다.

```terminal
$ mongod --auth --port 27017 --dbpath /data/mongodata
```

접근제어 `--auth` 옵션으로 데이터베이스를 시작하면 로그인시 `-u <username>, -p <password>` 와 `--authenticationDatabase <database>` 를 지정해 주어야 한다.

```terminal
$ mongo --port 27017 -u "admin" -p "****" --authenticationDatabase "admin"
MongoDB shell version v3.4.0
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.0
>
```


---

이전 버전의 mongod 명령

#### mongoDB v2.4 인증모드로 시작

``mongod` 명령라인으로 시작할 수 있다.

```terminal
$ sudo mongod --port 27017 --dbpath /data/mongodata
```

mongoDB v2.4는 다음 같이 인증 모드로 시작한다. `mongod` 명령라인에서 `--auth` 옵션을 붙여 DB 인스턴스(`mongod`)를 시작 혹은 재시작한다.

```terminal
$ mongod --auth --port 27017 --dbpath /data/db1
```

혹은 _mongod.conf_ 설정 파일에서 **auth** 를 활성화 한다.

```
auth = true
```



## 참조

[^1]: [Install mongodb on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition)
