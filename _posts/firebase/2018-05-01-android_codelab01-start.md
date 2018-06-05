---
title: "Firebase - Android Codelab (1): 프로젝트 시작"
date: 2018-05-01 08:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에 있는 2~4번 과정을 정리한 것이다.

## Android 프로젝트에서 Codelab

제공하는 코드랩 코드 *friendlychat-android* 저장소는 프로젝트 두개로 구성되 있다:

- android-start: 코드랩을 따라할 수 있는 샘플 코드.
- android: 코드랩을 완료한 완성한 코드.

#### 다운로드 

```terminal
$ mkdir firebase-codelab
$ cd firebase-codelab/
```

깃헙에서 코드랩 샘플 코드를 다운로드 한다:

```terminal
$ git clone https://github.com/firebase/friendlychat-android
Cloning into 'friendlychat-android'...
```


#### 프로젝트 가져오기

*Android Studio*에서 다운로드한 `android-start` 프로젝트를 들여온다.

![](/images/google/firebase-codelab-import.png)
<figcaption>[그림. 코드랩 소스 들여오기]</figcaption>

이 프로젝트에서 파이어베이스 코드랩을 따라갈 때 사용할 클래스 파일은 *MainActivity*, *MyFirebaseInstanceIdService*, *MyFirebaseMessagingService* 이다.

이 프로젝트에 구성되 있는 파이어베이스 의존성은 [Firebase 시작하기]({{ "" | relative_url }}{% post_url /firebase/2018-04-20-firebase-start %}) 를 참조해서 그래들 파일 *build.gradle* 내용을 살펴보기 바란다.


---

## 참조

{% include_relative firebase_series.md %}

