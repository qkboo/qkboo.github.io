---
title: "Firebase - Android Codelab (8): 원격지 설정 - 메시지 크기"
date: 2018-05-03 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에서 제공하는 **11. Remotely Configure Friendly Message Length** 튜토리얼을 한국어로 정리/요약 했다. 

파이어베이스의 Remote Config는 원격에서 앱을 설정할 수 있게 해준다.

## Remotely configure

Firebase Cloud Messaging (FCM)은 앱 사용자에게 *알림*을 보낼 수 있다. 여기서 Firebase console에서 FCM을 보내고 앱에서 확인 알림을 받는 구성을 해보겠다.

여기서 Friendly Messages 는 메시지 크기가 제한되 있는데 파이어베이스 콘솔에서 이것을 변경해 보겠다.

### 콘솔에서 구성 규칙을 추가하기

파이어베이스 콘솔의 **Remote Config** 에서 **Add Parameter** 를 클릭한다.

![](/images/google/firebase-remotelyconfig.png){:width="500"}

그리고 파라미터 키로 **friendly_msg_length** 를 주고 값으로 10 정도를 입력한 후에 **Add Parameter** 버튼을 눌러 완료한다.

![](/images/google/firebase-remotelyconfig-add.png){:width="500"}

*변경사항 게시* 버튼을 눌러 공개한다.


### 앱에 Remote config 활성화 하기

앱 모듈의 app/build.gradle 파일에 *firebase-config* 의존성을 추가한다:

*app/build.gradle*

```
implementation 'com.google.firebase:firebase-config:15.0.0'
```

#### 액티비티

액티비티에 Remote Config 객체를 선언한다.


MainActivity.java (instance variable)

```java
// Firebase instance variables
private FirebaseRemoteConfig mFirebaseRemoteConfig;
```


#### 요청과 설정 사용

MainActivity의 `oncreate()` 내부에 데이터베이스 초기화 코드 위에 FirebaseRemoteConfig 를 초기화 하고 `fetchConfig()` 메서드를 호출한다.

*MainActivity.java*

```java
// Initialize Firebase Remote Config.
mFirebaseRemoteConfig = FirebaseRemoteConfig.getInstance();

// Define Firebase Remote Config Settings.
FirebaseRemoteConfigSettings firebaseRemoteConfigSettings =
       new FirebaseRemoteConfigSettings.Builder()
       .setDeveloperModeEnabled(true)
       .build();

// Define default config values. Defaults are used when fetched config values are not
// available. Eg: if an error occurred fetching values from the server.
Map<String, Object> defaultConfigMap = new HashMap<>();
defaultConfigMap.put("friendly_msg_length", 10L);

// Apply config settings and default values.
mFirebaseRemoteConfig.setConfigSettings(firebaseRemoteConfigSettings);
mFirebaseRemoteConfig.setDefaults(defaultConfigMap);

// Fetch remote config.
fetchConfig();
```


fetchConfig() 메서드는  Remote Config API를 사용해서 설정을 받아온다.

```java
// Fetch the config to determine the allowed length of messages.
public void fetchConfig() {
   long cacheExpiration = 3600; // 1 hour in seconds
   // If developer mode is enabled reduce cacheExpiration to 0 so that 
   // each fetch goes to the server. This should not be used in release
   // builds.
   if (mFirebaseRemoteConfig.getInfo().getConfigSettings()
           .isDeveloperModeEnabled()) {
       cacheExpiration = 0;
   }
   mFirebaseRemoteConfig.fetch(cacheExpiration)
           .addOnSuccessListener(new OnSuccessListener<Void>() {
               @Override
               public void onSuccess(Void aVoid) {
                   // Make the fetched config available via
                   // FirebaseRemoteConfig get<type> calls.
                   mFirebaseRemoteConfig.activateFetched();
                   applyRetrievedLengthLimit();
               }
           })
           .addOnFailureListener(new OnFailureListener() {
               @Override
               public void onFailure(@NonNull Exception e) {
                   // There has been an error fetching the config
                   Log.w(TAG, "Error fetching config: " + 
                           e.getMessage());
                   applyRetrievedLengthLimit();
               }
           });
}


/**
 * Apply retrieved length limit to edit text field. 
 * This result may be fresh from the server or it may be from cached
 * values.
 */
private void applyRetrievedLengthLimit() {
   Long friendly_msg_length =
           mFirebaseRemoteConfig.getLong("friendly_msg_length");
   mMessageEditText.setFilters(new InputFilter[]{new 
           InputFilter.LengthFilter(friendly_msg_length.intValue())});
   Log.d(TAG, "FML is: " + friendly_msg_length);
}
```

또한 메뉴에서 설정을 다시 받아 오게끔 옵션 메뉴에 추가한다.

MainActivity.java

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
   switch (item.getItemId()) {
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


### 앱 실행

1. 앱을 실행해서 메시지를 작성하면 10자로 제한된다.
2. 파이어베이스 콘솔에서 friendly_msg_length를 15 정도로 늘려서 테스트 해본다.

변경된 값을 수신하면 로그캣에 아래 같이 찍힌다.

![](/images/google/firebase-remotelyconfig-log.png){:width="500"}


> NOTE: Fresh config requests (requests from Remote Config server) are limited to 5 per hour. If this limit is exceeded requests will be throttled and cached values will be returned till the end of the hour.


---

## 참조

{% include_relative firebase_series.md %}


