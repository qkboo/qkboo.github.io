---
title: "Firebase - Android Codelab (7): Notifications"
date: 2018-05-02 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에서 제공하는 **10. Receive Reengagement Notifications** 튜토리얼을 한국어로 정리/요약 했다. 

## 노티피케이션 받기

Firebase Cloud Messaging (FCM)은 앱 사용자에게 *알림*을 보낼 수 있다. 여기서 Firebase console에서 FCM을 보내고 앱에서 확인 알림을 받는 구성을 해보겠다.

### FCM 의존성 추가


앱 모듈 그래들 *app/build.gradle* 파일에 의존성을 추가한다.

```gradle
implementation 'com.google.firebase:firebase-messaging:15.0.0'
```


### FCM 서비스

앱에서 FCM 서비스를 사용하기 위해서 토픽 가입에 `RegistrationIntentService` 클래스, 메시지 처리에 `MyFirebaseMessagingService` 백그라우드 서비스 클래스로 사용한다.

`RegistrationIntentService` 클래스는 백그라운드 서비스로 *InstanceID* 를 사용해서 FCM 서버에 앱을 식별시켜준다. 이것은 토픽 메시징을 통해 확인 메시지를 전송하는데 사용하는 토픽에 가입한다.

`MyFirebaseMessagingService` 클래스는 수신하는 FCM 메시지를 처리하는 백그라운드 서비스이다. 이것은 의존성으로 추가한 firebase-fcm library에 있는 `FirebaseMessagingService`를 상속한다. 

#### MyFirebaseMessagingService 클래스

`MyFirebaseMessagingService` 클래스는 `onMessageReceived` 를 재정의해서 알림을 처리한다.

*MyFirebaseMessagingService.java*

```java
public class MyFirebaseMessagingService extends FirebaseMessagingService {

   private static final String TAG = "MyFMService";

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       // Handle data payload of FCM messages.
       Log.d(TAG, "FCM Message Id: " + remoteMessage.getMessageId());
       Log.d(TAG, "FCM Notification Message: " +
               remoteMessage.getNotification());
       Log.d(TAG, "FCM Data Message: " + remoteMessage.getData());
   }
}
```


#### MyFirebaseInstanceIdService 클래스

`MyFirebaseInstanceIdService` 는 FCM 로직을 다루를 서비스로 InstanceID 토큰이 발생하면 앱을 갱신하는데 사용한다. 그러기 위해서 `FirebaseInstanceIdService`를 상속받아 `onTokenRefresh` 메서드를 재정의한다.

*MyFirebaseInstanceIdService.java*

```java
public class MyFirebaseInstanceIdService extends FirebaseInstanceIdService {

   private static final String TAG = "MyFirebaseIIDService";
   private static final String FRIENDLY_ENGAGE_TOPIC = "friendly_engage";
  
   /**
    * The Application's current Instance ID token is no longer valid 
    * and thus a new one must be requested.
    */
   @Override
   public void onTokenRefresh() {
       // If you need to handle the generation of a token, initially or
       // after a refresh this is where you should do that.
       String token = FirebaseInstanceId.getInstance().getToken();
       Log.d(TAG, "FCM Token: " + token);

       // Once a token is generated, we subscribe to topic.
       FirebaseMessaging.getInstance()
               .subscribeToTopic(FRIENDLY_ENGAGE_TOPIC);
   }
}
```


#### AndroidManifest.xml 클래스 등록

`MyFirebaseMessagingService`과 `MyFirebaseInstanceIdService` 클래스를 서비스로 등록한다.

*AndroidManifest.xml*

```xml
<service
   android:name=".MyFirebaseMessagingService"
   android:exported="false">
   <intent-filter>
       <action android:name="com.google.firebase.MESSAGING_EVENT" />
   </intent-filter>
</service>

<service
   android:name=".MyFirebaseInstanceIdService"
   android:exported="false">
   <intent-filter>
       <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
   </intent-filter>
</service>
```



### 실행

앱을 실행하고 Home 버튼을 눌러 현재 화면에 실행을 백그라운드로 보낸다.

#### 파이어베이스 콘솔에서 알림을 작성한다.

파이어베이스 콘솔에서 *Notification*을 활성화 시킨다. 왼쪽 메뉴에서 GLOW->Notification 를 선택하거나,

![](/images/google/firebase-cloudmessaging.png){:width="500"}

제품 목록에서 Notification 를 선택한다.

![](/images/google/firebase-cloudmessaging2.png){:width="500"}

#### 메시지 작성

Cloud Messaging -> Notificatios 화면에서 즉시 발송할 메시지를 작성을 시작한다

![](/images/google/firebase-cloudmessag2.png){:width="500"}

메시지 내용을 작성한다.

![](/images/google/firebase-cloudmessag4.png){:width="500"}


#### 알림 수신 확인

앱이 전면에 실행중에 로그캣에 출력하므로 로그캣을 확인해 보면 된다.

![](/images/google/firebase-cloudmessag4-log.png){:width="500"}


앱 실행이 중단되면 Notification manager로 알림이 수신되어 Notifications에서 확인이 가능하다.

![](/images/google/firebase-cloudmessag4-notify.png){:width="350"}


---

## 참조

{% include_relative firebase_series.md %}

