---
title: "Firebase - Android Codelab (5): Send messages"
date: 2018-05-02 10:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에서 제공하는 튜토리얼을 한국어로 정리/요약 했다. 

## 메시지 보내기

채팅 메시지 전송을 추가해 보자,텍스트를 입력하고 전송 버튼을 누르면 메시지가 보내는 과정을 처리해 보자,


### 메시지 보내기

전송버튼을 누르면 `FriendlyMessage` 객체가 생성되고 파이어베이스 실시간 데이터베이스(RTDB)에 넣어진다. 이 객체를 *DatabaseReference* 객체의 `push()` 메서드로 전달하면, 객체의 경로에 자동 생성한 ID를 추가하게 된다. 아이디는 새 메시지가 목록에 추가될 때마다 순차적으로 증가하게 된다.

**MainActivity.java** 의 mSendButton.onClick() 메서드에서 메시지 전송을 추가해 보자,

```java
mSendButton = (Button) findViewById(R.id.sendButton);
mSendButton.setOnClickListener(new View.OnClickListener() {
   @Override
   public void onClick(View view) {
       FriendlyMessage friendlyMessage = new 
               FriendlyMessage(mMessageEditText.getText().toString(),
                               mUsername,
                               mPhotoUrl,
                               null /* no image */);
       mFirebaseDatabaseReference.child(MESSAGES_CHILD)
               .push().setValue(friendlyMessage);
       mMessageEditText.setText("");
   }
});
```


### 이미지 메시지 보내기

이미지를 포함한 메시지를 전송하려면

1. 이미지를 선택
2. 선택 이미지 처리
3. RTDB에 임시 이미지 메시지를 쓴다
4. 이미지를 업로드한다
5. 업로드 완료되면 업로드한 이미지의 메시지 URL을 갱신한다.

#### 1. 이미지를 선택

`mAddMessageImageView.onClick()` 메서드에서 **Intent.ACTION_OPEN_DOCUMENT** 인텐트 액션으로 내장 메모로 혹은 구글 드라이브에서 이미지 형식의 파일을 선택할 수 있다.

```java
MainActivity.java
mAddMessageImageView = (ImageView) findViewById(R.id.addMessageImageView);
mAddMessageImageView.setOnClickListener(new View.OnClickListener() {
   @Override
   public void onClick(View view) {
       Intent intent = new Intent(Intent.ACTION_OPEN_DOCUMENT);
       intent.addCategory(Intent.CATEGORY_OPENABLE);
       intent.setType("image/*");
       startActivityForResult(intent, REQUEST_IMAGE);
  }
});
```

앱을 실행하고 메시지 입력창 왼쪽의 **+** 버튼이 이미지 메시지 선택 버튼이다.

![](/images/google/firebase-start-imagemessage1.png){:width="500"}

버튼을 실행하면 도큐멘트 선택 앱이 실행된다.

![](/images/google/firebase-start-open-document.png){:width="500"}

선택한 이미지는 `onActiivtyResult()` 에서 처리한다.

#### 2. 선택 이미지 처리


```java
MainActivity.java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
   super.onActivityResult(requestCode, resultCode, data);
   Log.d(TAG, "onActivityResult: requestCode=" + requestCode + ", resultCode=" + resultCode);

   if (requestCode == REQUEST_IMAGE) {
       if (resultCode == RESULT_OK) {
           if (data != null) {
               final Uri uri = data.getData();
               Log.d(TAG, "Uri: " + uri.toString());

               FriendlyMessage tempMessage = new FriendlyMessage(null, mUsername, mPhotoUrl,
                       LOADING_IMAGE_URL);
               mFirebaseDatabaseReference.child(MESSAGES_CHILD).push()
                       .setValue(tempMessage, new DatabaseReference.CompletionListener() {
                           @Override
                           public void onComplete(DatabaseError databaseError,
                                                  DatabaseReference databaseReference) {
                               if (databaseError == null) {
                                   String key = databaseReference.getKey();
                                   StorageReference storageReference =
                                           FirebaseStorage.getInstance()
                                           .getReference(mFirebaseUser.getUid())
                                           .child(key)
                                           .child(uri.getLastPathSegment());

                                   putImageInStorage(storageReference, uri, key);
                               } else {
                                   Log.w(TAG, "Unable to write message to database.",
                                           databaseError.toException());
                               }
                           }
                       });
           }
       }
   }
}
```

실제 이미지 Uri 를 업로드하는 *putImageInStorage()* 메서드를 구현한다.

```
private void putImageInStorage(StorageReference storageReference, Uri uri, final String key) {
   storageReference.putFile(uri).addOnCompleteListener(MainActivity.this,
           new OnCompleteListener<UploadTask.TaskSnapshot>() {
               @Override
               public void onComplete(@NonNull Task<UploadTask.TaskSnapshot> task) {
                   if (task.isSuccessful()) {
                       FriendlyMessage friendlyMessage =
                               new FriendlyMessage(null, mUsername, mPhotoUrl,
                                       task.getResult().getMetadata().getDownloadUrl()
                                               .toString());
                       mFirebaseDatabaseReference.child(MESSAGES_CHILD).child(key)
                               .setValue(friendlyMessage);
                   } else {
                       Log.w(TAG, "Image upload task was not successful.",
                               task.getException());
                   }
               }
           });
}
```

#### 스토리지 허용하기

Firebase Storage를 콘솔에서 생성해야 클라이언트에서 사용할 수 있다. [Firebase Storage 사용하기]({{ "" | relative_url }}{% post_url /firebase/2018-04-20-firebase-storage %}) 를 참조해서 생성한다.


#### 이미지 전송

앱을 실행하고 메시지 입력창 왼쪽의 **+** 버튼을 클릭해 이미지를 선택한다.

![](/images/google/firebase-start-imagemessage1.png){:width="500"}


> Note: 여기서 메시지 글자수가 10자로 제한하고 있다. 다음 11단계의 **Remote Config** 단락에서 제한을 푸는 것을 알게된다.



---

## 참조

{% include_relative firebase_series.md %}

