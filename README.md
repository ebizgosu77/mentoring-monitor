# 멘토링 모니터 (Mentoring Monitor)

멘토링 산출물 모니터링 웹앱입니다. 매니저들이 공동으로 사용하며, **Firebase Realtime Database**로 실시간 데이터를 공유합니다.

- 과정 / 멘토 / 단계 / 산출물 관리 (설정 패널)
- 멘토 × 산출물 **체크 현황표** + 완료율
- 멘토별 **수행일지** 추가/수정/삭제
- 여러 매니저 간 **실시간 동기화** (Firebase `onValue`)

## 기술 스택

- 순수 HTML + CSS + Vanilla JavaScript (빌드 도구 없음)
- Firebase v9 CDN (compat 버전)
- 단일 `index.html` 파일

## 로컬 실행

빌드가 없으므로 브라우저에서 바로 열 수 있습니다. 다만 Firebase 인증 도메인 제약 때문에 로컬 서버 사용을 권장합니다.

```bash
# Python 3
python -m http.server 8000
# 또는 Node
npx serve .
```

→ 브라우저에서 `http://localhost:8000` 접속 후 콘솔(F12)에서 `[Firebase] 온라인 🟢` 로그를 확인하세요.

## Firebase 설정 교체 방법

이 저장소의 `index.html`에는 데모용 Firebase 프로젝트 설정이 들어 있습니다. **본인 프로젝트로 교체**하려면 아래 순서를 따르세요.

1. [Firebase Console](https://console.firebase.google.com/) 에서 프로젝트를 생성합니다.
2. **빌드 → Realtime Database**를 만들고 리전을 선택합니다.
3. **프로젝트 설정 → 일반 → 내 앱**에서 웹 앱(`</>`)을 추가하면 `firebaseConfig` 값을 받을 수 있습니다.
4. `index.html`을 열어 `<script>` 안의 `firebaseConfig` 객체를 본인 값으로 교체합니다.

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.<region>.firebasedatabase.app",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

> `databaseURL`은 리전에 따라 형식이 다릅니다. Realtime Database 페이지 상단에 표시된 URL을 그대로 복사하세요.

5. **Realtime Database 보안 규칙**을 설정합니다. 공개 데모라면 아래처럼 열 수 있으나(누구나 읽기/쓰기), 운영 환경에서는 인증 기반 규칙을 권장합니다.

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> 규칙이 막혀 있으면 앱에서 쓰기 작업 시 우측 하단에 빨간 실패 토스트가 뜨고, 콘솔에 `permission_denied`가 출력됩니다.

6. **승인된 도메인 추가**: 프로젝트 설정 → Authentication → Settings(또는 Hosting 도메인 설정)에서 GitHub Pages 도메인(`<username>.github.io`)을 허용 도메인에 추가합니다. (Realtime Database만 사용하고 Auth를 쓰지 않으면 생략 가능)

앱 최초 실행 시 데이터가 비어 있으면 샘플 과정/멘토/단계/산출물이 자동으로 시드됩니다.

## GitHub Pages 배포

1. 이 저장소를 GitHub에 푸시합니다 (`main` 브랜치, `index.html`이 루트).
2. GitHub 저장소 → **Settings → Pages**
3. **Source**: `Deploy from a branch`
4. **Branch**: `main` / `/ (root)` 선택 후 **Save**
5. 잠시 후 `https://<username>.github.io/mentoring-monitor/` 에서 접속 가능합니다.

## 디자인

[encorecampus.ai](https://encorecampus.ai) 스타일의 다크/테크 테마 (딥 네이비 배경 + 사이안/퍼플 강조).
