# 인프런 firebase 강좌 정리

[강좌 사이트 바로가기](https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%96%B4%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B0%95%EC%A2%8C-%EC%9B%B9-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98/)

## 환경 설정

1. [Firebase console](https://console.firebase.google.com/)에서 프로젝트 생성
2. 로컬 환경 설정
	-  [node.js](https://nodejs.org/ko/) 설치 
	-  npm에서 Firebase CLI 설치 `$ npm install firebase-tools -g`
	- Firebase CLI 로그인 `$ firebase login`
	- Firebase 프로젝트 정보 보기 `$ firebase list`
	- Firebase 프로젝트 초기화 `$ firebase init`
	- Firebase 로컬 서버 동작시키기 `$ firebase serve`

## 프로젝트 개발 - Web

### 1. 환경 설정 코드 추가

console에 있는 초기화 설정 코드를 프로젝트 파일에 추가

### 2. 인증 기능

#### 1. 인증 팝업 띄우기

```js
<script>
var auth; // firebase 인증 객체 
... (firebase 초기화)

auth = firebase.auth();
var authProvider = new firebase.auth.GoogleAuthProvider();
auth.signInWithPopup(authProvider);
</script>
```

#### 2. 로그인 성공/실패 분기 처리

```js
...
auth.onAuthStateChanged(function(user) {
    // user: 유저의 인증 정보
    if(user) {
      // 인증 성공시
      console.log("success");
      console.log(user);
    } else {
      // 인증 실패시
      auth.signInWithPopup(authProvider);
    }
  });
```

### 3. 데이터 저장하기

```js
var database = firebase.database();
var ref = database.ref('memos/');
ref.push({
	text: 'text'
})
```

### 4. 데이터 출력하기

```js
var database = firebase.database();
var ref = database.ref('memos/');
ref.on('child_added', function(data) {
	console.log(data.key);
	console.log(data.val());
});
```

### 5. 데이터 수정하기

```js
ref.on('child_changed', function(data) {
	console.log(data.key);
	console.log(data.val());
});
```

### 6. 데이터 삭제하기

```js
var ref = database.ref('memos/' + key);
ref.remove();
```

## 프로젝트 개발 - Android

### 1. 안드로이드 환경 설정

1. 콘솔에서 `google-services.json` 파일을 다운받아 `app` 위치에 넣어주기
2. SDK 추가

프로젝트 레벨 

```
buildscript {
    // ...
    dependencies {
        // ...
        classpath 'com.google.gms:google-services:3.0.0'
    }
}
```

모듈 레벨

```
apply plugin: 'com.android.application'

android {
  // ...
}

dependencies {
  // ...
  compile 'com.google.firebase:firebase-core:10.0.1'

  // Getting a "Could not find" error? Make sure you have
  // the latest Google Repository in the Android SDK manager
}

// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```

### 2. 인증 기능

#### 1. SDK 추가 - 모듈 레벨

```
compile 'com.google.firebase:firebase-auth:10.0.1'
compile 'com.google.android.gms:play-services-auth:10.0.1'
```

### 2. 인증 기능

#### 1. 인증 팝업 띄우기

```java
 mFirebaseAuth = FirebaseAuth.getInstance();

        GoogleSignInOptions googleSignInOptions = new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestIdToken(getString(R.string.default_web_client_id))
                .requestEmail()
                .build();

        mGoogleApiClient = new GoogleApiClient.Builder(this)
                .enableAutoManage(this, this)
                .addApi(Auth.GOOGLE_SIGN_IN_API, googleSignInOptions)
                .build();

        auth_sign_in.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = Auth.GoogleSignInApi.getSignInIntent(mGoogleApiClient);
                startActivityForResult(intent, 100);
            }
        });
```

#### 2. 로그인 성공/실패 분기 처리

```java
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == 100) {
            GoogleSignInResult result = Auth.GoogleSignInApi.getSignInResultFromIntent(data);
            GoogleSignInAccount account = result.getSignInAccount();
            if (result.isSuccess()) {
                 AuthCredential credential = GoogleAuthProvider.getCredential(account.getIdToken(), null);

        Task<AuthResult> authResultTask = mFirebaseAuth.signInWithCredential(credential);

        authResultTask.addOnSuccessListener(new OnSuccessListener<AuthResult>() {
            @Override
            public void onSuccess(AuthResult authResult) {
                startActivity(new Intent(AuthActivity.this, MainActivity.class));
                finish();
            }
        });
            } else {
                Toast.makeText(AuthActivity.this, "인증 실패", Toast.LENGTH_LONG).show();
            }
        }
    }
    ...
    @Override
    public void onConnectionFailed(@NonNull ConnectionResult connectionResult) {
        Toast.makeText(AuthActivity.this, "인증 실패", Toast.LENGTH_LONG).show();
    }
```

### 3. 데이터 저장하기

```java
mFirebaseDatabase.getReference("memos/" + mFirebaseUser.getUid()).push()
                .setValue(memo)
                .addOnSuccessListener(this, new OnSuccessListener<Void>() {
                    @Override
                    public void onSuccess(Void aVoid) {
                        Snackbar.make(content, "메모 저장", Snackbar.LENGTH_LONG).show();
                        initMemo();
                    }
                });
```

### 4. 데이터 출력하기

```java
mFirebaseDatabase.getReference("memos/" + mFirebaseUser.getUid())
                .addChildEventListener(new ChildEventListener() {
                    @Override
                    public void onChildAdded(DataSnapshot dataSnapshot, String s) {
                        Memo memo = dataSnapshot.getValue(Memo.class);
                        memo.setKey(dataSnapshot.getKey());
                        displayMemoList(memo);
                    }

                    @Override
                    public void onChildChanged(DataSnapshot dataSnapshot, String s) {
                        Memo memo = dataSnapshot.getValue(Memo.class);
                        memo.setKey(dataSnapshot.getKey());

                        for (int i = 0; i < mNavigationView.getMenu().size(); i++) {
                            MenuItem menuItem = mNavigationView.getMenu().getItem(i);
                            if (memo.getKey().equals(((Memo) menuItem.getActionView().getTag()).getKey())) {
                                menuItem.getActionView().setTag(memo);
                                menuItem.setTitle(memo.getTitle());
                                break;
                            }
                        }
                    }

                    @Override
                    public void onChildRemoved(DataSnapshot dataSnapshot) {

                    }

                    @Override
                    public void onChildMoved(DataSnapshot dataSnapshot, String s) {

                    }

                    @Override
                    public void onCancelled(DatabaseError databaseError) {

                    }
                });
```

### 5. 데이터 수정하기

```java
 Memo memo = new Memo();
        memo.setText(text);
        memo.setCreateDate(new Date().getTime());
        mFirebaseDatabase.getReference("memos/" + mFirebaseUser.getUid() + "/" + mSelectedMemoKey)
                .setValue(memo)
                .addOnSuccessListener(new OnSuccessListener<Void>() {
                    @Override
                    public void onSuccess(Void aVoid) {
                        Snackbar.make(content, "메모 수정", Snackbar.LENGTH_LONG).show();
                    }
                });
```

### 6. 데이터 삭제하기

```java
mFirebaseDatabase.getReference("memos/" + mFirebaseUser.getUid() + "/" + mSelectedMemoKey)
                        .removeValue(new DatabaseReference.CompletionListener() {
                            @Override
                            public void onComplete(DatabaseError databaseError, DatabaseReference databaseReference) {

                            }
                        });
```