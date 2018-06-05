---
title: "Firebase - Android Codelab (12): 크래시 보고"
date: 2018-05-04 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 튜토리얼 중에서 **15. Report Crashes** 을 한국어로 정리/요약 했다. 


## 크래시 보고

Firebase Crashlytics는 앱이 크래시 발생과 크래시를 이끈 이벤트 로그를 보고할 수 있도록 한다.


### 파이어베이스 크래시 의존성 추가

앱의 모듈 `app/build.gradle` 파일에 `crashlytics` 의존성을 추가한다.

*app/build.gradle* 파일:

```gradle
apply plugin: 'com.android.application'
apply plugin: 'io.fabric'

dependencies {
    // ...
  implementation 'com.crashlytics.sdk.android:crashlytics:2.7.1'
}
```

그리고 Fabric maven repository 의존성이 프로젝트 `build.gradle`에 있는지 확인한다..

*project/build.gradle*

```gradle
buildscript {
    repositories {
        // ...
        maven {
           url 'https://maven.fabric.io/public'
        }
    }
    dependencies {
        // ...
        classpath 'io.fabric.tools:gradle:1.24.4'
    }
}
```


### 초기화

메뉴에 Cause Crash 메뉴를 추가하고, 메뉴 처리시 크래시를 발생하도록 해보자,
`onOptionsItemSelected` 메서드에 crash_menu를 다루도록 추가한다.

*MainActivity.java*

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
   switch (item.getItemId()) {
       case R.id.crash_menu:
           Log.w("Crashlytics", "Crash button clicked");
           causeCrash();
           return true;
       case R.id.invite_menu:
           sendInvitation();
           return true;
       case R.id.fresh_config_menu:
           fetchConfig();
           return true;
       case R.id.sign_out_menu:
           mFirebaseAuth.signOut();
           mUsername = ANONYMOUS;
           startActivity(new Intent(this, SignInActivity.class));
           return true;
              default:
           return super.onOptionsItemSelected(item);
   }
}
```


#### causeCrash method

 MainActivity안에 다음 causeCrash method 를 추가해 주자:

*MainActivity.java*

```java
private void causeCrash() {
   throw new NullPointerException("Fake null pointer exception");
}
```


#### Test AdMob

1. To activate the app with Firebase Crashlytics, go to the Firebase Dashboard and click ‘Crashlytics', select ‘Yes, this app is new to Crashlytics.'

![](/images/google/firebase-crashlytic1.png){:width="500"}
<figcaption>[그림. 새 크래시리틱 생성하기]</figcaption>


2. 앱 화면의 메뉴에서 **Cause Crash** 를 실행한다.

3. 크래시 보고가 업로드 성공하면 로그캣에서 확인할 수 있다. (크래시 보고 업로드가 안 보이면 로그캣 필터를 *No Filters*로 한다.)

![](/images/google/firebase-crashlytic2-logcat.png){:width="500"}
<figcaption>[그림. 크래시 보고 로그캣 메시지]</figcaption>

크래시가 보고된 후에 5분 후 파이어베이스 콘솔에서 확인이 가능하다.

4. 파이어베이스 콘솔에서 *Crashlytics* 에서 보고서를 확인할 수 있다.

![](/images/google/firebase-crashlytic3.png){:width="500"}
<figcaption>[그림. 크래시리틱 보고]</figcaption>



---

## 참조

{% include_relative firebase_series.md %}
