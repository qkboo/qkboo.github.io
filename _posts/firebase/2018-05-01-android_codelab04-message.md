---
title: "Firebase - Android Codelab (4): Read messages"
date: 2018-05-01 12:00:00 +0900
layout: post
tags: [android, firebase, data binding, mvp, mvvm]
categories: [Programming, Android]
toc: true
---

[Firebase Android Codelab](https://codelabs.developers.google.com/codelabs/firebase-android/) 에서 제공하는 튜토리얼을 한국어로 정리/요약 한 것.

## 메시지 읽기

채팅 메시지를 포함하고 있는 Codelab 샘플 앱에 포함되 있는 *initial_messages.json* 파일을 파이어베이스 데이터베이스로 가져와온다.

### 메시지 가져오기

1. 파이어베이스 콘솔에서 Database를 실행해 Realtime Database로 들어간다.

2. 마우스 오른쪽 클릭으로 Import JSON을 실행한다.

![](/images/google/firebase-database-import-json.png){:width="500"}

*initial_messages.json* 파일을 선택한다.

![](/images/google/firebase-database-import-json2.png){:width="500"}

3. 메시지는 아래 같은 구조를 가지고 있다.

![](/images/google/firebase-database-import-json2.png){:width="500"}

#### 실시간 데이터베이스와 스토리지 의존성 추가

앱 프로젝트의 모듈 build.gradle 에 다음 의존성을 추가

```json
implementation 'com.google.firebase:firebase-database:15.0.0'
implementation 'com.google.firebase:firebase-storage:15.0.0'
```



#### 메시지 동기화

파이어베이스 데이터베이스를 초기화 하고 데이터의 변경을 감지하는 핸들에 리스너를 추가해 준면 리사이클러 뷰에 메시지가 표시된다.

*MainActivity.java* 에 선언를 한다.

```java
// Firebase instance variables
private DatabaseReference mFirebaseDatabaseReference;
private FirebaseRecyclerAdapter<FriendlyMessage, MessageViewHolder> mFirebaseAdapter;
```

*MainActivity.java* 의 onCreate() 에서 `mProgressBar.setVisibility(ProgressBar.INVISIBLE);` 를 아래 코드로 대체한다.
이 코드는 초기 있는 메시지를 추가하고 파이어베이스 데이터베이스에서 새 메시지를 대기한다.

```java
// New child entries
mFirebaseDatabaseReference = FirebaseDatabase.getInstance().getReference();
SnapshotParser<FriendlyMessage> parser = new SnapshotParser<FriendlyMessage>() {
            @Override
            public FriendlyMessage parseSnapshot(DataSnapshot dataSnapshot) {
                FriendlyMessage friendlyMessage = dataSnapshot.getValue(FriendlyMessage.class);
                if (friendlyMessage != null) {
                    friendlyMessage.setId(dataSnapshot.getKey());
                }
                return friendlyMessage;
            }
        };

DatabaseReference messagesRef = mFirebaseDatabaseReference.child(MESSAGES_CHILD);
        FirebaseRecyclerOptions<FriendlyMessage> options =
                new FirebaseRecyclerOptions.Builder<FriendlyMessage>()
                        .setQuery(messagesRef, parser)
                        .build();
mFirebaseAdapter = new FirebaseRecyclerAdapter<FriendlyMessage, MessageViewHolder>(options) {
            @Override
            public MessageViewHolder onCreateViewHolder(ViewGroup viewGroup, int i) {
                LayoutInflater inflater = LayoutInflater.from(viewGroup.getContext());
                return new MessageViewHolder(inflater.inflate(R.layout.item_message, viewGroup, false));
            }
            
            @Override
            protected void onBindViewHolder(final MessageViewHolder viewHolder,
                                            int position,
                                            FriendlyMessage friendlyMessage) {
       mProgressBar.setVisibility(ProgressBar.INVISIBLE);
if (friendlyMessage.getText() != null) {
   viewHolder.messageTextView.setText(friendlyMessage.getText());
   viewHolder.messageTextView.setVisibility(TextView.VISIBLE);
   viewHolder.messageImageView.setVisibility(ImageView.GONE);
} else {
   String imageUrl = friendlyMessage.getImageUrl();
   if (imageUrl.startsWith("gs://")) {
       StorageReference storageReference = FirebaseStorage.getInstance()
               .getReferenceFromUrl(imageUrl);
       storageReference.getDownloadUrl().addOnCompleteListener(
               new OnCompleteListener<Uri>() {
           @Override
           public void onComplete(@NonNull Task<Uri> task) {
               if (task.isSuccessful()) {
                   String downloadUrl = task.getResult().toString();
                   Glide.with(viewHolder.messageImageView.getContext())
                           .load(downloadUrl)
                           .into(viewHolder.messageImageView);
               } else {
                   Log.w(TAG, "Getting download url was not successful.",
                           task.getException());
               }
           }
       });
   } else {
       Glide.with(viewHolder.messageImageView.getContext())
               .load(friendlyMessage.getImageUrl())
               .into(viewHolder.messageImageView);
   }
   viewHolder.messageImageView.setVisibility(ImageView.VISIBLE);
   viewHolder.messageTextView.setVisibility(TextView.GONE);
}


viewHolder.messengerTextView.setText(friendlyMessage.getName());
if (friendlyMessage.getPhotoUrl() == null) {
   viewHolder.messengerImageView.setImageDrawable(ContextCompat.getDrawable(MainActivity.this,
           R.drawable.ic_account_circle_black_36dp));
} else {
   Glide.with(MainActivity.this)
           .load(friendlyMessage.getPhotoUrl())
           .into(viewHolder.messengerImageView);
}

   }
};

mFirebaseAdapter.registerAdapterDataObserver(new RecyclerView.AdapterDataObserver() {
   @Override
   public void onItemRangeInserted(int positionStart, int itemCount) {
       super.onItemRangeInserted(positionStart, itemCount);
       int friendlyMessageCount = mFirebaseAdapter.getItemCount();
       int lastVisiblePosition =
              mLinearLayoutManager.findLastCompletelyVisibleItemPosition();
       // If the recycler view is initially being loaded or the 
       // user is at the bottom of the list, scroll to the bottom 
       // of the list to show the newly added message.
       if (lastVisiblePosition == -1 ||
               (positionStart >= (friendlyMessageCount - 1) &&
                       lastVisiblePosition == (positionStart - 1))) {
           mMessageRecyclerView.scrollToPosition(positionStart);
       }
   }
});

mMessageRecyclerView.setAdapter(mFirebaseAdapter);
```


이어서 파이어베이스 데이터베이스에서 갱신하기 위해서 `onPause()` 와 `onResume()` 메서드를 갱신한다.

MainActivity.java

```java
@Override
public void onPause() {
    mFirebaseAdapter.stopListening();
    super.onPause();
}

@Override
public void onResume() {
    super.onResume();
    mFirebaseAdapter.startListening();
}
```

#### Test message sync

파이어베이스 데이터베이스에 새 메시지를 추가해서 클라이언트와 동기화가 되는지 확인해 보자,

1. 앱을 시작하자,
2. 파이어베이스 콘솔의 데이터베이스에서 새 메시지를 넣자, 

![](/images/google/fierbase-database-new-node.png){:width="500"}

3. 앱에서 추가된 메시지가 확인된다.



---

## 참조

{% include_relative firebase_series.md %}

