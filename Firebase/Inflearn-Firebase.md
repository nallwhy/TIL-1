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

## 프로젝트 개발

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
      // 후에 메모 리스트 출력 기능
    } else {
      // 인증 실패시
      auth.signInWithPopup(authProvider);
    }
  });
```

### 3. 데이터 가져오기


<!--
4. 메모 저장 기능 구현
5. 메모 한 건 출력 기능 구현
6. 메모 수정 기능 구현
7. 메모 삭제 기능 구현-->

