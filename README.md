# 🏸 셔틀로그 — 배드민턴 클럽 기록 앱

배드민턴 스코어보드 + 경기 기록 + 파트너 분석 웹앱입니다.
설치 없이 브라우저로 쓰고, 홈 화면에 추가하면 앱처럼 동작합니다 (PWA · 오프라인 지원).

## 기능
- 코트 6개 동시 진행, 점수판에서 코트 즉시 전환
- 복식 서비스 규칙 자동 적용 (서브/리시브 선수·좌우 코트 위치 표시)
- 게임 수(1~3) / 게임당 포인트(15~30) / 듀스 허용 여부 설정
- 연속득점 · 게임 포인트 · 매치 포인트 실시간 표시, 실행취소
- 셔틀콕 사용 수 카운트
- 회원 등록 (급수 S~D·초심, 나이대)
- 날짜별 사용자 게임 수, 파트너(듀오) 승률·득실·접전·최근 폼 분석

---

## 1단계 — GitHub에 올려서 URL 만들기 (5분)

1. [github.com](https://github.com) 가입 후 **New repository** → 이름 예: `shuttlelog` → Public → Create
2. **Add file → Upload files** 로 이 폴더의 파일 전부 업로드 후 Commit
   (`index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`)
3. 저장소 **Settings → Pages** → Branch를 `main` / `(root)` 로 선택 → Save
4. 1~2분 뒤 `https://아이디.github.io/shuttlelog/` 주소가 생깁니다 → 클럽에 공유!

폰에서 이 주소를 열고 **홈 화면에 추가**하면 아이콘이 생기고 전체화면 앱처럼 실행됩니다.

## 2단계 — 실시간 서버 저장 켜기 (Firebase, 무료, 10분)

이 단계를 하면 **어느 폰에서 열어도 같은 기록·회원**이 보입니다. 안 해도 앱은 동작합니다 (기기별 저장).

1. [console.firebase.google.com](https://console.firebase.google.com) → **프로젝트 추가** (이름 예: shuttlelog) → 애널리틱스는 꺼도 됨
2. 프로젝트 홈에서 **웹 앱(</> 아이콘) 추가** → 앱 등록 → 화면에 나오는 `firebaseConfig = { ... }` 부분 복사
3. 왼쪽 메뉴 **빌드 → Firestore Database → 데이터베이스 만들기** → 위치 `asia-northeast3 (서울)` → **테스트 모드**로 시작
4. `index.html`을 열어 `const FIREBASE_CONFIG = null;` 을 찾아 복사한 설정으로 교체:
   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza....",
     authDomain: "shuttlelog-xxxx.firebaseapp.com",
     projectId: "shuttlelog-xxxx",
     storageBucket: "shuttlelog-xxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
5. GitHub에서 index.html을 다시 업로드(덮어쓰기) → 끝.
   상단에 **● 실시간 동기화** 표시가 뜨면 성공. 기존 폰에 있던 기록은 첫 접속 때 자동으로 서버로 옮겨집니다.

> ⚠️ 테스트 모드 규칙은 30일 후 만료되고, 주소를 아는 누구나 쓸 수 있는 상태입니다.
> Firestore → 규칙에서 만료일을 늘리거나, 나중에 로그인 기능을 붙일 때 규칙을 강화하세요.
> 동네 클럽 기록 용도로는 충분하지만 민감한 정보는 넣지 마세요.

## 3단계 (나중에) — 구글 플레이 스토어 앱으로

같은 코드를 그대로 [Capacitor](https://capacitorjs.com)로 감싸면 안드로이드 앱(AAB)으로 빌드됩니다.
- Google Play 개발자 계정 필요 (최초 1회 $25)
- 이 저장소에 Capacitor 설정 + GitHub Actions 빌드 파일만 추가하면 되며, 웹 버전과 코드를 공유합니다

---

## 데이터 저장 방식
| 상태 | 저장 위치 | 공유 |
|---|---|---|
| 기본 | 각 기기의 localStorage | 기기별 개별 |
| Firebase 설정 후 | Firestore (서버) + 로컬 백업 | **전 기기 실시간 공유** |

오프라인(체육관)에서도 동작하며, Firebase 사용 시 연결이 돌아오면 자동 동기화됩니다.
