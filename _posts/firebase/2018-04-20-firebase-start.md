---
title: "Firebase - Getting Started"
date: 2018-04-20 09:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase](https://firebase.google.com/docs/)는  고품질 앱을 개발하는 기본적인 도구를 제공해서 앱 개발에 필요한 기초적인 기능을 쉽게 추가할 수 있기 때문에 앱 개발자는 비즈니스 로직에 더 집중할 수 있게 된다.


## Firebase

앱 개발 및 테스트를 위해 다음 같은 Firebase 제품을 즉시 사용할 수 있다:

 - gmp_database: 실시간 데이터베이스
 - gmp_firestore: Cloud Firestore
 - gmp_auth: 인증
 - gmp_storage: Cloud Storage
 - gmp_test_lab: Test Lab
 - gmp_performance: 성능 모니터링
 - gmp_crashlytics: Crashlytics
 - gmp_crash: 오류 보고
 - gmp_functions: Cloud 함수
 - gmp_hosting: 호스팅


앱에 Firebase 기능 추가:

 - 애널리틱스로 사용자 행동을 파악합니다.
 - 인증으로 사용자 인증을 설정합니다.
 - 실시간 데이터베이스로 사용자 정보를 저장하거나 Cloud Storage로 BLOB 데이터를 저장합니다.
 - 클라우드 메시징으로 사용자에게 알림을 보냅니다.
 - 오류 보고로 앱이 다운된 시점과 이유를 조사합니다.

### Firebase 

요금제 

https://firebase.google.com/pricing/?authuser=1




### Android 프로젝트에 Firebase 추가

사용 조건:
 - Android 4.0(Ice Cream Sandwich) 이상 및 Google Play 서비스 11.8.0 이상을 실행하는 기기
 - Android Studio 최신 버전


이런 서비스를 [console](https://console.firebase.google.com/u/1/) 에서 프로젝트를 생성하고 프로젝트의 그래들에 SDK를 추가하면 된다.

#### SDK 추가

프로젝트 루트 build.gradle 에 google-services 플러그인 과 Google maven 저장소를 추가한다.

```json
buildscript {
    // ...
    dependencies {
      classpath 'com.google.gms:google-services:3.2.0' // google-services plugin
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}
```

앱 모듈의 build.gradle 에 `apply plugin` 으로 Firebase 제품 플러그인을 활성화 한다.

```json
apply plugin: 'com.android.application'

android {
  // ...
}

dependencies {
  // ...
}

// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```

이제 파이어베이스 의존성 라이브러리를 추가하고 사용하면 된다.

#### 파이어베이스 라이브러리

다양한 Firebase에서 라이브러리를 사용해서 앱에 필요한 기능을 추가 할 수 있다.

- `com.google.firebase:firebase-core`      애널리틱스
- `com.google.firebase:firebase-database` :  실시간 데이터베이스
- `com.google.firebase:firebase-firestore`: Cloud Firestore
- `com.google.firebase:firebase-storage`:   저장소
- `com.google.firebase:firebase-crash`:     오류 보고
- `com.google.firebase:firebase-auth`: 인증
- `com.google.firebase:firebase-messaging`:  클라우드 메시징
- `com.google.firebase:firebase-config`:   원격 구성
- `com.google.firebase:firebase-invites`:   초대 및 동적 링크
- `com.google.firebase:firebase-ads`: AdMob
- `com.google.firebase:firebase-appindexing`:  앱 색인 생성
- `com.google.firebase:firebase-perf`: 성능 모니터링


파이어베이스 라이브러리는 앞서 선언한 그래들 파일에 사용하려는 파이어베이스 라이브러리를 그래들 의존성 *dependencies*에 추가해 주면 된다.

```json
apply plugin: 'com.android.application'

android {
  // ...
}

dependencies {
  // ...
  implementation 'com.google.firebase:firebase-core:15.0.0'
  implementation 'com.google.firebase:firebase-auth:15.0.0'
  implementation 'com.google.firebase:firebase-storage:15.0.0'
}

// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```




---

## 참조

{% include_relative firebase_series.md %}

