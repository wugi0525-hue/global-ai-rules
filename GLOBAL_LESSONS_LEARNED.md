# Global Lessons Learned (전사 프로젝트 공통 레슨런)

이 문서는 앞으로 진행될 모든 신규 앱/웹 프로젝트에서 동일한 시행착오를 겪지 않기 위해 과거 프로젝트(APHASIA-APP 등)에서 겪었던 주요 이슈와 해결 방법을 누적 기록하는 문서입니다. 
AI 에이전트는 반복되는 오류나 무한 루프 발생 시 반드시 이 문서를 최우선으로 참고하여 해결책을 찾아야 합니다.

---

## 1. Authentication (로그인 및 인증)
### 1.1 소셜 로그인 적용 시 signInWithRedirect 사용 금지 및 signInWithPopup 사용 원칙
- **문제점**: `signInWithRedirect` 방식을 사용하면, 외부 인증 화면을 거쳐 되돌아올 때 교차 출처(Cross-Origin) 보안 및 서드파티 쿠키 차단으로 인해 세션이 소실되어 무한 로그인 튕김 현상이 발생함.
- **해결 방법**: 향후 모든 프로젝트는 무조건 `signInWithPopup` (팝업 방식)을 사용할 것!
- **인앱 브라우저 예외 처리**: 프론트엔드 로그인 페이지 진입 시 `userAgent`를 감지하여, 인앱 브라우저 환경일 경우 외부 브라우저(Chrome/Safari)로 앱을 강제 실행시키거나 링크를 복사해 이동하도록 유도하는 방어요소를 반드시 씌울 것.

## 2. QA and Verification Principles (검수 및 테스트 원칙)
### 2.1 AI 에이전트 다이렉트 검수 필수화
- **문제점**: 코드를 작성한 후 실제 환경에서 테스트하지 않고 완료 보고를 하여 치명적인 백색 화면(Crash)이 발생하는 사례 누적.
- **해결 방법**: 모든 UI 변경이나 로직 수정 시, 에이전트가 직접 가상 브라우저(Browser Subagent)를 띄워 라우팅, 로그인 등을 직접 클릭해 런타임 에러 여부를 100% 검증한 뒤에만 보고할 것.
### 2.2 가상 검수 임시 파일(Screenshots/Videos) 자동 폐기 원칙
- **해결 방법**: 시각적 UI 검수가 완전히 통과된 직후, 에이전트는 터미널 커맨드를 실행하여 방금 생성된 테스트용 이미지/영상 찌꺼기들을 전부 영구 삭제(Garbage Collection)해야 함.

## 3. Python 환경 설정 (Python Environment Setup)
### 3.1 Python 패키지 설치 규칙
- **해결 방법**: 모든 파이썬 패키지를 서버나 로컬에 설치(다운로드)할 때는, 터미널 명령어 서두에 항상 `python -m` 명령어를 먼저 붙인 뒤에 `pip`를 이어가도록 할 것.
- ❌ 잘못된 예: `pip install requests`
- ✅ 올바른 예: `python -m pip install requests`

## 4. React 및 프론트엔드 환경 설정 (React & Frontend Setup)
### 4.1 [날짜: 2026-02-25] Vite 기반 React 프로젝트 빌드 시 빈 화면(White Screen) 에러
- **문제점**: Vite로 만든 React 앱에서 화면이 하얗게 나오는 문제 발생.
- **해결 방법**: `vite.config.js` 파일에서 `base: './'` 설정을 추가해주어야 빌드 후 파일 경로를 제대로 찾음.
- **관련 프로젝트**: `rpa-app`, `stock-dashboard`

## 5. Git 및 버전 관리 (Git & Version Control)
### 5.1 [날짜: 2026-02-25] GitHub 다중 프로젝트 관리 시 커밋 충돌 방지
- **문제점**: VS Code에서 여러 개의 프로젝트 폴더를 띄워놓고 작업할 때, Git 커밋 창이 혼재되어 충돌 위험 발생.
- **해결 방법**: VS Code 창 하나당 **오직 한 개의 하위 프로젝트 폴더(예: `rpa-app`)만 단독으로 열어서 작업(Open Folder)**하는 것이 깃 꼬임을 방지하는 가장 안전한 방법임.

