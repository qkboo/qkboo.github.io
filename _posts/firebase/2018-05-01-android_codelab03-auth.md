---
title: "Firebase - Android Codelab (3): Auth"
date: 2018-05-01 12:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에 있는 Authentication을 정리했다.


### 인증 활성화

채팅 메시지를 읽고 보내기 위해 인증을 추가해 보자

#### 실시간 데이터 베이스

파이어베이스 실시간 데이터 베이스는 JSON 설정을 구성할 수 있다. 파이어베이스 프로젝트에 들어가 Database 를 선택하고 

![](/images/google/firebase-database-01.png){:width="500"}

**Realtime Database* 탭을 시작해서

![](/images/google/firebase-database-02.png){:width="500"}

규칙을 선언한다.

![](/images/google/firebase-database-03.png){:width="500"}

인증후 사용할 수 있도록 아래 같이 구성한다.

![](/images/google/firebase-database-04.png){:width="500"}


#### Authentication API

파이어베이스 인증은

1. Navigate to the Firebase console and select your project
2. Select Authentication
3. Select the Sign In Method tab
4. Toggle the Google switch to enabled (blue)
5. Press Save on the resulting dialog


파이어베이스 콘솔에서 프로젝트를 선택한 후에 Authentication 으로 들어간다.

![](/images/google/firebase-authentication-01.png){:width="500"}

여기서 가능한 인증 방법은 이메일/비밀번호, 전화, Google, Play gmae, facebook, twitter, github, anonymous 가 제공된다.

Google 인증으로 시작해 보자

![](/images/google/firebase-authentication-03.png){:width="500"}

#### Auth dependency 추가하기

앱 모듈의 build.gradle에 firebase-auth 플러그인을 추가한다.

*app/build.gradle* 파일:

```json
implementation 'com.google.firebase:firebase-auth:15.0.0'
```

Add the Auth instance variables in the MainActivity class:

*MainActivity.java* 파일

```java
// Firebase instance variables
private FirebaseAuth mFirebaseAuth;
private FirebaseUser mFirebaseUser;
```


#### 사용자 확인하기

로그인 화면을 표시하기 위해 onCreate 에 아래 코드를 추가하자

```java
// Set default username is anonymous.
mUsername = ANONYMOUS;

// Initialize Firebase Auth
mFirebaseAuth = FirebaseAuth.getInstance();
mFirebaseUser = mFirebaseAuth.getCurrentUser();
if (mFirebaseUser == null) {
    // Not signed in, launch the Sign In activity
    startActivity(new Intent(this, SignInActivity.class));
    finish();
    return;
} else {
    mUsername = mFirebaseUser.getDisplayName();
    if (mFirebaseUser.getPhotoUrl() != null) {
        mPhotoUrl = mFirebaseUser.getPhotoUrl().toString();
    }
}
```

로그아웃을 메뉴 아이템으로 추가해 보자,

```java
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.sign_out_menu:
                mFirebaseAuth.signOut();
                Auth.GoogleSignInApi.signOut(mGoogleApiClient);
                mUsername = ANONYMOUS;
                startActivity(new Intent(this, SignInActivity.class));
                finish();
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
```

#### 로그인 화면 구현

`SignInActivity` 클래스를 열고 구글 계정으로 로그인할 수 있도록 코드를 추가해 보자,

SignInActivity.java

```java
// Firebase instance variables
private FirebaseAuth mFirebaseAuth;
Then, edit the onCreate() method to initialize Firebase in the same way you did in MainActivity:
```

SignInActivity.java의 onCreate에 

```java
// Initialize FirebaseAuth
mFirebaseAuth = FirebaseAuth.getInstance();
```


로그인 버튼을 클릭하면 인증화면이 호출되도록 해보자,

SignInActivity.java

```java
@Override
public void onClick(View v) {
   switch (v.getId()) {
       case R.id.sign_in_button:
           signIn();
           break;
   }
}
```

Add the required signIn method that actually presents the user with the Google Sign-In UI.

SignInActivity.java

```java
private void signIn() {
        Intent signInIntent = Auth.GoogleSignInApi.getSignInIntent(mGoogleApiClient);
        startActivityForResult(signInIntent, RC_SIGN_IN);
    }
```


onActivityResult() 에 결과를 처리하는 코드를 추가해 보자,

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    // Result returned from launching the Intent from GoogleSignInApi.getSignInIntent(...);
    if (requestCode == RC_SIGN_IN) {
        GoogleSignInResult result = Auth.GoogleSignInApi.getSignInResultFromIntent(data);
        if (result.isSuccess()) {
            // Google Sign-In was successful, authenticate with Firebase
            GoogleSignInAccount account = result.getSignInAccount();
            firebaseAuthWithGoogle(account);
        } else {
            // Google Sign-In failed
            Log.e(TAG, "Google Sign-In failed.");
        }
    }
}
```

앱을 실행하면 구글 로그인 화면을 볼 수 있다.

![](/images/google/firebase-authentication-04-signin.png){:width="500"}

로그인을 진행하면서 구글 서비스 이용에 대한 동의 절차가 진행된다. 로그인 후 채칭 화면의 옵션 메뉴에서 Logout 도 정상적으로 동작하는지 확인해 보자.


---

## 참조

{% include_relative firebase_series.md %}



