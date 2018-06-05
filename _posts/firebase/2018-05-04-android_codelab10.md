---
title: "Firebase - Android Codelab (10): 사용자 추적"
date: 2018-05-04 09:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 튜토리얼 중에서 **13. Track User Flows** 을 한국어로 정리/요약 했다. 

파이어베이스를 통해 구글 애널리틱스에 사용자가 앱에서 행한 행위에 대한 기록을 전달해 분석하고 보고할 수 있어서 앱 개선에 도움을 줄 수 있다.

## 사용자 흐름 추적하기

파이어베이스 구글 애널리틱 `firebase-analytics`은 앱 사용 행위를 통한 사용자들의 행동을 이해하는 방법을 제공한다.


### 파이어베이스 애널리틱스 의존성 추가

앱의 모듈 `app/build.gradle` 파일에 `firebase-analytics` 의존성을 추가한다.

*app/build.gradle* 파일:

```gradle
implementation 'com.google.firebase:firebase-analytics:15.0.0'
```


#### 애널리틱스 초기화

멤버 변수로 FirebaseAnalytics 객체를 선언한다.

*MainActiivty.java* 

```java
private FirebaseAnalytics mFirebaseAnalytics;
```

mFirebaseAnalytics 객체를 onCreate() 안에서 초기화 한다.

```java
mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
```

파이어베이스 구글 애널리틱스를 초기화 하면 앱 설치, 세션 생명주기 같은 기본 매트릭스를 제공하게 초기화  한다.

#### 사용자 이벤트 전송

파이어베이스 구글 애널리틱스의 기본 매트릭스 보다 사용자 정의 기록을 처리하기 위해서, 주문형 이벤트 정보를 기록할 수 있다.  이런 주문형 이벤트 기록은 `mFirebaseAnalytics.logEvent()` 를 호출한다.

onActivityResult 에서 초대 이벤트를 로그한다. *REQUEST_INVITE* 요청 코드를 처리하는데 SHARE 이벤트를 성공 혹은 실패에 대해서 인자로 보낸다.


```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
   super.onActivityResult(requestCode, resultCode, data);
   Log.d(TAG, "onActivityResult: requestCode=" + requestCode + ", resultCode=" + resultCode);

   If (requestCode == REQUEST_IMAGE) {
       // ... 
   } else if (requestCode == REQUEST_INVITE) {
       if (resultCode == RESULT_OK) {
           Bundle payload = new Bundle();
           payload.putString(FirebaseAnalytics.Param.VALUE, "sent");
           mFirebaseAnalytics.logEvent(FirebaseAnalytics.Event.SHARE,
                   payload);
           // Check how many invitations were sent and log.
           String[] ids = AppInviteInvitation.getInvitationIds(resultCode,
                   data);
           Log.d(TAG, "Invitations sent: " + ids.length);
       } else {
           Bundle payload = new Bundle();
           payload.putString(FirebaseAnalytics.Param.VALUE, "not sent");
           mFirebaseAnalytics.logEvent(FirebaseAnalytics.Event.SHARE,
                  payload);
           // Sending failed or it was canceled, show failure message to
           // the user
           Log.d(TAG, "Failed to send invitation.");
       }
   }
}
```


파이어베이스에서 구글 애널리틱으로 기록한 이벤트는 모으고, 익명처리되어 24시간 사이에 콘솔에 보고된다.

Any events you log to Google Analytics for Firebase will be aggregated, anonymized, and reported in the Firebase console within 24 hours.



---

## 참조

{% include_relative firebase_series.md %}
