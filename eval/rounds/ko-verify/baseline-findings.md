# 기준선 측정에서 나온 발견 — 번역과 무관한 기존 결함

한글화 검증의 대조군(`before/`)을 만드는 과정에서 드러난 것들이다.
**번역 때문에 생긴 문제가 아니라 현재 영문 스킬이 가진 결함이다.**

---

## 발견 1 — 차트가 있는 대시보드가 로드 시 크래시한다 (심각)

`before/saas-q3-dashboard.html` 1차 생성분이 Layer 2 평가 중 예외로 중단됐다.

```
TypeError: Cannot read properties of undefined (reading 'safeInit')
  at buildRevenueChart (saas-q3-dashboard.html:547)
  at onThemeChange (saas-q3-dashboard.html:605)
  at applyTheme (saas-q3-dashboard.html:436)
```

### 원인 — 스켈레톤의 실행 순서

| 위치 | 코드 | 시점 |
|---|---|---|
| `skeleton.md:247` | `applyTheme(currentTheme);` | 스크립트 로드 즉시 실행 |
| `skeleton.md:244` | `if (typeof onThemeChange === 'function') onThemeChange();` | 위에서 호출됨 |
| `skeleton.md:303` | `// === YOUR SCRIPTS BELOW ===` | 사용자 코드는 여기 아래 |
| `SKILL.md:54` | `var ChartManager = { ... }` 를 쓰라고 지시 | 결국 항상 247줄 **뒤**에 놓인다 |

`onThemeChange`는 함수 선언이라 호이스팅되어 호출은 된다. 그러나 `ChartManager`는
`var`로 선언되어 이 시점에 `undefined`다. 그래서 `undefined.safeInit()`이 된다.

**`var` 규칙은 이 문제를 못 막는다.** `var`는 TDZ `ReferenceError`를 `undefined` 접근으로
바꿀 뿐이다. 조용히 실패하는 쪽으로 바뀐 것이지 안전해진 것이 아니다.

가드 `if (typeof onThemeChange === 'function')`는 **함수의 존재만** 확인하고 그 함수가
의존하는 객체가 준비됐는지는 확인하지 않는다.

`skeleton.md:300`이 제시하는 대안 패턴도 같은 문제를 갖는다:
```javascript
// function onThemeChange() { chartsBuilt = false; buildCharts(); }
```
`buildCharts`가 `ChartManager`를 건드리면 동일하게 깨진다.

### 재현성

2차 생성분에서 재현 여부를 확인 중. 구조적 원인이므로 재현될 가능성이 높다.

### 고칠 위치 (번역과 별개 작업)

`skeleton.md`의 `applyTheme(currentTheme);` 즉시 호출을 `DOMContentLoaded` 이후로 미루거나,
`onThemeChange` 호출을 첫 적용 시에는 건너뛰도록 한다. 후자가 더 작은 수정이다.

---

## 발견 2 — `examples/` 17개는 현재 스킬을 검증하지 못한다

`examples/` 중 `ChartManager`를 쓰는 파일이 **하나도 없다.** 이 지시는 나중에 SKILL.md에
추가되었고 예시들은 그 이전에 만들어졌다.

즉 `examples/`를 대상으로 한 평가(평균 9.21)는 **현재 지시문이 만들어 내는 결과물이
아니라 과거 결과물을 채점한 것**이다. 회귀 검증용 코퍼스로서는 낡았다.

이번 한글화 검증에서 `examples/`를 쓰는 목적은 다르다 — "번역이 코드 블록을 깨뜨렸는가"를
보는 용도이므로 여전히 유효하다. 다만 스킬 품질 측정용으로는 신뢰할 수 없다.

---

## 발견 3 — 출력 경로가 세 곳으로 갈린다

`claude -p`를 같은 작업 디렉터리에서 실행했는데 결과 파일이 매번 다른 곳에 떨어졌다.

| 프롬프트 | 저장 위치 |
|---|---|
| P1 (대시보드) | 현재 작업 디렉터리 |
| P2 (슬라이드 덱) | `C:\Users\kim_dongik\Downloads\` |
| P3 (소셜 카드) | `D:\Agent\visualize\` (저장소 루트) |
| P4 (타임라인) | 현재 작업 디렉터리 |
| P5 (비교) | 현재 작업 디렉터리 |

`SKILL.md:156`은 `~/Downloads/`에 쓰라고 하는데 실제로는 세 곳으로 흩어진다.

**이것이 개선 루프를 조용히 망가뜨린다.** `LOOP-PROMPT.md` Step 2가
`node run.js --dir .../generated/` 로 채점하므로, 다른 곳에 떨어진 파일은 평가에서
그냥 빠진다. 오류도 안 나고 라운드가 파일 수만 줄어든 채로 진행된다.

이번 검증에서는 생성 후 세 위치를 훑어 모으는 방식으로 우회했다.

---

## 기준선 점수 (1차)

| 파일 | 형식 | Layer 2 |
|---|---|---:|
| `2026-h1-roadmap.html` | dashboard | 100 |
| `blog-announce-card.html` | generic | 100 |
| `deploy-tool-pitch.html` | slide-deck | 100 |
| `docs-tool-comparison.html` | comparison | 100 |
| `saas-q3-dashboard.html` | unknown | **0 (평가 중 크래시)** |

크래시한 파일의 0점은 "0점짜리 품질"이 아니라 "평가하지 못함"이다. 둘을 섞으면
평균이 8.00으로 나오는데 이 숫자는 의미가 없다. 나머지 4개 기준으로는 **100/100**이다.

`2026-h1-roadmap.html`이 타임라인을 요청했는데 `dashboard`로 판정된 것은 Layer 1
형식 판정의 한계다. 점수에는 영향이 없다.
