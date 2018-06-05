---
title: "Firebase - Android Codelab (11): 수익과 광고"
date: 2018-05-04 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 튜토리얼 중에서 **14. 광고 수익** 을 한국어로 정리/요약 했다. 


## 광고 수익

파이어베이스 구글 애널리틱은 사용자 앱 통한 사용자들의 이동을 이해하는 방법을 제공한다.


### 파이어베이스 애널리틱스 의존성 추가

앱의 모듈 `app/build.gradle` 파일에 `play-services-ads` 의존성을 추가한다.

*app/build.gradle* 파일:

```gradle
implementation 'com.google.android.gms:play-services-ads:15.0.0'
```


#### 이름 공간 추가

activity_main.xml 파일의 최상위 RelativeLayout에 `xmlns:ads`을 추가한다.

*activity_main.xml* 

```xml
<RelativeLayout
   xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:tools="http://schemas.android.com/tools"
   xmlns:ads="http://schemas.android.com/apk/res-auto"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:paddingBottom="@dimen/activity_vertical_margin"
   android:paddingLeft="@dimen/activity_horizontal_margin"
   android:paddingRight="@dimen/activity_horizontal_margin"
   android:paddingTop="@dimen/activity_vertical_margin"
   tools:context="com.google.firebase.codelab.friendlychat.MainActivity">
```

그리고 *activity_main.xml*의 RelativeLayout의 맨 위에 ad view를 추가한다.

*activity_main.xml*

```xml
<com.google.android.gms.ads.AdView
   android:id="@+id/adView"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:layout_centerHorizontal="true"
   android:layout_alignParentTop="true"
   ads:adSize="BANNER"
   ads:adUnitId="@string/banner_ad_unit_id">
</com.google.android.gms.ads.AdView>
```

RecyclerView를 adView 밑에 배치하기 위해 `android:layout_below="@+id/adView"` 로 배치해 준다.한다.

*activity_main.xml*

```xml
<android.support.v7.widget.RecyclerView
   android:id="@+id/messageRecyclerView"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:layout_below="@+id/adView"
   android:layout_above="@+id/linearLayout"/>
```


#### AdView 객체

In the MainActivity add an instance variable that represents the AdView:

*MainActivity.java*

```java
private AdView mAdView;
Request Ad
```

*MainActivity.java* 안의 `onCreate()` 메서드에 ad 요청을 추가한다:

*MainActivity.java*

```java
mAdView = (AdView) findViewById(R.id.adView);
AdRequest adRequest = new AdRequest.Builder().build();
mAdView.loadAd(adRequest);
```


#### Handle lifecycle events

MainActivity에 액티비티 생명주기 이벤트, 정지, 재시작 그리고 종료에 대한 처리를 추가한다.


*MainActivity.java*

```java
@Override
public void onPause() {
   if (mAdView != null) {
       mAdView.pause();
   }
   mFirebaseAdapter.stopListening();
   super.onPause();
}

/** Called when returning to the activity */
@Override
public void onResume() {
   super.onResume();
   mFirebaseAdapter.startListening();
   if (mAdView != null) {
       mAdView.resume();
   }
}

/** Called before the activity is destroyed */
@Override
public void onDestroy() {
   if (mAdView != null) {
       mAdView.destroy();
   }
   super.onDestroy();
}
```


#### Test AdMob

1. 실행을 한다.
2. 광고가 보이즌지 확인한다.

![](/images/google/firebase-adview.png){:width="350"}
<figcaption>[그림. 앱 광고 화면]</figcaption>



---

## 참조

{% include_relative firebase_series.md %}
