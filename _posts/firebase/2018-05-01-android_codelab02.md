---
title: "Firebase - Android Codelab (2): Firebase console"
date: 2018-05-01 11:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에 있는 Start와 Authentication을 정리했다.

## Firebase console Project

Firebase console 에서 프로젝트를 생성한다. 앱의 해시 값을 Firebase project에 등록해 준 후에, 생성한 `google-service.json` 파일을 다운로드해서 앱 프로젝트의 모듈에 복사한다.


### Android 프로젝트에서 Firebase 추가

Firebase console[console](https://console.firebase.google.com/u/1/) 에서 프로젝트를 생성하고 프로젝트의 그래들에 SDK를 추가하면 된다.

1. 새 프로젝트를 생성하고
2. 안드로이드 앱 등록 : 패키지 이름 및 SHA1 hash 등록
3. 구성 파일 다운로드: google-service.json
4. Firebase SDK 추가


#### Firebase 프로젝트 생성

Firebase console 로 들어간다.

![](/images/google/firebase-start.png){:width="500"}

새 프로젝트를 생성한다.

![](/images/google/firebase-start-02newproject.png){:width="400"}


#### 안드로이드 앱 등록

앱을 추가한다.

![](/images/google/firebase-start-04.png){:width="500"}

안드로이드 앱 프로젝트 정보를 등록한다.

![](/images/google/firebase-start-05android.png){:width="500"}

SHA1 해시 값을 얻는다. 기본 debug.keystore 를 사용하면 다음 같다.

```terminal
$ keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore -list -v -storepass android
```

#### google-service.json

서비스 파일을 다운로드해서 앱에 등록한다.

![](/images/google/firebase-start-06json.png){:width="500"}


##### Firebase SDK 추가

안드로이드 앱 프로젝트의 그래들에 Firebase SDK plugin을 추가한다.

프로젝트 수준의 build.gradle (<project>/build.gradle):

```gradle
buildscript {
  dependencies {
    // Add this line
    classpath 'com.google.gms:google-services:3.2.0'
  }
}
```


앱 수준의 **build.gradle** (<project>/<app-module>/build.gradle):

```gradle
dependencies {
  // Add this line
  compile 'com.google.firebase:firebase-core:15.0.0'
}


// Add to the bottom of the file
apply plugin: 'com.google.gms.google-services'
```

마지막으로 IDE의 표시줄에 있는 '지금 동기화'를 누르세요.

#### 앱 실행

콘솔에서 Firebase SDK 추가 단계 다음을 진행하면 안드로이드 앱을 실행하기를 요구한다. 

![](/images/google/firebase-start-08.png){:width="250"}


앱이 실행되고 제대로 Firebase 프로젝트가 등록되면 성공 메시지가 나온다.

![](/images/google/firebase-start-07ok.png){:width="500"}


#### Google play service 업그레이드

*com.google.gms:google-services* 를 사용할 때 단말의 구글 플레이 버전이 낮다면, 앱 실행시 아래 같은 Google play service upgrade 요청 메시지를 보게 된다.

![](/images/google/update-googleplayservice1.png){:width="500"}

그리고 Authentication 등의 서비스 이용을 요청시 Notification으로 업그레이드 실행을 요청받는다.

![](/images/google/update-googleplayservice2.png){:width="500"}

업그레이드를 실행하면 서비스 앱을 업그레이드한다.

![](/images/google/update-googleplayservice3.png){:width="500"}


---

## 참조

{% include_relative firebase_series.md %}

