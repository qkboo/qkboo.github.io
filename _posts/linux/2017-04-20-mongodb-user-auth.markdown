---
title: MongoDB - Database와 User Authentication
date: 2017-04-20 09:00:00 +0900
layout: post
description: "ubuntu, debian에 mongodb 3을 설치하고 데이터베이스 생성 및 사용자 인증을 다루고 있다"
tags: [linux, mongodb, armbian, odroid-c2, ubuntu, debian, arm64, amd64, "우분투", "데비안"]
categories:
- Linux
- Database
---

> 2018-06-21 설치 링크로 대체
{:.right-history}

MongoDB 설치후 데이터베이스 위치, 로그, 인증 등에 관련한 서버 구성과 설정을 정리한다. 

> MongoDB 2.6 과 MongoDB Community Edition 3.x 버전을 사용했다.

## mongoDB 접근제어

mongoDB 는 설치과정 중에 인증과 관련해 설정하는 부분이 없어서 설치 후 누구나 DB에 접속 할 수 있다.  인증을 추가해 데이터베이스 관리자와 데이터베이스 사용자로 구분해서 이용하고, 각 데이터베이스의 사용자는 허가된 역할(Role)을 가지고 데이터베이스에 접근 가능하도록 구성한다.

여기서는 다음 두 가지를 다루고 있다.

- **(1) 데이터베이스 관리자 추가** 
- **(2) 데이터베이스 사용자 추가**

Ubuntu/Debian 리눅스 배포본에 MongoDB 3.x 버전이 지원되지 않으면, MongoDB Community Edition 를 패키지 혹은 소스로 설치할 수 있다. 

 - [MongoDB Community Edition 3.4 on Armv8]({% post_url /linux/2017-04-11-mongodb-3.4-install-armv8 %})
 - [MongoDB Community Edition 3.6]({% post_url /linux/2018-06-08-mongodb-3.6-install %})


### 데이터베이스 관리자

mongod가 **비인증 모드로 실행**중인 상태에서, `mongo` 클라이언트로 데이터베이스에 접속한다.접속에 성공하면 **>** 프롬프트가 표시된다. 그리고 접속한 후에 *admin* 데이터베이스로 전환한다.

```terminal
> use admin
switched to db admin
>
```

`mongo` 클라이언트로 접속해 mongoDB 데이터베이스 관리자 **admin** 추가해서, 사용자 롤로 `userAdminAnyDatabase` 롤을 추가해준다.

#### mongoDB 2.6 이후 관리자 계정 추가

mongoDB 2.6 이후는 `db.createUser()` 로 사용자를 추가한다. [^2]
다음은 admin 데이터베이스에서 사용자를 관리하는 admin 계정을 생성하고 있다.

```terminal
>
> db.createUser({ user:'admin', 
                   pwd:'****', 
                   roles:['userAdminAnyDatabase']
               })
Successfully added user: { "user" : "admin", "roles" : [ "userAdminAnyDatabase" ] }
>
> db.getUsers() // 데이터베이스 사용자 확인
[
    {
        "_id" : "admin.admin",
        "user" : "admin",
        "db" : "admin",
        "roles" : [
            {
                "role" : "userAdminAnyDatabase",
                "db" : "admin"
            }
        ]
    }
]
```


