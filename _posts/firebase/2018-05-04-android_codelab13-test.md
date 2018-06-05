---
title: "Firebase - Android Codelab (13): 테스트"
date: 2018-05-04 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 튜토리얼 중에서 **16. 앱 테스트(클라우드)** 을 한국어로 정리/요약 했다. 

## Firebase Test Lab

파이어베이스 테스트 랩은 앱을 여러 종류의 안드로이드 단말을 여러 API 수준과 지역화를 가로질러 테스트할 수 있도록 해준다. 이 테스트들은 모두 클라우드에서 자동으로 이루어진다.

[Android Test Lab](https://firebase.google.com/docs/test-lab/android/overview) 은 세 가지 테스트 기반을 제공한다.

**Test Dimensions × Test Executions = Test Matrix**

 - **Test Dimensions**: 앱을 테스트할 장치 유형으로, 장치 모델, 오에스 버전, 지역 그리고 화면 방향 등이다.
 - **Test Executions**: 개별 실행 가능한 테스트로 Test dimensions의 조합을 선택해 테스트한다.
 - **Test Matrix**: Test dimensions에서 실행한 테스트 결과.



### Espresso instrument 의존성 확인

앱의 모듈 `app/build.gradle` 에 `androidTestImplementation`이 있는지 확인한다.

*app/build.gradle* 파일:

```gradle
androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
```

### 로그온 및 Firebase Test plugin 활성화 확인

먼저 Android Studio 의 preference 에서 Firebase Test lab plugin이 활성화 되도록 한다.

![](/images/google/firebase-testlab-plugin.png){:width="500"}
<figcaption>[그림. Firebase Test Lab Plugin]</figcaption>

그리고 Google Cloud 계정에 로그인 해주어야 한다.

![](/images/google/as-google-login.png){:width="500"}
<figcaption>[그림. Android Studio에서 Google Cloud 로그인]</figcaption>


### Instrumentation test case 추가

파이어베이스 테스트 랩에서 실행할 기구 테스트를 `MainActivityEspressoTest.java` 파일에 

![](/images/google/firebase-testlab-test-file.png){:width="350"}
<figcaption>[그림. 테스트 케이스 파일]</figcaption>

테스트 케이스 메서드를 추가한다.

*MainActivityEspressoTest.java*

```java
@Test
public void verifySignUpButtonDisplayed() {
    onView(ViewMatchers.withId(R.id.sign_in_button)).check(matches(isDisplayed()));
}
```


### 테스트 위한 구성을 실행

Android Studio 프로젝트 뷰에서 **app > Edit Configurations...** 을 실행한다.

![](/images/google/firebase-testlab-edit_config.png){:width="500"}
<figcaption>[그림. Edit Configurations... ]</figcaption>

Configuration 화면이 열리면 **(+)** 버튼을 눌러 *Android Instrumented Tests*를 선택해서 새 테스트 구성을 생성한다:

1. **Name:** FriendlyChat Test
2. **Module:** app
3. **Test:** Class
4. **Class:** com.google.firebase.codelab.friendlychat.MainActivityEspressoTest

![](/images/google/firebase-testlab-config1.png){:width="500"}
<figcaption>[그림. Android Instrumented 구성하기 ]</figcaption>

5. **Target Options:** 타겟 메뉴에서 **Firebase Test Lab Device Matrix** 를 선택한다.

![](/images/google/firebase-testlab-config2.png){:width="500"}
<figcaption>[그림. 타겟 선택하기 ]</figcaption>

파이어베이스 프로젝트 선택하기

![](/images/google/firebase-testlab-config3.png){:width="500"}
<figcaption>[그림. 파이어베이스 프로젝트 선택 ]</figcaption>

Test Lab 구성을 실행한 결과는 콘솔에서 확인할 수 있다.

![](/images/google/firebase-testlab-result.png){:width="500"}
<figcaption>[그림. 콘솔에서 결과 화면]</figcaption>



### 파이어베이스 콘솔에서 테스트 랩 사용하기

여기서는 Android Studio 에서 Test Lab을 사용하는 사례를 보고 있다. 완성된 앱의 APK 를 직접 콘솔의 테스트 랩에서 Test Matrix를 실행할 수 있다.

![](/images/google/firebase-testlab-console1.png){:width="500"}
<figcaption>[그림. 파이어베이스 콘솔 - 프로젝트 오버뷰 화면]</figcaption>


안드로이드 앱 테스트에 대해 더 자세히 알고 싶다면 [Android Testing Codelab](https://codelabs.developers.google.com/codelabs/android-testing/index.html?)을 살펴보면 좋겠다.


---

## 참조

{% include_relative firebase_series.md %}
