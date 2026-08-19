## Design System — 디자인 시스템

여기 적힌 기본값을 적용한다. 의도적으로 정해졌고 검증된 값이므로, 사용자가 요청할 때만 덮어쓴다.

### 설계 메모 (Design Notes)
- **아이콘은 인라인 SVG를 쓰고 이모지는 쓰지 않는다.** 경로 기반의 단순한 SVG가 전문적으로 보인다. 아래 Icons 절 참조.
- **Chart.js 차트는 테마 전환 시 반드시 파기하고 다시 만들어야 한다** — CSS 변수만 바꿔서는 안 된다. 색은 렌더 시점에 읽히므로 새로 계산된 값으로 차트를 다시 만들어야 한다.
- **Chart.js: 기본 애니메이션을 끈다** — 차트를 만들기 전에 `Chart.defaults.animation = false;`를 넣는다. 기본 애니메이션 때문에 스크린샷, Playwright 테스트, 초기 렌더에서 차트가 빈 칸으로 보이거나 깨진다. 13~25라운드에서 "차트 깨짐"으로 반복되던 버그다.
- **Chart.js: 색은 명시적인 값으로** — CSS 변수 값에 16진수 투명도를 이어 붙이지 않는다 (예: `c.remote + '18'`). 대신 `rgba()` 값을 그대로 쓴다: `'rgba(12, 206, 107, 0.15)'`.
- **Chart.js: resetCanvas를 쓰지 않는다** — 같은 canvas 요소를 그대로 재사용한다. 이전 차트 인스턴스를 `.destroy()`로 없앤 다음 같은 canvas에 새로 만든다.
- **Chart.js: 만드는 시점** — `document.addEventListener('DOMContentLoaded', buildCharts)`로 만들고, 테마 전환 시에는 `onThemeChange()`에서 `requestAnimationFrame` + `setTimeout(100)`을 거쳐 다시 만든다. CSS 변수가 갱신된 뒤에 색을 읽기 위해서다.

### 타이포그래피 (Typography)
- **기본 글꼴:** Inter via Google Fonts CDN — `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap`
- **전문적인 자간 (Apple/Stripe 참고):**
  - **대표 제목 (h1):** `letter-spacing: -0.03em` — Apple 키노트 제목처럼 빡빡하게
  - **섹션 제목 (h2-h3):** `letter-spacing: -0.02em`
  - **본문:** `letter-spacing: -0.011em` — 가독성이 정제된다
  - **라벨·대문자:** 작은 대문자 텍스트에 `letter-spacing: 0.05em`
  - **글꼴 기능:** Inter의 대체 자형을 쓰려면 `font-feature-settings: 'cv11', 'ss01';`
- **글꼴 굵기 위계 (Stripe 참고):**
  - **h1:** `font-weight: 700` (800이 아니라 700 — 큰 크기에서 시각적으로 더 깔끔하다)
  - **h2:** `font-weight: 600` (세미볼드)
  - **카드 제목:** `font-weight: 600`
  - **본문:** `font-weight: 400` (레귤러)
  - **라벨:** `font-weight: 500` (미디엄)
- **글자 색 (Vercel 참고):** 순백은 쓰지 않는다. `#ededed` 또는 다크 모드에서 `#f5f5f7`로 매핑되는 `var(--text)`를 쓴다. 보조 글자는 불투명도 60% 느낌으로 (`#888` 또는 `var(--text-secondary)`)
- **제목에 그라디언트 텍스트 금지** — 단색만 쓴다. 그라디언트 텍스트는 크게 넣을수록 싸구려 보인다.
- **다국어 지원:** 내용에 라틴 문자가 아닌 글자(한국어, 일본어, 중국어 등)가 들어가면 알맞은 Google Fonts를 추가한다:
  - **한국어:** Noto Sans KR — `https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;600;700;800;900&display=swap`
  - **일본어:** Noto Sans JP — `https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;600;700;800;900&display=swap`
  - **중국어 (간체):** Noto Sans SC — `https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;600;700;800;900&display=swap`
  - **아랍어:** Noto Sans Arabic — `https://fonts.googleapis.com/css2?family=Noto+Sans+Arabic:wght@300;400;500;600;700;800;900&display=swap`
  - `font-family: 'Noto Sans KR', 'Inter', sans-serif;`로 설정한다 (CJK 글꼴을 앞에, 숫자·라틴은 Inter가 받는다)
  - `<html lang="ko">`로 설정한다 (또는 해당 언어 코드)
