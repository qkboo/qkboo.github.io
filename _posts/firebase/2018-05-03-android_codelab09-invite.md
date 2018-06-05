---
title: "Firebase - Android Codelab (9): 초대 설치하기"
date: 2018-05-03 10:30:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 튜토리얼 중에서 **12. Send Install Invites** 을 한국어로 정리/요약 했다. 


## 설치 초대하기

파이어베이스 앱 초대하기 서비스는, 앱 사용자에게 이메일, 문자를 통해 앱을 공유하도록 할 수 있다.

### AppInvite 추가하기

앱 모듈의 그래들 파일에 appinvite 의존성을 추가한다.

*app/build.gradle*

```gradle
implementation 'com.google.android.gms:play-services-appinvite:15.0.0'
```

#### GoogleApiClient 구성하기

액티비티에 `GoogleApiClient.OnConnectionFailedListener` 인터페이스를 구현한다.

```java
public class MainActivity extends AppCompatActivity
        implements GoogleApiClient.OnConnectionFailedListener {
    //...
    @Override
    public void onConnectionFailed(ConnectionResult connectionResult) {
        Log.d(TAG, "onConnectionFailed:" + connectionResult);
    }
}
```


AppInvites는 `startActivityForResult` 를 호출해서 개시된다. 이것은 AppInvites UI 가 초대장을 생성하고 `onActivityResult` 를 통해 활동을 호출하는 완성된 상태를 반환하는 것을 처리한다. oncrete에서 GoogleApiClient를 초기화 한다.


*MainActivity.java*

```java
mGoogleApiClient = new GoogleApiClient.Builder(this)
       .enableAutoManage(this, this)
       .addApi(Auth.GOOGLE_SIGN_IN_API)
       .build();
```


#### 초대 보내기

액티비티에서 AppInviteInvitation.IntentBuilder로 인텐트를 구성해 startActivityForResult로 AppInvites를 호출하는 `sendInvitation()` 메서를 구현한다

*MainActivity.java*

```java
private void sendInvitation() {
   Intent intent = new AppInviteInvitation.IntentBuilder(getString(R.string.invitation_title))
           .setMessage(getString(R.string.invitation_message))
           .setCallToActionText(getString(R.string.invitation_cta))
           .build();
   startActivityForResult(intent, REQUEST_INVITE);
}
```

AppInvites 의 결과는 onActivityResult()에서 요청한 `REQUEST_INVITE` 식별자를 이용해 다룬다.

*MainActivity.java*

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
   super.onActivityResult(requestCode, resultCode, data);
   Log.d(TAG, "onActivityResult: requestCode=" + requestCode + ", resultCode=" + resultCode);

   if (requestCode == REQUEST_IMAGE) {
       if (resultCode == RESULT_OK) {
           
           //...
           
       }
   } else if (requestCode == REQUEST_INVITE) {
       if (resultCode == RESULT_OK) {
           // Check how many invitations were sent and log.
           String[] ids = AppInviteInvitation.getInvitationIds(resultCode, data);
           Log.d(TAG, "Invitations sent: " + ids.length);
       } else {
           // Sending failed or it was canceled, show failure message to the user
           Log.d(TAG, "Failed to send invitation.");
       }
   }
}
```


초대는 메뉴 아이템으로 구성해서 `sendInvitation`을 호출한다.

*MainActivity.java*

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
   switch (item.getItemId()) {
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



### 초대 실행

앱을 실행하고 메뉴 항목에서 **Invite**를 실행하면 App Intites가 실행되어 현재 단말의 주소록에서 이메일 혹은 SMS 사용자가 표시된다. 모든 사용자에게 보내거나 사용자를 선택해서 작성한 메시지와 앱 주소를 보내게 된다.

![](/images/google/firebase-remotelyconfig-log.png){:width="500"}
<figcaption>[그림.]</figcaption>

---

## 참조

{% include_relative firebase_series.md %}

