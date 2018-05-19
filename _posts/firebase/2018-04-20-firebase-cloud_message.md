---
title: "Firebase - Cloud Message"
date: 2018-04-20 09:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase](https://firebase.google.com/docs/) 에서 Cloud Messaging을 사용해서 앱의 재사용을 촉진하는 과정을 알게된다.

## Firebase Cloud Message

FCM을 사용해 앱 사용자에게 기능 소개, 동기화 요구 등의 알림 메시지를 전송할 수 있다. 메시지는 4KB 크기 페이로드를 전송할 수 있다.

##### 주요기능

 - 알림 메시지 또는 데이터 메시지 전송
 - 다양한 메시지 타겟팅
 - 클라이언트 앱에서 메시지 전송

이들은 아래 같이 메시지 전송 체계를 제공하고 있다.

![FCM workflow](https://firebase.google.com/docs/cloud-messaging/images/messaging-overview.png?authuser=1)


### Cloud Messaging

파이어베이스 콘솔에서 *Notification*을 활성화 시킨다. 왼쪽 메뉴에서 GLOW->Notification 를 선택하거나,

![](/images/google/firebase-cloudmessaging.png){:width="500"}

제품 목록에서 Notification 를 선택한다.

![](/images/google/firebase-cloudmessaging2.png){:width="500"}

#### 메시지 작성

Cloud Messaging -> Notificatios 화면에서 즉시 발송할 메시지를 작성을 시작한다

![](/images/google/firebase-cloudmessag2.png){:width="500"}

메시지 내용을 작성한다.

![](/images/google/firebase-cloudmessag4.png){:width="500"}

##### 메시지 수신

앱이 화면에 실행중이 아니면 Notification Manager로 메시지가 전달되어, 단말의 Notification 에서 확인이 가능하다.

![](/images/google/firebase-cloudmessag4-notify.png){:width="350"}


---

## 참조

{% include_relative firebase_series.md %}

