# Anatomy 교육용 사례집

실제 물체를 아이소메트릭 해부도로 그려 원리를 설명하는 [anatomy](https://github.com/wheresryan22/anatomy) 스킬로 만든 교육용 사례집입니다.

- **1단계**: 공식 예제 9종을 원본 그대로 재현하고 검증했습니다.
- **2단계**: 9종 각각에 인터랙티브 한국어 교육판을 만들었습니다. 변수를 바꾸면 그림이 물리 모델대로 반응합니다. 부품에 마우스를 올리면 한국어 설명이 나오고, 단계 가이드와 탐구 질문이 들어 있습니다.

**바로 보기**: https://qksrhrrltnf-ctrl.github.io/anatomy-casebook/ (갤러리)

## 들어 있는 것

| 경로 | 내용 |
|---|---|
| `사례집/index.html` | 갤러리. 카드마다 교육판(다크·라이트)과 원본 링크가 있습니다. |
| `사례집/<이름>/<이름>-ko.html` | 한국어 교육판. 파일 하나로 인터넷 없이 열립니다. |
| `사례집/<이름>/<이름>.html` | 1단계 원본 재현본 |
| `사례집/<이름>/보고서.md`, `교육판-보고서.md` | 단계별 검증 보고서 |
| `PLAN.md`, `PLAN-2.md` | 1·2단계 계획서 |
| `작업기록.md` | 날짜별 작업 내역 |

예제 9종: 다이얼 게이지, 데스크 컴퓨터, 누름 버튼, 오락기, 랩터 엔진, 물결통, 3D 다이얼, 버튼 시험대, 반점판.

## 소스

교육판 소스는 원본 저장소의 포크인 [qksrhrrltnf-ctrl/anatomy](https://github.com/qksrhrrltnf-ctrl/anatomy/tree/edu-ko)의 `edu-ko` 브랜치에 있습니다. 빌드 방법은 그 저장소의 `edu/README.md`를 보십시오.

## 라이선스와 출처

그림과 공통 키트는 [wheresryan22/anatomy](https://github.com/wheresryan22/anatomy)(MIT License, Copyright (c) 2026 Ryan)에서 왔습니다. 한국어 교육 레이어와 교육판도 같은 MIT 조건을 따릅니다.
