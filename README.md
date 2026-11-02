# ShotCraft

영화 같은 AI 영상을 위한 프롬프트 빌더(설치형 웹앱). GitHub Pages로 배포하고 PWABuilder로 Google Play용 앱 파일을 만듭니다.

- 앱: `index.html` · 매니페스트: `manifest.webmanifest` · 오프라인: `sw.js`
- Google 로그인 설정값: `firebase-config.js` (켜는 방법: [FIREBASE-SETUP.md](FIREBASE-SETUP.md))
- 개인정보처리방침: `privacy.html`

## 1.0 출시 설정 (`index.html` 안 `PLAN`)
- `PLAN.ads=false`, `PLAN.proSale=false`: 1.0은 광고·결제 없이 모든 기능 무료. 실제 광고나 Google Play 결제(서버에서 구매 확인 포함)를 붙인 뒤에만 켜세요.
- `CONTACT_EMAIL`: 채우면 앱 아래에 '문의' 링크가 생겨요.
- 링크 미리보기 그림: `og-image.png`
