# Subway English Driver — Claude Code 작업 지침

## 프로젝트 요약
- 초등학생(승우, 10세)이 지하철 기관사가 되어 영어 안내방송·승객 응대를 무전기로 말하며 익히는 브라우저 게임이다.
- 배포: GitHub Pages `https://spapa2351-design.github.io/subway-english/`
- 대상 기기: 24인치 iMac(크롬, 키보드), iPad(사파리, 터치).
- 현재 코드: 저장소 루트 `index.html` 단일 파일(HTML+CSS+JS, 외부 이미지·음원 없음).
- 기획 원본: `docs/specs/spec-subway-english-driver.md`. 구현 기준은 이 문서다.

## 첫 작업 (리팩터링)
1. `index.html`의 동작을 바꾸지 않고 아래 구조로 분리한다.
   - `index.html` — 마크업
   - `css/style.css`
   - `js/data.js` — LEVELS, 역 목록, TALK 문항, 방송 문장 함수
   - `js/render.js` — 캔버스 그리기(터널·야외·승강장·열차·사람·무전기)
   - `js/audio.js` — 효과음 합성, TTS, 음성 인식
   - `js/game.js` — 상태 전환, 입력, HUD
2. 빌드 도구를 쓰지 않는다. `<script type="module">` 또는 순서 있는 `<script>`로 로드한다.
3. 분리 후 모든 레벨을 1회 완주해 콘솔 오류 0건을 확인하고 보고한다.

## 기술 제약
- 외부 스크립트 라이브러리를 추가하지 않는다. 글꼴은 Google Fonts "Press Start 2P"만 쓴다.
- 그래픽은 캔버스 480×270 도트 렌더링을 유지한다. `image-rendering: pixelated`.
- 효과음은 Web Audio 합성으로 만든다. 음원 파일을 추가하지 않는다.
- 음성 인식은 `SpeechRecognition || webkitSpeechRecognition`, `lang='en-US'`.
- 저장은 `localStorage` 키 `subwayEnglishUnlocked` 하나만 쓴다(열린 레벨 번호).
- 화면 문구는 영어만 쓴다. 한글은 화면에 표시하지 않는다.
- 단어 "train" 대신 "subway"를 쓴다.

## 기획 문서 작업 규칙
- 기획 문서는 docs/specs/ 아래 spec-*.md 이다. 구현 전에 해당 문서를 읽는다.
- 문서에 명시된 규칙과 수용 기준만 구현한다.
- 문서에 없는 동작이 필요하면 구현하지 말고 작업 끝에 "확인 필요" 목록으로 보고한다.
- "범위 밖" 섹션의 항목은 구현하지 않는다.
- "미정" 항목은 기본안으로 구현하고, 보고에 해당 ID를 적는다.
- "프로토타입 전용" 섹션의 내용은 구현하지 않는다.
- CHANGELOG 파일은 참고용이다. 구현 기준은 spec 본문뿐이다.
- 작업이 끝나면 구현한 수용 기준 ID 목록과 테스트 결과를 보고한다.

## 배포
- main 브랜치에 커밋·푸시하면 GitHub Pages에 반영된다.
- 푸시 전에 로컬 서버(`python3 -m http.server`)에서 크롬으로 1개 레벨 이상 확인한다.
- 마이크·iPad 동작은 Claude Code가 검증할 수 없다. 해당 변경은 보고에 "보호자 실기기 확인 필요"로 적는다.
