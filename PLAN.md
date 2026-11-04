# Anatomy 교육용 사례집 — 1단계: 공식 예제 그대로 재현하기

작성일: 2026-11-04 · 기준 버전: `wheresryan22/anatomy` 커밋 `1482e84` (2026-10-18)

## 1. 목표

공식 예제 9종을 이 컴퓨터에서 원본과 똑같이 다시 만들고, 각 예제에 화면 캡처·PDF·한국어 해설을 붙여 `사례집/` 폴더에 모읍니다. 사용자가 이를 검토한 뒤 2단계(전공 연계 프로젝트)로 넘어갑니다.

### "그대로 제작"의 기준

예제마다 소스(`build.mjs`)가 들어 있으므로, 에이전트가 그림을 새로 그리지 않고 **소스를 그대로 빌드**합니다.

- 판정 기준: 빌드한 HTML이 저장소에 들어 있는 원본 HTML과 **바이트 단위로 같으면** 재현에 성공한 것으로 봅니다. 원저자의 CI도 같은 방식으로 검사합니다.
- 에이전트에게 예제를 처음부터 다시 그리게 하면 예제 하나에 몇 시간이 들고, 결과도 원본보다 떨어집니다. 처음부터 그리는 작업은 2단계 전공 프로젝트에서 합니다.

## 2. 예제 목록

| 예제 | 내용 | 방식 | 빌드 | 추가 검증 | 인쇄 PDF |
|---|---|---|---|---|---|
| dial-indicator | 스탠드 위 다이얼 게이지(가장 작음) | SVG | `build.mjs` (+`--light`) | — | — |
| desk-computer | 키보드로 입력하면 CRT에 글자가 뜨는 컴퓨터 | SVG | 같음 | — | — |
| push-button | 누름 버튼 | SVG | 같음 | — | `sheet.mjs` 3종 |
| arcade-cabinet | 동전이 경사로를 굴러 스위치를 누르는 오락기 단면 | SVG | 같음 | `--audit` | `sheet.mjs` 3종 |
| raptor-engine | 로켓엔진, 11개 묶음으로 분해, 화염 | SVG+WebGL | 같음 | `--audit` | `sheet.mjs` 3종 |
| ripple-tank | 학교 물결통, 간섭무늬와 빛 | SVG+WebGL | 같음 | — | — |
| turning-dial | 3D로 도는 다이얼과 로봇 손(베타) | 3D | 같음 + `--light --verify`, `stress.mjs --light` | `--audit` | — |
| test-rig | 스프링 캐리지와 다이얼 게이지 시험대 | React | 하네스 필요 | — | — |
| spot-plate | 유리 방울이 합쳐지는 반점판 | React | 하네스 필요 | — | — |

React 예제 2종은 `build.mjs`가 없고 원본 HTML도 없습니다. 작은 Vite + React 프로젝트를 만들어 띄우고, 화면이 제대로 나오는지와 콘솔 오류가 없는지로 판정합니다.

## 3. 사전 작업 (메인 세션이 먼저, 순서대로 진행)

병렬 작업은 이 단계를 마친 뒤 시작합니다.

1. **실행 권한 열기.** 자동 승인 정책이 외부 코드 실행(`node build.mjs`)을 막았습니다. 서브에이전트도 같은 정책을 따르므로, 프로젝트 `.claude/settings.local.json`에 `node` 실행 허용 규칙을 넣어야 합니다. React 담당은 `npm install`(네트워크) 허용도 필요합니다.
2. **빌드용 작업 폴더를 로컬 디스크에 만들기.** 경로는 `C:\anatomy-work\anatomy`입니다. 저장소를 `git clone`한 뒤 `1482e84`로 고정합니다.
   - 이유: 구글 드라이브(X:)에서 git 저장소와 headless Chrome 프로필을 돌리면 동기화 충돌과 잠금 문제가 생기기 쉽습니다.
   - `git diff`로 원본과 같은지 바로 확인할 수 있습니다.
   - 설치된 스킬 폴더(`.claude/skills/anatomy`)는 손대지 않고 참조용으로만 씁니다.
