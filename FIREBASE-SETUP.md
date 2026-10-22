# ShotCraft Google 로그인 켜기 (Firebase 설정, 약 10분)

대표님 Google 계정으로 한 번만 하면 돼요. 사용자가 적을 때는 무료(Spark 요금제)라 카드 등록이 필요 없어요.
설정 전에는 앱이 게스트 모드로만 동작하고, 첫 화면의 'Google로 시작하기' 버튼이 회색으로 꺼져 있어요.

## 1. 프로젝트 만들기
1. https://console.firebase.google.com 에 들어가 Google 계정으로 로그인해요.
2. **프로젝트 만들기**(또는 '프로젝트 추가')를 누르고 이름에 `ShotCraft` 를 넣어요.
3. Google 애널리틱스는 **사용 안 함**으로 끄고 만들기를 눌러요.

## 2. 웹 앱 등록하고 구성값 받기
1. 프로젝트 첫 화면에서 **`</>`(웹)** 아이콘을 눌러요.
2. 앱 닉네임에 `ShotCraft web` 을 넣고 **앱 등록**을 눌러요. (Firebase 호스팅은 체크하지 않아요)
3. 화면에 나오는 `const firebaseConfig = { apiKey: "...", authDomain: "...", ... };` 에서 **중괄호 `{ ... }` 부분 전체**를 복사해 둬요.
   - 나중에 다시 보려면: 왼쪽 위 톱니바퀴 › **프로젝트 설정** › 아래쪽 **내 앱** › SDK 설정 및 구성 › **구성**

## 3. Google 로그인 켜기
1. 왼쪽 메뉴 **빌드 › Authentication** › **시작하기**.
2. **로그인 방법(Sign-in method)** 탭 › **Google** › **사용 설정** 스위치를 켜요.
3. '프로젝트의 공개용 이름'에 `ShotCraft`, '프로젝트 지원 이메일'에 대표님 이메일을 고르고 **저장**.
4. **설정** 탭 › **승인된 도메인** › **도메인 추가** › `dgon45.github.io` 를 넣고 추가해요.
   (`localhost` 와 `....firebaseapp.com` 은 이미 들어 있어요)

## 4. 작업 저장소(Firestore) 만들기
1. 왼쪽 메뉴 **빌드 › Firestore Database** › **데이터베이스 만들기**.
2. 위치는 **asia-northeast3 (서울)** 을 고르고, **프로덕션 모드**로 시작해요.
3. 만들어지면 **규칙** 탭을 열고, 안의 글을 모두 지운 뒤 아래를 붙여 넣고 **게시**를 눌러요.
   (각 사람은 자기 데이터만 읽고 쓸 수 있게 막는 규칙이에요)

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## 5. 구성값을 앱에 넣기
둘 중 편한 방법으로 하세요.

- **쉬운 방법**: 2번에서 복사한 `{ ... }` 를 채팅에 붙여 주세요. 제가 넣고 올릴게요.
  (이 값은 웹페이지에 원래 공개되는 값이라 비밀번호가 아니에요. 보안은 4번 규칙이 지켜요)
- **직접 하기**: GitHub 저장소 `dgon45/sturdy-eureka` › `firebase-config.js` › 연필(수정) 아이콘 ›
  `window.SHOTCRAFT_FIREBASE = null;` 의 `null` 을 복사한 `{ ... }` 로 바꾸고 **Commit changes**.

```js
window.SHOTCRAFT_FIREBASE = {
  apiKey: "AIza....",
  authDomain: "shotcraft-xxxx.firebaseapp.com",
  projectId: "shotcraft-xxxx",
  storageBucket: "shotcraft-xxxx.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef"
};
```

## 6. 확인
1. 1~2분 뒤 https://dgon45.github.io/sturdy-eureka/ 를 열어요. (예전 화면이 보이면 새로고침 두 번)
2. **Google로 시작하기** › 계정 선택 › 앱 화면 아래에 내 이메일과 '계정 삭제'가 보이면 성공이에요.
3. 폰에서 장르를 하나 고르고, PC에서 같은 계정으로 로그인하면 그 장르가 골라져 있어요.

## 참고
- 비용: 무료 한도(하루 읽기 5만 번, 쓰기 2만 번)는 사용자 수백 명까지 충분해요.
- 아이폰(애플 로그인)은 iOS 출시 때 Authentication › 로그인 방법 › Apple 을 추가하면 돼요. 애플 개발자 계정(연 99달러)이 필요해요.
- Firebase 콘솔 › Authentication › 사용자 탭에서 가입한 사람 목록을 볼 수 있어요.
