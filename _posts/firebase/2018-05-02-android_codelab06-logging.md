---
title: "Firebase - Android Codelab (6): Logging Action"
date: 2018-05-02 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에서 제공하는 튜토리얼을 한국어로 정리/요약 했다. 

## 단말 인덱스에 메시지 넣기

파이어베이스 앱 인덱싱을 사용하면 단말에 개인 콘텐츠를 인덱스할 수 있다. 단말 인덱스가 활성화 되면 검색 쿼리로 콘텐츠를 찾을 수 있다.

여기서 앱 메시지 작성시 인덱스하고 구글 앱에서 발견할 수 있도록 한다.

### 인덱스 의존성 추가

파이어베이스 인덱싱 의존성은 메시지 작성시과 사용자 행위 기록을 단말에서 인덱스 하도록 한다. 앱 모듈의 build.gradle에 다음을 추가한다.


```json
implementation 'com.google.firebase:firebase-appindexing:15.0.0'
```


### 인텐츠 필터 추가

`AndroidManifest.xml` 에 필터를 추가해 준다.

```xml
<activity android:name="com.google.firebase.codelab.friendlychat.MainActivity">
   <intent-filter>
       <action android:name="android.intent.action.MAIN"/>
       <category android:name="android.intent.category.LAUNCHER"/>
   </intent-filter>
   <intent-filter>
       <action android:name="android.intent.action.VIEW"/>
       <category android:name="android.intent.category.DEFAULT"/>
       <category android:name="android.intent.category.BROWSABLE"/>
       <data
         android:host="friendlychat.firebase.google.com"
         android:scheme="http"
         android:pathPrefix="/message"
/>
   </intent-filter>
</activity>
```


### 개인 콘텐츠를 인덱스

단말에서 언제든 메시지를 전송할 때 이 메시지를 단말 인덱스에 추가할 수 있어서 구글 앱을 통해 발견할 수 있다. 아래 코드는 메시지를 단말 인덱스에 새 메시지를 생성하는 코드이다.

*MainActivity.java*

```java
private Indexable getMessageIndexable(FriendlyMessage friendlyMessage) {
   PersonBuilder sender = Indexables.personBuilder()
           .setIsSelf(mUsername.equals(friendlyMessage.getName()))
           .setName(friendlyMessage.getName())
           .setUrl(MESSAGE_URL.concat(friendlyMessage.getId() + "/sender"));

   PersonBuilder recipient = Indexables.personBuilder()
           .setName(mUsername)
           .setUrl(MESSAGE_URL.concat(friendlyMessage.getId() + "/recipient"));

   Indexable messageToIndex = Indexables.messageBuilder()
           .setName(friendlyMessage.getText())
           .setUrl(MESSAGE_URL.concat(friendlyMessage.getId()))
           .setSender(sender)
           .setRecipient(recipient)
           .build();

   return messageToIndex;
}
```

새 메시지가 있으면 viewHolder에 `indexMessage()` 호출하는 코드를 추가한다.


*MainActivity.java*

```java
@Override
protected void onBindViewHolder(final MessageViewHolder viewHolder, FriendlyMessage friendlyMessage, int position) {
  //...
 if (friendlyMessage.getText() != null) {
   // write this message to the on-device index
   FirebaseAppIndex.getInstance()
           .update(getMessageIndexable(friendlyMessage));
 }
}
```

> **Note:** It helps to add an IntentService that establishes a base index of all messages for you initially. See details in the [App Indexing documentation](https://firebase.google.com/docs/app-indexing/android/app).


### 사용자 행위 기록

앱 안에서 [사용자 행위 기록](https://firebase.google.com/docs/app-indexing/android/log-actions)은 구글 앱에서 앱 컨텐츠에 대한 사용자 경험을 개선하는데 도움을 준다.

메시지를 보는 것 같은 개인적 내용에 대한 사용자 행위를 기록하기 위해서는 `Action` 객체의 `Metadata` 에서 업로드 속성을 false 로 해주어야 단말에 기록이 남고 구글 서버에 업로드 되지 않는다.

*MainActivity.java*

```java
private Action getMessageViewAction(FriendlyMessage friendlyMessage) {
return new Action.Builder(Action.Builder.VIEW_ACTION)
               .setObject(friendlyMessage.getName(), MESSAGE_URL.concat(friendlyMessage.getId()))
               .setMetadata(new Action.Metadata.Builder().setUpload(false))
               .build();
}
```


기록이 끝나면 `FirebaseUserActions.getInstance().end(...)` 메서드에 `getMessageViewAction()` 에서 번환하는 Action 객체를 넘겨주면서 호출해 준다.


MainActivity.java

```java
...

@Override
protected void onBindViewHolder(final MessageViewHolder viewHolder, FriendlyMessage friendlyMessage, int position) {

...
if (friendlyMessage.getText() != null) {
    // write this message to the on-device index
    FirebaseAppIndex.getInstance()
            .update(getMessageIndexable(friendlyMessage));
}

// log a view action on it
FirebaseUserActions.getInstance().end(getMessageViewAction(friendlyMessage));
}
```



### 행위 기록 테스트

1. 앱을 실행하고 메시지 *Hi world* 메시지를 전송한다
2. Google app에서 *Hi world* 메시지를 검색하고, 결과는 Personnel tab에서 확인할 수 있다.

![](/images/google/firebase-logging-useraction2.png){:width="500"}


---

## 참조

{% include_relative firebase_series.md %}