3. **환경 점검.** `node examples/dial-indicator/build.mjs`를 한 번 실행합니다. `git status`가 깨끗하면 Windows에서도 원본과 같은 결과가 나온다는 뜻입니다. 이어서 `scripts/capture.mjs`로 PNG 한 장을 찍어 Chrome 연동을 확인합니다.
4. **(선택) 회귀 테스트.** CI처럼 react·typescript·esbuild를 임시 폴더에 설치하고 `node --test scripts/*.test.mjs`를 돌립니다.
5. **결과 폴더 만들기.** `X:\내 드라이브\AI에이전트\Anatomy\사례집\` 아래에 예제별 하위 폴더를 만듭니다.

## 4. 병렬 작업 분담 (서브에이전트 5개)

무게가 비슷해지도록 묶었습니다. 각 에이전트는 **자기 예제 폴더와 자기 결과 폴더에만 씁니다.** 그래서 서로 충돌하지 않습니다.

| 에이전트 | 맡는 예제 | 무거운 부분 |
|---|---|---|
| A 경량 | dial-indicator, desk-computer, push-button | push-button PDF 3종 |
| B 오락기 | arcade-cabinet | 기하 감사(`--audit`), PDF 3종 |
| C 로켓 | raptor-engine | 가장 큼(HTML 1.6MB), 감사, WebGL, PDF 3종 |
| D 물리·3D | ripple-tank, turning-dial | WebGL 2겹, 3D 검증·스트레스 페이지 |
| E React | test-rig, spot-plate | Vite 하네스, npm 설치 |

### 공통 절차 (A~D)

예제마다 다음 순서로 진행합니다.

1. `C:\anatomy-work\anatomy`에서 다크·라이트를 모두 빌드합니다(`node examples/<이름>/build.mjs`, `--light`).
2. `git diff --stat -- examples/<이름>`으로 원본과 비교합니다. 차이가 있으면 **고치지 말고** 차이 내용을 보고서에 적습니다.
3. 감사가 있는 예제는 `--audit`를 돌리고 `audit passed`인지 확인합니다.
4. `scripts/capture.mjs`로 캡처합니다. 데스크톱 폭, 휴대폰 폭(390px), 마우스를 올린 상태, 움직임 중 2~3 순간을 다크·라이트 모두 찍습니다. 분해되는 예제는 `scripts/drive.mjs`로 분해 자세도 찍습니다. WebGL 예제는 `--no-webgl` 대체 화면도 찍습니다.
5. `sheet.mjs`가 있는 예제는 `node scripts/print.mjs examples/<이름>/sheet.mjs --format all --png`로 PDF 3종(poster, drawing, detailed)을 만듭니다. PDF는 생성 시각 때문에 원본과 바이트가 다를 수 있으므로 쪽수와 PNG 미리보기로 판정합니다.
6. 만든 PNG를 **직접 열어 봅니다.** 빈 화면, 깨진 셰이더, 콘솔 오류가 없는지 확인합니다.
7. 결과물을 `사례집/<이름>/`으로 복사하고 `보고서.md`를 씁니다(아래 5절 양식).

### 개별 지시

- **D**: turning-dial은 `build.mjs`, `--light`, `--light --verify`, `stress.mjs --light`까지 CI와 같은 4개를 만듭니다. `scripts/turn-check.mjs` 전체 실행은 15~20분 걸리므로 이번 단계에서는 빼고 보고서에 "미실행"으로 적습니다.
- **E**: `C:\anatomy-work\react-harness`에 Vite + React 19 + TypeScript 프로젝트를 만듭니다. `kit/`와 `examples/test-rig`, `examples/spot-plate`를 복사해 각 `figure.tsx`를 한 페이지씩 띄웁니다. 예제 소스는 고치지 않고, 하네스 쪽 코드로만 맞춥니다. `references/react.md`의 서버/클라이언트 구분은 Vite에서는 해당하지 않으므로, 그대로 클라이언트에서 렌더링합니다. 실행 화면을 `capture.mjs`로 찍고, `vite build` 결과(정적 HTML)를 `사례집/<이름>/`에 넣습니다.

### 서브에이전트에게 주지 않는 것

- 예제 소스 수정, 원본과 다르게 "개선"하는 작업
- `scripts/publish.mjs` 실행(외부 게시)
- 다른 에이전트의 폴더에 쓰기

## 5. 결과물 구조

```
사례집/
  index.html              ← 통합 갤러리 (메인 세션이 마지막에 만듦)
  <예제>/
    <예제>.html           다크
    <예제>-light.html     라이트
    shots/*.png           캡처
    *.pdf                 인쇄본 (해당 예제만)
    보고서.md
```

`보고서.md` 양식:

1. **재현 결과**: 원본과 바이트가 같은지, 감사 결과, 콘솔 오류
2. **무엇을 설명하는 그림인가**: 원래 비유한 개념과 실물
3. **조작법**: 마우스, 키보드, 자동 투어
4. **눈여겨볼 기법**: lathe, tube, WebGL, 3D, 분해, 인쇄 중 어떤 것을 썼는가. 2단계에서 참고할 부분을 적습니다.
5. **교육 활용 메모**: 어떤 교과와 단원에 바로 쓸 수 있는가
6. **문제와 미해결 사항**

## 6. 통합 (메인 세션, 병렬 작업이 끝난 뒤)

1. 5개 보고서를 모아 결과를 한 표로 정리합니다. 성공, 차이 있음, 실패로 나눕니다.
2. `사례집/index.html`을 만듭니다. 예제별 카드(대표 캡처, 한 줄 설명, 다크·라이트 링크, PDF 링크, 보고서 링크)를 넣습니다.
3. 사용자에게 검토를 요청합니다. 2단계 후보 주제를 고르는 데 쓰도록, 예제마다 "이 기법으로 만들 수 있는 전공 주제"를 함께 적습니다.

## 7. 완료 기준

- [ ] SVG·WebGL·3D 예제 7종의 다크·라이트 HTML이 원본과 바이트 단위로 같습니다. 같지 않으면 원인을 적었습니다.
- [ ] 감사가 있는 3종(arcade-cabinet, raptor-engine, turning-dial)이 `audit passed`로 끝났습니다.
- [ ] React 2종이 화면에 뜨고 콘솔 오류가 없습니다.
- [ ] PDF 3종 × 3예제 = 9개가 만들어졌습니다.
- [ ] 예제 9종 모두 캡처와 `보고서.md`가 있습니다.
- [ ] `사례집/index.html`에서 모든 결과물이 열립니다.

## 8. 위험과 대응

| 위험 | 대응 |
|---|---|
| 서브에이전트의 `node` 실행이 막힘 | 3절 1번에서 미리 허용 규칙을 넣습니다. 막히면 해당 에이전트는 멈추고 보고합니다. |
| headless Chrome 여러 개가 동시에 떠서 느려짐 | 에이전트를 5개로 제한합니다. 무거운 C와 D는 캡처를 순서대로 찍습니다. |
| Windows에서 빌드 결과가 macOS 원본과 다름 | 3절 3번에서 먼저 확인합니다. 차이가 나면 줄바꿈 문제인지 수치 문제인지 가려서 보고합니다. |
| WebGL이 headless에서 안 뜸 | 스크립트가 SwiftShader 옵션을 씁니다. 그래도 안 되면 `--no-webgl` 화면으로 대신하고 보고서에 적습니다. |
| 구글 드라이브 동기화 충돌 | 빌드는 로컬 `C:\anatomy-work`에서 하고, 완성본만 드라이브로 복사합니다. |