[^2]: [Enable Authentication after Creating the User Administrator(v2.6)](https://docs.mongodb.com/v2.6/tutorial/enable-authentication-without-bypass/)

#### mongoDB 2.4 이전 관리자 계정 추가

mongoDB 2.4 까지는 새로운 사용자는  `db.addUser()` 로 추가한다.[^1]

```terminal
$ mongo     // mongo client 로 접속
>use admin  // admin DB 사용
>db.addUser( { user: "<username>", // admin name
              pwd: "<password>",
              roles: [ "userAdminAnyDatabase" ] // Database role
          } )
```

> mongoDB 2.6까지 32bit 버전을 지원하고 있다.

[^1]: [Add User Administrator(v2.4 )](https://docs.mongodb.com/v2.4/tutorial/add-user-administrator/)


관리자 계정을 만든후 MongoDB에 `mongo` 클라이언트로 인증 로그인을 한 후에 데이터베이스를 생성하고 해당 데이터베이스 사용자에 접근 권한을 추가해 준다.




#### `security.authorization`

**mongodb.conf** 파일에 `security.authorization` 을 활성화 한다

```
security:
  authorization: enabled
```



### 데이터 베이스 생성과 롤 기반 인증



#### 관리자 로그인

이제 데이터베이스 관리자 계정으로 로그인해서 사용하려는 데이터베이스를 `use`로 선택하고 해당 데이터베이스 사용자를 추가해준다.

`mongo` 클라이언트 로그인시 `-u <username>`, `-p <password>` 와 `--authenticationDatabase <database>` 를 지정해 주어야 한다.

```sh
$ mongo --port 27017 -u "admin" -p "****" --authenticationDatabase "admin"
MongoDB shell version v3.4.0
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.0
```

혹은 인증없이 `mongo` 클라이언트로 데이터베이스에 접속한 후에 `db.auth()` 명령을 사용할 수 있다.

```json
> use admin
switched to db admin
> db.auth("admin", "****")
1
> show users
{
    "_id" : ObjectId("5733676238ac1ddf4cf745c2"),
    "user" : "admin",
    "readOnly" : false,
    "pwd" : "24413999168dccff96dcc735720c85ce"
}
>
```

이제 각 데이터베이스에 사용자를 생성해서 사용해서 인증한 사용자만 데이터베이스를 사용하게 할 수 있다. 


### Database 사용자 추가


#### mongoDB v3.x 사용자 관리

mongoDB v2.6 히우는 대부분 mongoDB v3.4와 호환되는 사용자 관리 명령을 사용한다. 여기서는 [User Management Methods (v3.4)](https://docs.mongodb.com/manual/reference/method/js-user-management/)를 참고하고 있다.


| Name | Description |
| ----------------- | --------------------------------------|
| db.auth() | 데이터베이스에 사용자 인증 |
| db.createUser() | Creates a new user. |
| db.updateUser() | Updates user data. |
| db.changeUserPassword() | 사용자 패스워드 변경 |
| db.dropAllUsers() | 데이터베이스에 관련된 모든 사용자를 삭제한다. |
| db.dropUser() | 한 사용자를 삭제한다 |
| db.grantRolesToUser() | 롤과 권한을 사용자에 허용한다 |
| db.revokeRolesFromUser() | 사용자에 부여한 롤을 삭제한다 |
| db.getUser() | 지정한 사용자의 정보를 반환한다|
| db.getUsers() | 데이터베이스에 관련된 모든 사용자의 정보를 반환한다 |


#### createUser()

`db.createUser()` 는 두 개의 도큐멘트를 인자로 사용한다:

> db.createUser(user, writeConcern)

여기서 **user** 도큐멘트는 아래 같은 형식을 갖는다:

```json
{ user: "<name>",
  pwd: "<cleartext password>",
  customData: { <any information> },
  roles: [
    { role: "<role>", db: "<database>" } | "<role>",
    ...
  ]
}
```
 - **customData**: 선택적으로 추가할 정보를 담은 도큐멘트.

다음은 *product* 데이터베이스로 전환해서 *product* 데이터베이스 사용자를 추가하고 있다. *customeData* 를 주목하자.

```terminal
> use products
db.createUser( { user: "user1",
                 pwd: "changeMe",
                 customData: { employeeId: 12345 }, // prducts 
                 roles: [ { role: "clusterAdmin", db: "admin" },
                          { role: "readAnyDatabase", db: "admin" },
                          "readWrite"] },
               { w: "majority" , wtimeout: 5000 } )
```

다양한 사례는 [createUser() Exmaple](https://docs.mongodb.com/manual/reference/method/db.createUser/#examples) 를 참고하자.



mongoDB v2.4는 사용자 추가 방법이 조금 다르다.

#### mongoDB v2.4 사용자 추가

사용하려는 데이터베이스의 계정으로 접근제어를 추가해 주어야 한다. mongoDB 2.4 까지는 새로운 사용자는  `db.addUser()` 로 추가한다.[^3]


[^3]: [Add User To Database(v2.4)](https://docs.mongodb.com/v2.4/tutorial/add-user-to-database/)

아래는 데이터베이스 관리자 계정 *admin*으로 로그인해서, 필요하면 students 데이터베이스를 생성합니다. 그리고 students 데이터베이스 사용자 *student* 계정을 추가하고 있다.

```js
> use students
switched to db students
> db.addUser('student', '****')
{
    "user" : "student",
    "readOnly" : false,
    "pwd" : "7a70591507db46bdd3df47a213d8922f",
    "_id" : ObjectId("57336a71c1ef2bed6688d296")
}
> db.auth('student', '012345')
1
> db.student.save({name:'qkboo', class:'Database', grade:'A'})
> db.student.find()
{ "_id" : ObjectId("57336b7d1be9091521cbeb36"), "name" : "qkboo", "class" : "IoT", "grade" : "A" }
>
```

이제 `mongo` 클라이언트로 생성한 *students* 에 데이터베이스 사용자 *student*로 로그인한다. 

```sh
$ mongo student -u student -p ****
MongoDB shell version: 2.4.10
connecting to: student
```

만약 해당 데이터베이스 사용자가 아닌 계정에서 데이터베이스 접근시 다음 같이 인증되지 않은 접근으로 에러를 발생한다.

```js
$ mongo
MongoDB shell version: 2.4.10
connecting to: test
> use student
switched to db student
> show dbs
Tue Sep 27 23:45:38.269 listDatabases failed:{ "ok" : 0, "errmsg" : "unauthorized" } at src/mongo/shell/mongo.js:46
>
```


## MongoDB에 관련 글

{% include_relative _inc_mongodb_series.md %}
