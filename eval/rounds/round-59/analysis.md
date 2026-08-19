# Round 59 Analysis

## 목적

이번 라운드는 통상적인 개선 라운드가 아니다. 커밋 `c1a0b47`의 **SKILL.md 정리(6,541 →
4,940단어)가 해를 끼쳤는지** 확인하는 것이 유일한 목적이다. 판정 기준은 생성 결과를 보기
전에 `criteria.md`에 기록했다.

## 결과

| 파일 | 형식 | L2 | L3 | 종합 |
|---|---|---|---|---|
| director-briefing.html | slide-deck | 100 | 8.92 | **9.36** |
| quality-dashboard.html | dashboard | 97 | 8.60 | 9.04 |
| marketing-kpi-dashboard.html | dashboard | 97 | 8.50 | 8.98 |
| improvement-timeline.html | (timeline) | 96 | 8.55 | 8.97 |
| unicode-stats-info.html | (infographic) | 97 | 7.88 | 8.61 |
| patient-safety-info.html | (infographic) | 97 | 7.22 | **8.21** |

**평균 8.86 / 최저 8.21 / 게이트 ACCEPTABLE** (L2만 보면 9.73, VIRAL)

## 판정

| # | 조건 | 결과 |
|---|---|---|
| 1 | 평균이 R58(8.44) 아래로 떨어지지 않을 것 | **통과** 8.86 |
| 2 | `Cannot use import statement outside a module` 0건 | **통과** — 6건 전부 콘솔 오류 0 |
| 3 | 차트 체크 6개 전부 통과 | **통과** — 실패 0건 |
| 4 | R57형 실패(스켈레톤 미사용) 없을 것 | **통과** — 메뉴·테마·인쇄 전부 통과 |

**정리는 해를 끼치지 않았다.**

+0.42는 미리 정한 ±0.5 잡음 범위 안이다. **개선으로 기록하지 않는다.** 차트가 정상
동작한 것도 정리 덕분이 아니라 `f01c7a1`의 CDN 수정 덕분이며, 그 사실은 편집 전에 따로
확인했다 (8월 생성물 10건, 오류 0건).

## 발견과 조치

### 1. `no-overflow-375` — 원인 두 개, 둘 다 수정 [SKILL]

3건이 실패했고 baseline `examples/`에서도 4건이 실패하던 항목이다. 측정해 보니 원인이
둘이었다.

**(가) 차트 캔버스가 그리드 셀을 밀어낸다 (오버플로 180~330px)**
`div.chart-card`가 w=540인데 parentW=343이었다. Chart.js가 canvas에 인라인 `width`를
박고, 그리드·플렉스 항목의 기본값 `min-width: auto`가 그 너비에 밀려 셀 밖으로 커진다.
창을 넓게 열었다가 좁히면 차트가 줄어들지 않는다.
→ `canvas { max-width: 100% }` + 카드 `min-width: 0`을 정식 패턴에 MANDATORY로 추가.

**(나) 규칙 자체가 효과가 없었다 (오버플로 5~25px)**
SKILL.md는 `@media (max-width: 375px) { body { overflow-x: hidden; } }`를 지시했는데,
체크는 `document.documentElement.scrollWidth`를 잰다. `body`만으로는 **아무 효과가
없다.** 스킬이 통과할 수 없는 규칙을 지시하고 있었다.
→ `html, body`로 정정하고 이유를 본문에 명시.

**검증:** 두 수정을 합쳐 7개 파일(생성물 3 + examples 4)에 적용한 결과 **7/7 통과**
(556→375, 703→375, 400→375, 399→375, 386→375, 380→375, 375 유지).

### 2. 첫 화면 아래 내용이 PNG 내려받기에서 빠진다 [REF:skeleton]

`patient-safety-info.html`은 L2 97점인데 스크린샷에서 하단 75%가 비어 있다. 내용은 DOM에
다 있고 `.reveal { opacity: 0 }` + IntersectionObserver 때문에 안 보이는 것이다.

- 스크롤하는 사람에게는 정상이고, `@media print`에는 이미 `opacity: 1 !important`가 있다.
- 그러나 `html-to-image`는 DOM을 그대로 캡처하므로 **내려받은 PNG가 대부분 빈 칸이 된다.**
  스킬이 그 내려받기 버튼을 필수로 요구한다.

→ `skeleton.md`의 `downloadImage()`가 캡처 전에 `.reveal:not(.visible)`을 잠시 보이게
하고 캡처 후 되돌리도록 수정. 메뉴를 숨겼다 되돌리는 기존 패턴과 같은 방식이다.

**이것은 이번 정리로 생긴 회귀가 아니다.** 규칙(SKILL.md 155행, `data-reveal`은 첫 화면
아래에만 최대 3~4개 섹션)은 그대로 살아 있고, 이 파일은 정확히 4개를 썼다.
`skeleton.md`의 reveal 처리는 `git diff` 기준 한 줄도 바뀌지 않았다.

### 3. details 라벨 중복 [REF:skeleton]

`improvement-timeline.html`의 카드 7개 전부 "자세히 보기 자세히 보기"로 나온다.
`<summary>`의 텍스트 노드와 `summary::after { content: '자세히 보기' }`가 겹쳤다.
스킬이 가르친 패턴은 아니고 생성물이 스스로 만든 것이지만, 스켈레톤이 details 패턴을
소유하므로 거기에 정식 토글 라벨과 주의 한 줄을 넣었다.

**표본 1건이므로 일반화하지 않는다.** 다음 라운드에서 재발하는지 본다.

### 4. Layer 1 형식 판정 오류 [보류]

`improvement-timeline`(타임라인)과 `unicode-stats-info`·`patient-safety-info`
(인포그래픽)가 전부 `dashboard`로 판정됐다. 점수에 직접 반영되지는 않지만 형식별 체크가
잘못 적용된다. `format-detect.js` 문제이며 이번 범위 밖으로 남긴다.

## 하네스 결함 (스킬 문제 아님)

`marketing-kpi-dashboard`가 `$124.5K`를 `$24.5K`로, `CAC $47.80`을 `$7.80`으로 렌더했다.
처음에는 내용 정확도 실패로 봤으나, **생성 스크립트의 히어독이 `\$124.5K`를 `\24.5K`로
망가뜨린 것**이었다. 모델은 받은 그대로 정확히 렌더했다. 스킬 결함으로 기록하지 않으며,
이를 근거로 체크리스트를 추가하지도 않는다 — 잘못된 전제 위에 규칙을 쌓는 것이 바로
R50~R58에서 반복된 실수다.

다음 라운드 교훈: **`$`가 들어간 프롬프트는 히어독 대신 파일로 전달할 것.**

## 이번 라운드에서 하지 않은 것

`loopsSinceResearch = 5`로 연구 단계 조건에 걸리지만 건너뛰었다. 연구 단계는 SKILL.md에
내용을 **추가**하는 작업이라 "정리가 해를 끼쳤는가"라는 판정을 오염시킨다. 다음 라운드로
미룬다.