- **맞춤 글꼴:** 사용자가 특정 글꼴이나 분위기를 요청할 때:
  - **세리프·편집물 느낌:** Lora, Playfair Display, Source Serif Pro
  - **고정폭·코드:** JetBrains Mono, Fira Code, Source Code Pro
  - **디스플레이·창의적:** Space Grotesk, Outfit, Sora, Poppins
  - **손글씨:** Caveat, Patrick Hand
  - 항상 Google Fonts CDN으로 불러온다: `https://fonts.googleapis.com/css2?family=FONTNAME:wght@WEIGHTS&display=swap`
  - `--font-primary` CSS 변수와 `body { font-family: ... }`를 거기 맞게 고친다
- **라이트 모드 글자 최적화:** 강한 검정 대신 부드러운 글자 색을 쓴다:
  - 본문 글자: `#1a1a1a` (#000000이 아니다) — 가독성이 낫다
  - 보조 글자: `#666666` — 위계를 살린다
  - 흰 배경에 순수 검정 글자는 쓰지 않는다 — 너무 강하다
- **글꼴 자동 판단:** 맥락으로 알맞은 글꼴을 고른다:
  - 한국어·일본어·중국어 내용 → Noto Sans KR/JP/SC를 자동으로 추가
  - 코드가 많은 내용(치트시트) → 코드 블록에 JetBrains Mono 추가
  - 격식 있거나 편집물 성격의 내용 → 제목에 세리프 글꼴 검토
  - 경쾌하거나 창의적인 내용 → 디스플레이 글꼴 검토
- **고정폭 글꼴:** JetBrains Mono 또는 시스템 글꼴 `'SF Mono', 'Fira Code', 'Consolas', monospace`
- **대체 글꼴:** `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
- **크기 배율 (과감하게):** 대표 제목은 더 크게, 본문은 더 숨 쉬게 — 14 → 16 → 20 → 25 → 31 → 39 → 49px
- **행간:** 본문 1.5~1.7, 제목 1.1 (빡빡해야 전문적으로 보인다)
- **최대 줄 너비:** 가독성을 위해 65~75자
- 유동적인 반응형 크기에는 `clamp()`를 쓴다: `clamp(1rem, 2.5vw, 1.25rem)`

### 최신 CSS 기법 (Chrome 105+)

지원되는 환경에서는 아래 최신 CSS 기능을 써서 사용성을 높인다:

**Popover API** (Chrome 114+) — JS 없이 만드는 툴팁, 정보 패널, 모달:
```html
<button popovertarget="info-panel">ℹ Details</button>
<div id="info-panel" popover>
  <h3>More Information</h3>
  <p>Details shown on click, no JS needed.</p>
</div>
```
```css
[popover] {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 8px; padding: 16px; max-width: 320px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.2);
}
[popover]::backdrop { background: rgba(0,0,0,0.3); }
```
쓰는 곳: 대시보드 지표 상세, 아키텍처 노드 정보, 차트 주석.

**Exclusive `<details>` Accordion** (Chrome 120+) — JS 없는 접이식 섹션:
```html
<details name="faq" open><summary>Section 1</summary><p>Content</p></details>
<details name="faq"><summary>Section 2</summary><p>Content</p></details>
```
`name` 속성이 같으면 한 번에 하나만 열린다. 쓰는 곳: 치트시트, 절차 안내, FAQ 등 접었다 펼 수 있는 묶음이 있는 모든 내용.

**`::details-content` Styling** (Chrome 131+) — 아코디언 열고 닫기 애니메이션:
```css
details { overflow: hidden; }
::details-content {
  transition: block-size 0.3s ease, opacity 0.3s ease;
  block-size: 0; opacity: 0;
}
details[open]::details-content {
  block-size: auto; opacity: 1;
}
```

**CSS Anchor Positioning** (Chrome 125+) — 요소를 기준으로 툴팁 위치 잡기:
```css
.node { anchor-name: --node-tooltip; }
.tooltip {
  position: fixed; position-anchor: --node-tooltip;
  position-area: block-start; margin-bottom: 8px;
}
```
쓰는 곳: 아키텍처 다이어그램, 조직도, 위치를 잡아야 하는 주석이 필요한 모든 요소.

**Container Queries** — 뷰포트가 아니라 컨테이너 크기에 맞춰 요소 크기 결정:
```css
.card-container {
  container-type: inline-size;
  container-name: card;
}
@container card (width > 400px) {
  .card-title { font-size: clamp(1.25rem, 4cqi, 2rem); }
}
```

**:has() Parent Selector** — 자식 요소를 기준으로 부모에 스타일 적용:
```css
/* Style card if it contains an image */
.card:has(img) { padding-block: 2rem; }
/* Reduce spacing on headings followed by subheadings */
h1:has(+ h2) { margin-bottom: 0.25rem; }
```

**color-mix() Function** — 색을 동적으로 만들어 낸다:
```css
background: color-mix(in oklch, var(--accent), transparent 20%);
border-color: color-mix(in srgb, var(--text), var(--bg) 85%);
```

**light-dark() Function** (Chrome 123+) — 속성 하나로 테마 전환:
```css
:root { color-scheme: light dark; }
background: light-dark(white, #1a1a1a);
color: light-dark(#333, #fff);
```

**@starting-style** (Chrome 117+) — 새로 나타나는 요소의 진입 애니메이션:
```css
.modal {
  opacity: 1; transform: scale(1);
  transition: opacity 0.3s, transform 0.3s;
  @starting-style {
    opacity: 0; transform: scale(0.8);
  }
}
```

### 색 체계 (클래스 기반 테마 — NO @media prefers-color-scheme)

**CRITICAL: 테마 변수에 `@media (prefers-color-scheme)`를 쓰지 말 것.** 클래스 기반 `.theme-dark`/`.theme-light`와 충돌해서, OS 설정과 선택한 테마가 다를 때 테마가 깨진다. `html`에 붙는 클래스 선택자만 쓴다:

```css
/* Theme via class on <html> — JS detects prefers-color-scheme on first load */
html.theme-dark {
  --bg: #0A0A0A;              /* near-black (Linear-inspired) */
  --surface: #141414;          /* barely lighter than bg */
  --surface-hover: #1C1C1C;
  --border: rgba(255,255,255,0.04);  /* nearly invisible borders */
  --text: #EDEDED;             /* not pure white */
  --text-secondary: #888888;
  --accent: #3b82f6;
  --accent-secondary: #8b5cf6;
  --positive: #10b981;
  --negative: #f43f5e;
  --warning: #f59e0b;
  --info: #06b6d4;             /* semantic info color */
  --muted: #0f0f0f;            /* subtle backgrounds */
}
html.theme-light {
  --bg: #FAFAF9;               /* warm off-white */
  --surface: #FFFFFF;
  --surface-hover: #F5F5F4;
  --border: #e5e5e5;           /* more visible than 0.06 opacity */
  --text: #1a1a1a;             /* softer than pure black */
  --text-secondary: #666666;   /* better contrast than #64748b */
  --accent: #2563eb;
  --accent-secondary: #7c3aed;
  --positive: #059669;
  --negative: #e11d48;
  --warning: #d97706;
  --info: #0ea5e9;             /* semantic info color */
  --muted: #f8fafc;            /* subtle backgrounds */
}
```

**JS 테마 초기화**는 첫 방문 시 OS 설정을 감지하고, 이후에는 localStorage 값을 우선한다:
```javascript
var saved = localStorage.getItem('viz-theme');
var initial = saved || (window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark');
applyTheme(initial);
```

차트 색 순서: `#3b82f6, #8b5cf6, #ec4899, #f59e0b, #10b981, #06b6d4, #f43f5e`

### 의미 있는 색 사용 (Semantic Color Usage)

장식이 아니라 의미를 담아 색을 쓴다:

**성공·긍정 지표** — `var(--positive)` (초록):
- 매출 성장, 사용자 유치, 완료율
- 위쪽 화살표, 양수 퍼센트, "양호" 상태 표시

**경고·주의** — `var(--warning)` (앰버):
- 임계값에 가까워진 지표, 대기 상태
- 중립적 알림, "확인 필요" 표시

**오류·부정 지표** — `var(--negative)` (빨강):
- 하락하는 지표, 실패, 심각 알림
- 아래쪽 화살표, 음수 퍼센트, 오류 상태

**정보·주요 동작** — `var(--info)` (파랑):
- 주요 행동 유도 버튼, 정보성 강조, 절차 단계

**강조색 절제 규칙**:
- **카드 4개 이상인 KPI 그리드:** 강조색은 최대 2개 — 가장 중요한 지표 하나에만 `var(--accent)`, 나머지는 `var(--text)`
- **숫자에 임의로 색을 입히지 않는다** — 색은 반드시 의미를 나타내야 한다
- **변화량 표시에만:** `var(--positive)`/`var(--negative)`는 화살표와 퍼센트에 쓰고, 카드의 주요 수치에는 쓰지 않는다

### 카드 체계 (Professional Card System)

단순한 테두리를 넘어선 현대적 카드 스타일:

```css
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;                    /* Larger than 8px for premium feel */
  box-shadow: 0 1px 3px rgba(0,0,0,0.05); /* Subtle depth in light mode */
  padding: 24px;                          /* Generous internal spacing */
  transition: all 0.15s ease;
}

/* Light mode: visible shadows */
.theme-light .card {
  box-shadow: 0 1px 3px rgba(0,0,0,0.05), 0 0 0 1px rgba(0,0,0,0.03);
}

/* Dark mode: border emphasis */
.theme-dark .card {
  box-shadow: 0 0 0 1px var(--border);
}

.card:hover {
  /* Light mode: deeper shadow */
  box-shadow: 0 4px 16px rgba(0,0,0,0.08), 0 0 0 1px rgba(0,0,0,0.03);
}

.theme-dark .card:hover {
  /* Dark mode: brighter border */
  box-shadow: 0 0 0 1px rgba(255,255,255,0.08);
}
```

**핵심 원칙:**
- 고급스러운 느낌을 위해 모서리 반지름 12px (8px가 아니다)
- 라이트 모드는 그림자를, 다크 모드는 테두리를 살린다
- 내부 여백은 24px로 일정하게
- 호버 상태는 기존 시각 스타일을 강화하는 방향으로

### Chart.js 전문가용 스타일링

라이브러리 기본값을 넘어 모든 차트에 적용한다:

```css
.chart-card {
  padding: 24px;
  background: var(--surface);
  border-radius: 12px;
  border: 1px solid var(--border);
}
.chart-wrap {
  position: relative;     /* 없으면 높이가 계속 늘어난다 */
  height: 360px;          /* height를 쓴다. min-height는 쓰지 않는다 */
}
```

**차트 설정 보강** (동작 규칙과 복사용 코드는 SKILL.md의 **Chart.js Integration Rules**를 볼 것):
- 여백 지정: `layout: { padding: 20 }`
- 막대 모서리 둥글게: `borderRadius: 4`
- 격자선은 아주 희미하게: 다크 `rgba(255,255,255,0.04)`, 라이트 `rgba(0,0,0,0.06)`. `var(--border)` 값을 그대로 써도 된다
- 글꼴 크기: 축 눈금 13px 이상, 축 제목 14px, 차트 제목 16px 이상, 범례 13px
- 축 라벨은 가로로: `maxRotation: 0`. 라벨이 넘치면 개수를 줄인다
- 선 차트 점: 기본 반지름 0, 호버 6
- 도넛·원 차트: 조각에 퍼센트 라벨을 항상 넣는다
- 계열 색은 서로 뚜렷이 다르게 — 접근성과 판독성 양쪽에 필요하다
- 테마 강조색과 어울리는 색 팔레트

### 간격 (Spacing)
- **8px 그리드** — 모든 간격은 배수로: 4, 8, 12, 16, 24, 32, 48, 64, 96px
- **여유 있는 여백** — 카드는 `p-6`~`p-8`, 컨테이너는 `px-8`
- **컨테이너:** `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`
- **카드 간격:** 최소 `gap-6`

### 모바일 반응형 (Critical)
**모든 시각화는 모바일에서 흔들림 없이 동작해야 한다. 가로 오버플로는 허용되지 않는다.**

```css
/* Required mobile breakpoints */
@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr; /* Single column on tablet */
    gap: 1rem; /* Reduced gap */
  }
  .chart-section {
    grid-template-columns: 1fr; /* Stack chart sections */
  }
  .container {
    padding: 1rem; /* Reduced container padding */
  }
}

@media (max-width: 375px) {
  .container {
    padding: 0.75rem; /* Minimal padding on small phones */
  }
  .kpi-card, .chart-card {
    padding: 1rem; /* Reduced card padding */
  }
  .filter-toolbar {
    flex-direction: column; /* Stack filters vertically */
    align-items: stretch;
  }
}
```

**점검 목록:**
- ✅ 768px 뷰포트에서 가로 스크롤 없음
- ✅ 375px 뷰포트에서 가로 스크롤 없음  
- ✅ 모든 글자가 읽힌다 (최소 16px)
- ✅ 터치 대상 영역이 44px 이상
- ✅ 차트가 알맞게 크기 조정됨

### 카드 호버 마이크로인터랙션
모든 카드에 은은한 호버 효과를 둔다 — 그림자 높이기만 하고 변형은 쓰지 않는다:
```css
.card, .stat-card, .kpi-card, .stat-item, .chart-card {
  transition: box-shadow 0.2s ease;
}
.card:hover, .stat-card:hover, .kpi-card:hover, .stat-item:hover {
  box-shadow: 0 0 0 1px var(--border), 0 8px 16px rgba(0,0,0,0.08);
}
```
- 카드 호버에 `translateY`나 `scale` 쓰지 않기 — 싸구려 보인다
- 타임라인 항목: 호버 시 은은한 배경 강조
- 아키텍처 노드: 호버 시 은은한 그림자 높이기
- 목록 항목: 호버 시 translateX가 아니라 은은한 배경 톤 변화

### :focus-visible 표준
모든 파일이 반드시 포함해야 한다:
```css
*:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
  border-radius: 4px;
}
```

### SVG 차트 호버 패턴
인라인 SVG 차트 요소(막대, 도넛 조각, 데이터 지점)에:
```css
svg rect, svg circle, svg path.data-element {
  transition: opacity 0.2s, transform 0.2s;
}
svg rect:hover, svg circle:hover {
  opacity: 0.8;
  filter: brightness(1.1);
}
```
브라우저 기본 툴팁을 위해 SVG 도형 안에 `<title>` 요소를 항상 넣는다:
```html
<rect x="10" y="20" width="50" height="100">
  <title>Revenue: $142K</title>
</rect>
```

### 시각적 완성도 (Stripe/Vercel 수준)
- **모서리 반지름:** 일관되게 `8px` (12px도 16px도 아니다 — Stripe가 8px를 쓴다)
- **그림자 (다크 모드):** 거의 쓰지 않는다 — `box-shadow: 0 0 0 1px var(--border)`면 충분하다. 테두리가 일하게 둔다.
- **그림자 (라이트 모드):** 은은한 겹 — `box-shadow: 0 0 0 1px rgba(0,0,0,0.03), 0 2px 4px rgba(0,0,0,0.05)`
- **카드 호버:** 그림자만 살짝 진해진다. NO translateY, NO scale transforms. 이것만: `box-shadow: 0 0 0 1px rgba(0,0,0,0.03), 0 8px 16px rgba(0,0,0,0.08)`
- **깔끔한 카드 테두리:** `border: 1px solid var(--border)` — 그라디언트 테두리 금지, 왼쪽·윗쪽 색 테두리 금지
- **글래스 모피즘:** 아껴서 쓴다. 떠 있는 UI 요소(메뉴, 툴팁)에만 쓰고 카드에는 쓰지 않는다
- **강조색 절제:** 섹션당 한 가지에만 강조색을 쓴다 (버튼 하나, 링크 하나, 아이콘 하나) — 여기저기 쓰지 않는다
- **전환:** `transition: box-shadow 0.2s ease` — 변하는 것만 애니메이션한다

### 배경 분위기 (흔한 다크를 피할 것)
모든 시각화에는 은은한 배경 개성이 있어야 한다. 흔한 다크 템플릿처럼 보이는 밋밋한 `--bg` 배경은 피한다. 파일당 한 가지 기법만 고른다:

1. **은은한 방사형 그라디언트** — 가운데에서 퍼지는 아주 희미한 방사형 그라디언트 하나:
   ```css
   body { background: var(--bg); }
   body::before {
     content: ''; position: fixed; inset: 0; z-index: -1;
     background: radial-gradient(ellipse 80% 50% at 50% 20%, 
       color-mix(in srgb, var(--accent), transparent 92%), transparent);
   }
   ```
2. **노이즈·입자감 질감** (Vercel 방식): 작은 인라인 SVG 노이즈 필터를 쓴다:
   ```css
   body::after {
     content: ''; position: fixed; inset: 0; z-index: -1; opacity: 0.03;
     background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
   }
   ```
3. **점 격자** (아껴서 쓰고, 기술·아키텍처 문서에만):
   ```css
   body { background-image: radial-gradient(circle, rgba(255,255,255,0.03) 1px, transparent 1px);
     background-size: 24px 24px; }
   ```

내용의 성격에 맞는 기법을 고른다. 타임라인 → 방사형 그라디언트. 대시보드 → 입자감. 아키텍처 → 점 격자. 슬라이드 덱 → 내용에 맞는 강조색을 쓴 방사형 그라디언트.

### 드롭다운 메뉴 스타일 (Mandatory)
설정·내보내기 드롭다운은 반드시 다듬어진 모습이어야 한다:
```css
.dropdown-menu {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.3), 0 0 0 1px rgba(255,255,255,0.05);
  padding: 4px;
  min-width: 160px;
  animation: dropdownIn 0.15s ease;
}
.dropdown-menu button {
  width: 100%; text-align: left; padding: 8px 12px;
  border-radius: 6px; border: none; background: transparent;
  color: var(--text-secondary); font-size: 0.875rem; cursor: pointer;
  transition: background 0.15s, color 0.15s;
}
.dropdown-menu button:hover {
  background: var(--surface-hover); color: var(--text);
}
@keyframes dropdownIn {
  from { opacity: 0; transform: translateY(-4px); }
  to { opacity: 1; transform: translateY(0); }
}
```

### 진입 애니메이션 (모든 파일에 Mandatory)
모든 파일에 은은한 진입 애니메이션이 반드시 있어야 한다. 정적인 느낌의 페이지는 인터랙션 점수가 낮다. 페이지 로드 시 CSS `@keyframes`만 쓴다:
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}
.card, .step, .quote-card, section > * {
  animation: fadeInUp 0.5s ease both;
}
/* Stagger children */
.card:nth-child(1) { animation-delay: 0s; }
.card:nth-child(2) { animation-delay: 0.08s; }
.card:nth-child(3) { animation-delay: 0.16s; }
.card:nth-child(4) { animation-delay: 0.24s; }
```
치트시트나 인용 카드처럼 "정적인" 내용을 포함해 모든 카드·항목에 호버 상태를 넣는다:
```css
.card:hover, .command-group:hover, blockquote:hover {
  background: var(--surface-hover);
  box-shadow: 0 0 0 1px var(--border), 0 8px 16px rgba(0,0,0,0.08);
}
```

### 테마에 반응하는 콘텐츠 (캐러셀, 포스터)
카드가 장식용 그라디언트를 쓸 때(예: 인스타그램풍 파스텔 슬라이드), 그라디언트는 반드시 테마에 따라 바뀌어야 한다. 다크와 라이트에서 똑같아 보이는 고정 파스텔 색은 쓰지 않는다:
```css
/* BAD — same gradient in both themes */
.card { background: linear-gradient(135deg, #ff9a9e, #fecfef); }

/* GOOD — theme-adaptive gradients */
.theme-dark .card-1 { background: linear-gradient(135deg, #1a1a2e, #16213e); }
.theme-light .card-1 { background: linear-gradient(135deg, #ff9a9e, #fecfef); }
```

### 슬라이드 덱의 라이트 모드
슬라이드 덱은 다크를 먼저 생각하고 설계하지만, 라이트 모드가 곁다리로 느껴져서는 안 된다. 라이트 모드 발표에서는:
- 순백 대신 은은한 따뜻한 회색 배경(`#f5f5f0`)을 쓴다
- 깊이감을 위해 위에서 아래로 희미한 그라디언트를 넣는다
- 제목은 충분한 굵기의 어두운 글자를 쓴다
- 격자·발광 배경은 라이트 모드에서 은은한 점 패턴이나 부드러운 그라디언트로 바꾸어야 한다

### 차트 접근성 (Mandatory)
CSS로만 만든 차트(막대, 레이더, 도넛)와 Chart.js 차트는 모두 반드시 스크린 리더에 데이터를 노출해야 한다:
```html
<div role="img" aria-label="Bar chart showing React at 85%, Vue at 72%, Angular at 58%">
  <!-- visual chart here -->
  <div class="sr-only">React: 85%. Vue: 72%. Angular: 58%.</div>
</div>
```
모든 파일에 `.sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0,0,0,0); }`를 추가한다.

### 시각적 절제 — 안티패턴 (NEVER DO)
- ❌ **떠다니는 그라디언트 구** — 내용 뒤에 깔아 둔 장식용 원은 아마추어처럼 보인다
- ❌ **무지개·그라디언트 테두리** — 카드의 윗줄·왼줄 색 테두리는 "템플릿"이라고 소리치는 꼴이다
- ❌ **제목의 그라디언트 텍스트** — 단색을 쓴다. 그라디언트 텍스트는 2020년대 유행이고 오래 가지 못했다
- ❌ **호버 시 확대 변형** — 카드에 `scale(1.02)`를 주면 고급스럽기는커녕 어설프다
- ❌ **발광 효과** — `box-shadow: 0 0 20px rgba(blue)`는 예뻐 보인 적이 없다
- ❌ **장식용 애니메이션** — 도는 고리, 떠다니는 입자, 깜빡이는 점은 소음이다
- ❌ **색으로 구분한 테두리** — 카드 왼쪽·윗쪽 색 테두리는 Bootstrap 컴포넌트 같아 보인다
- ❌ **그라디언트 텍스트를 쓴 통계 숫자** — 단색 강조색이나 var(--text)를 쓴다

### 접근성 (Mandatory)

모든 시각화는 아래 기본 접근성 요구사항을 반드시 충족해야 한다:

**최소 접근성 점검 목록:**
- [ ] Skip-to-content 링크가 있다
- [ ] 모든 주요 섹션에 `aria-label`과 함께 `role="region"`
- [ ] 비교 섹션, 아키텍처 계층, 슬라이드 묶음에 `role="group"`
- [ ] 타임라인 섹션과 항목에 `role="list"` / `role="listitem"`
- [ ] 아이콘만 있는 모든 버튼에 `aria-label`
- [ ] 차트 섹션에 데이터 설명을 가리키는 `aria-describedby`
- [ ] 모든 인터랙티브 요소에 `border-radius: 4px`를 가진 `:focus-visible`
- [ ] 슬라이드 카운터·동적 콘텐츠에 `aria-live="polite"`

- **Skip navigation:** `<body>` 맨 위에 `<a href="#main-content" class="skip-link">Skip to content</a>`를 넣는다. 스타일은 평소에 숨기고 포커스를 받을 때 보이게.
- **랜드마크 역할:** `<main>`, `<nav>`, `<header>`, `<footer>`, `<section>`을 쓰고, 같은 랜드마크가 여럿이면 `aria-label`을 붙인다.
- **인터랙티브 요소:** 버튼·링크·컨트롤의 글자만으로 설명이 되지 않으면(예: 아이콘만 있는 버튼) `aria-label`을 반드시 붙인다.
- **포커스 표시:** 모든 인터랙티브 요소에 눈에 보이는 `:focus-visible` 스타일을 넣는다: `outline: 2px solid var(--accent); outline-offset: 2px;`
- **색으로만 된 표시:** 상태 점, 색 배지 등은 반드시 글자 대체물을 갖춰야 한다. 예를 들어 초록 상태 점에는 "Healthy" 글자나 `aria-label="Status: Healthy"`를 함께 둔다.
- **차트와 다이어그램 (MANDATORY):** 
  - 차트 canvas를 `role="img" aria-label="Description of what the chart shows"`를 가진 컨테이너로 감싼다
  - **모든 차트에 호버 툴팁이 켜져 있어야 한다** — Chart.js 툴팁을 끄지 않는다
  - 스크린 리더를 위해 데이터 표 대체물이나 시각적으로 숨긴 요약을 넣는다
  - 데이터 계열 간 색 차이가 충분한 고대비 색을 쓴다
- **스크린 리더 설명:** 복잡한 시각화에는 핵심 내용을 설명하는 `aria-description`이나 시각적으로 숨긴 글을 추가한다.
- **슬라이드 덱:** 슬라이드 카운터에 `aria-live="polite"`를, 내비게이션 버튼에 `aria-label`을 둔다.

### 아이콘 (인라인 SVG만)

모든 아이콘은 인라인 SVG로 넣는다. **이모지를 아이콘으로 쓰지 않는다** — 전문적이지 않아 보이고 환경마다 다르게 렌더된다.

단순한 Lucide 스타일 경로를 쓴다: 24x24 viewBox, 선 기반, `stroke="currentColor" fill="none" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"`.

자주 쓰는 아이콘:
- 테마 전환은 달·해
- PNG 내보내기는 내려받기 화살표  
- 인쇄는 프린터
- 내비게이션은 좌우 화살표
- 장단점은 체크·X
- 기기 종류는 지구·스마트폰·모니터

### 애니메이션 (CSS 우선 + 최신 기능)

**CSS 애니메이션이 주 수단이다.** 안정적이고 성능이 좋으며 JS 스코프 문제로 깨지지 않는다.

전체 패턴은 [references/animations.md](references/animations.md)를 보라.

**최신 CSS 애니메이션 기능 (점진적 향상):**

**Scroll-driven animations** (Chrome 115+) — JS 스크롤 리스너를 대체한다:
```css
.scroll-reveal {
  animation: fadeInUp 1ms linear;
  animation-timeline: view(); /* Animates based on viewport visibility */
}
.progress-bar {
  animation: grow 1ms linear;  
  animation-timeline: scroll(root inline); /* Animates based on page scroll */
}
```

**세 가지 애니메이션 기법 (전부 스켈레톤에 들어 있다):**

1. **페이지 로드 진입:** `animate` 클래스를 붙인다 (시차를 두려면 `delay-1`~`delay-6` 추가)
   ```html
   <h1 class="animate">Title</h1>
   <div class="card animate delay-1">Card 1</div>
   <div class="card animate delay-2">Card 2</div>
   ```

2. **스크롤 등장:** `data-reveal` 속성을 붙인다. JS가 `.reveal` 클래스(opacity:0)를 붙이고, 스크롤 시 `.visible`을 추가한다.
   ```html
   <section data-reveal>This fades in when scrolled into view</section>
   ```

3. **숫자 카운터:** `data-count` 속성을 붙인다. JS가 0에서 목표값까지 애니메이션한다.
   ```html
   <span data-count="77" data-suffix="%">77%</span>
   ```

**호버 효과**는 순수 CSS로 `.card`에 들어 있다 (:hover에 translateY + scale).

**규칙:**
- 내용은 CSS 상태에서 ALWAYS 보인다 — JS 애니메이션은 점진적 향상일 뿐이다
- 페이지 로드 애니메이션은 `@keyframes`, 호버·상태 변화는 CSS `transition`을 쓴다
- JS가 실패해도 `data-reveal` 요소는 최종 내용을 보여 준다 (빈 섹션이 생기지 않는다)
- `prefers-reduced-motion`이 모든 애니메이션을 자동으로 끈다
- `data-reveal` 요소는 CSS 기본 상태가 반드시 `opacity: 1`이어야 한다. JS가 `.reveal` 클래스(`opacity: 0`)를 붙이고 `.visible`이 되돌린다. JS가 실패하면 내용은 그대로 보인다.
- 페이지 로드 후 짧은 지연(500ms)을 두고 모든 reveal 요소를 보이게 만든다. 그래야 전체 페이지 스크린샷과 PNG 내보내기가 모든 내용을 담는다:
  ```javascript
  setTimeout(function() { document.querySelectorAll('.reveal').forEach(function(el) { el.classList.add('visible'); }); }, 500);
  ```

