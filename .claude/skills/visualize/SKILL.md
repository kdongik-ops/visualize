---
name: visualize
description: >
  Create beautiful, self-contained HTML visualizations from any content or idea.
  Use for: slide decks, presentations, infographics, dashboards, flowcharts, diagrams,
  timelines, comparison tables, data visualizations, landing pages, one-pagers, org charts,
  mind maps, process flows, kanban boards, report summaries, or any visual that helps
  humans digest information faster. Trigger on requests like "visualize this," "make a deck,"
  "create a slide," "build an infographic," "show me a dashboard," "make this visual,"
  or any request to present information in a visual HTML format.
license: MIT
metadata:
  author: kdongik-ops
  version: 0.3.0
  category: document-creation
  tags: [visualization, html, slides, dashboard, infographic]
---

# Visualize

어떤 아이디어, 데이터, 내용이든 단일 파일 HTML 시각화로 바꾼다.

## 파일을 만든 뒤에 (After Creating a File)

**HTML 파일을 쓴 다음 아래 두 가지를 항상 함께 한다:**

1. **브라우저에서 자동으로 연다:** `open <filename>.html` (macOS), `xdg-open <filename>.html` (Linux), 또는 `Start-Process <filename>.html` (Windows PowerShell)을 실행해 사용자가 바로 볼 수 있게 한다
2. **파일 경로를 클릭 가능한 URL로 돌려준다:** 응답에 `file://<absolute-path>`를 넣어 사용자가 눌러서 열 수 있게 한다

생성 후 응답 예시:
```
Created your visualization! Opening in browser now...
📄 file:///Users/you/project/my-dashboard.html
```

## Critical Requirements — 필수 요구사항 (NON-NEGOTIABLE)

⚠️ **EVALUATION FAILURE GUARANTEED WITHOUT THESE 9 ELEMENTS** ⚠️

**EVERY file MUST start from the skeleton template in [references/skeleton.md](references/skeleton.md) — 템플릿 ENTIRE 전체를 복사한 다음 내용을 추가한다.**

1. **CSS 사용자 정의 속성:** 아래 이름을 정확히 써야 한다: `--bg, --surface, --surface-hover, --border, --text, --text-secondary, --accent, --accent-secondary, --positive, --negative, --warning` — 다른 이름은 NO (--bg-primary도, --text-primary도 안 된다). **CRITICAL:** 평가 시스템과 맞물리려면 이 속성 이름이 정확해야 한다.
2. **유틸리티 메뉴 시스템 (MANDATORY):** `.viz-menu-toggle` 버튼, `.viz-menu-dropdown` 컨테이너, PNG 내려받기 버튼(`onclick="downloadImage()"`), 인쇄 버튼(`onclick="window.print()"`), html-to-image CDN 스크립트(`<script src="https://cdn.jsdelivr.net/npm/html-to-image@1.11.11/dist/html-to-image.js"></script>`)를 모두 갖춘 `.viz-menu` 요소. **EVALUATION CRITICAL:** 메뉴 시스템은 자동으로 검사되며 없으면 WILL CAUSE FAILURES. 전체 구현(HTML·CSS·JS, 인쇄 스타일, 슬라이드 덱 예외)은 [references/menu.md](references/menu.md)를 보라.
3. **테마 클래스 (EVALUATION CRITICAL):** 스타일시트에 `.theme-light`와 `.theme-dark` 클래스를 BOTH 둘 다 명시적으로 정의하고, 사용자 정의 속성을 빠짐없이 넣어야 한다. **EXAMPLE REQUIRED:**
```css
:root { /* base properties */ }
.theme-light { --bg: #ffffff; --surface: #f8f9fa; --text: #1a1a1a; /* etc */ }
.theme-dark { --bg: #0a0a0a; --surface: #1a1a1a; --text: #ffffff; /* etc */ }
```
**`:root`만 쓰거나 `@media (prefers-color-scheme)`에 의존하지 말 것 (NEVER) — 평가 시스템은 클래스 기반 테마를 검사한다.**
4. **시맨틱 HTML:** `<main id="main-content">` 요소, **MANDATORY: 주요 내용 블록마다 `<section>` 요소를 여러 개** (헤더, 지표, 차트 등), skip-to-content 링크, 랜드마크 역할 (`role="banner"`, `role="main"`, `role="complementary"`). 구분되는 내용 영역은 각각 시맨틱 `<section>` 태그로 감싼다.
5. **Chart.js 요구사항 (EVALUATION CRITICAL):** `</head>` 앞에 UMD 빌드를 MUST 넣어야 한다:
```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
<script>Chart.defaults.animation = false;</script>
```
**파일명이 `chart.umd.min.js`여야 한다 (MANDATORY).** `dist/chart.min.js`는 ESM 빌드라 평범한 `<script>`에서 `Cannot use import statement outside a module`을 던지고 파일 전체가 죽는다. 같은 이유로 **파일 어디에도 `import`/`export` 문법을 쓰지 않는다 (NEVER)** — 최상위는 `var` 선언만 쓴다.

   차트를 만드는 코드는 아래 **Chart.js Integration Rules** 절에 하나만 있다. 그것을 그대로 쓴다.
6. **반응형 디자인:** 섹션 간격 48px 이상, **CRITICAL: 375px 뷰포트에서 가로 오버플로 NO** (MANDATORY: 가로 스크롤을 막기 위해 `@media (max-width: 375px) { body { overflow-x: hidden; } }`를 넣는다), **MANDATORY FONT-SIZE HIERARCHY:** h1 ≥ 2.5rem, h2 ≥ 2rem, h3 ≥ 1.5rem, body = 1rem. **SLIDE DECK REQUIREMENTS:** 제목 슬라이드 h1 ≥ 3rem, 본문 슬라이드 제목 ≥ 2.5rem, 제목 단계 간 시각적 구분이 분명할 것. **SLIDE SECTION SPACING:** 슬라이드 안 주요 구역은 48px 이상 간격을 둔다 (제목-본문, 본문-차트, 차트-내비게이션). **모든 레이아웃을 375px 너비에서 확인한다 — 특히 대시보드는 차트 컨테이너가 넘치기 쉽다.** **CSS CONTAINER QUERIES:** 더 정밀한 반응형에는 컨테이너 기반 쿼리를 쓴다:
```css
.chart-card { container-type: inline-size; }
@container (max-width: 400px) { 
  .chart-legend { display: none; } 
  .chart-title { font-size: 1rem; }
}
```
뷰포트 미디어 쿼리를 넘어 컴포넌트 단위의 진짜 반응형을 얻을 수 있다.
7. **인쇄와 접근성:** `@media print` 스타일, 애니메이션을 끈 `@media (prefers-reduced-motion: reduce)`
8. **진입 애니메이션 (MANDATORY):** `.animate` 클래스, `data-reveal` 속성, 또는 CSS `@keyframes`로 진입 애니메이션을 반드시 넣는다. **EVALUATION CRITICAL:** 애니메이션 유무는 자동으로 검사되며 필수다.
9. **JavaScript 함수:** `cycleTheme()`, `toggleMenu()`, 최상위 변수는 `let`/`const`가 아니라 `var`를 쓴다

**🔥 CRITICAL: skeleton.md를 그대로 복사 → "YOUR CONTENT HERE"를 시각화 내용으로 교체 → 파일 저장**

## Core Principles — 핵심 원칙

1. **단일 파일 HTML** — CSS/JS가 인라인으로 들어간 `.html` 파일 하나. 아무 브라우저에서나 열리고, 오프라인에서 동작하고, 메일로 보내기 쉽다.
2. **라이트 테마 최적화** — 현대적 디자인은 라이트 모드의 품질을 우선한다. 다크 테마는 전환 버튼으로 제공한다.
3. **기본값이 이미 아름답다** — 첫 결과물이 손보지 않아도 전문적으로 보여야 한다.
4. **내용 우선** — 시각화는 메시지를 위해 존재한다. 미적인 이유로 명료함을 희생하지 않는다.
5. **반응형** — 명시적으로 크기가 고정된 경우(예: 16:9 슬라이드)를 빼면 데스크톱·태블릿·모바일에서 모두 동작한다.
6. **시각적 절제** — 전문적인 디자인은 소음이 되는 장식 요소를 피한다. 떠다니는 그라디언트 구, 무지개 테두리, 장식용 애니메이션은 쓰지 않는다.

## Philosophy — 철학

HTML은 "웹사이트"가 아니라 시각화 도구다. 코드는 값싸다. 누구든 무엇이든 시각화할 수 있어야 한다. 이 스킬은 대화 맥락, URL, 기사, 데이터, 다듬어지지 않은 아이디어를 몇 초 만에 눈으로 보고 이해할 수 있는 것으로 바꾼다.

사용자는 Claude Code와 **대화 도중에** 이 스킬을 부른다. 지금까지 나눈 이야기, 공유된 링크, 붙여 넣은 데이터 등 대화 맥락 전체를 재료로 쓴다. URL이 주어지면 크롤링해서 내용을 뽑아 시각화한다.

## Output Rules — 출력 규칙

**MANDATORY FIRST STEP: [references/skeleton.md](references/skeleton.md)에서 스켈레톤 전체를 복사한다 — 여기에 필요한 요소가 전부 들어 있다 (메뉴, 테마 시스템, CSS 속성, 시맨틱 HTML, 접근성 기능). HTML을 맨바닥부터 쓰지 않는다 (Never).**

- `.html` 파일 하나를 `~/Downloads/`에 쓴다 (또는 사용자가 지정한 경로)
- 파일명: 내용을 알 수 있는 kebab-case, 예: `q4-revenue-dashboard.html`, `team-roadmap-deck.html`
- skeleton.md 템플릿에서 시작해 `<!-- YOUR CONTENT HERE -->` 구역에 내용을 넣는다
- 맞춤 스타일은 전부 스켈레톤 기본 스타일 뒤의 `<style>`에 넣는다
- **CDN 라이브러리를 적극 쓴다** — 목적에 가장 맞는 도구를 고른다:
  - **Tailwind CSS** — `https://cdn.tailwindcss.com` (유틸리티 우선 스타일링, 자유롭게 쓴다)
  - **Chart.js** — `https://cdn.jsdelivr.net/npm/chart.js` (막대, 선, 원, 레이더, 도넛)
  - **D3.js** — `https://cdn.jsdelivr.net/npm/d3@7` (복잡하거나 맞춤형인 데이터 시각화, 포스 그래프)
  - **Mermaid** — `https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js` (순서도, 시퀀스 다이어그램)
  - **Three.js** — 필요할 때 3D에 쓴다
  - **Reveal.js** — 본격적인 슬라이드 엔진이 필요할 때. **CRITICAL:** `html, body { height: 100%; overflow: hidden; }`를 설정하고 `.reveal` 컨테이너에 `height: 100%`를 줘야 한다. 설정에는 숫자 크기를 MUST 써야 한다: `Reveal.initialize({ width: 1280, height: 720, center: true, controls: false })` — `'100%'` 같은 문자열 퍼센트는 NEVER 쓰지 않는다. 뷰포트 높이가 0이 되어 슬라이드가 빈 화면으로 나온다. **MANDATORY: Reveal.js 기본 컨트롤을 끈다** (`controls: false`) — 기본으로 겹쳐 나오는 `<` `>` 화살표는 보기 흉하다. 대신 아래쪽에 간결한 맞춤 내비게이션 바를 넣는다:
```html
<nav class="slide-nav" aria-label="Slide navigation">
  <button onclick="prevSlide()" aria-label="Previous slide">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M15 18l-6-6 6-6"/></svg>
  </button>
  <span class="slide-counter" id="slideCounter">1 / 8</span>
  <button onclick="nextSlide()" aria-label="Next slide">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18l6-6-6-6"/></svg>
  </button>
</nav>
```
```css
.slide-nav { position: fixed; bottom: 16px; left: 50%; transform: translateX(-50%); display: flex; align-items: center; gap: 8px; z-index: 9998; }
.slide-nav button { width: 28px; height: 28px; border-radius: 6px; background: transparent; border: none; color: var(--text-secondary); cursor: pointer; display: flex; align-items: center; justify-content: center; opacity: 0.3; transition: opacity 0.2s; }
.slide-nav button:hover { opacity: 0.7; }
.slide-counter { font-size: 12px; color: var(--text-secondary); font-weight: 400; min-width: 40px; text-align: center; opacity: 0.35; }
```
  - **Leaflet** — 지도와 지리 데이터 (`https://unpkg.com/leaflet@1.9/dist/leaflet.js` + CSS). **지리 데이터에는 필수** — SVG로 대륙 모양을 직접 그리지 않는다. OpenStreetMap 타일이나 간결한 타일 제공자를 쓴다.
- 아이콘과 단순한 그래픽은 SVG로 — 사용자가 제공하지 않는 한 외부 이미지 URL은 쓰지 않는다
- 가능하면 JS보다 CSS 애니메이션을 쓴다

자세한 CDN 링크, 패턴, 요령은 [references/libraries.md](references/libraries.md)를 보라.

## Design System — 디자인 시스템

여기 적힌 기본값을 적용한다. 의도적으로 정해졌고 검증된 값이므로, 사용자가 요청할 때만 덮어쓴다.

**전체 디자인 시스템 참조:** 타이포그래피, 색, 간격, 애니메이션, 접근성, 시각적 완성도의 전체 규격은 [references/design-system.md](references/design-system.md)를 보라.

주요 항목만 아래에 추린다 (자세한 내용은 참조 문서를 볼 것):

### 설계 메모 (Design Notes)

**테마 시스템 (CRITICAL):**
- **클래스 기반 테마만 (ONLY)** 쓴다 — `<html class="theme-dark">` 또는 `<html class="theme-light">`
- 테마 전환은 html 클래스를 바꾼다: `document.documentElement.className = 'theme-' + newTheme`
- **`data-theme` 속성을 쓰지 않는다 (Never)** — 평가 시스템은 클래스 기반 테마를 기대한다
- **필수 CSS 사용자 정의 속성:** `--bg, --surface, --text, --accent, --border` (평가와 맞물리기 위한 최소 집합)

**타이포그래피:**
- **Inter 글꼴 필수** — `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap`
- **MANDATORY 글꼴 굵기 위계:** h1 ≥ 700, h2 ≥ 600, h3 ≥ 500, body = 400 (평가의 핵심 요구사항)
- 제목 자간 -0.03em
- **KOREAN TYPOGRAPHY EXCELLENCE:** 한글 내용에는 본문에 Noto Sans KR을, UI 요소에는 Inter를 쓴다. 한글에는 `line-height: 1.6`을 적용한다 (라틴 문자는 1.4). 한글 Medium 굵기는 서구권 Regular(400)에 대응한다. 다음을 포함한다: `https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;600;700&display=swap`

**색:**
- 클래스 기반 테마만 (NO @media prefers-color-scheme)
- 다크: 배경 #0A0A0A, 글자 #EDEDED. 라이트: 배경 #FAFAF9, 글자 #0f172a
- 전체 팔레트는 참조 문서를 볼 것.
- **카드:** 모서리 8px, 그림자만 바뀌는 호버 (translateY/scale 없이), `1px solid var(--border)`. **GLASS MORPHISM UPGRADE:** 고급스러운 레이아웃에는 `backdrop-filter: blur(8px)`를 쓴 유리 컨테이너, CSS 변수 `--glass-opacity: 0.08`을 쓴 반투명 배경, 띄운 그림자를 쓴다. 대표 영역이나 주요 카드에만 선별적으로 적용해 정교한 층을 만든다.
- **애니메이션:** 페이지 로드에는 CSS @keyframes (.animate + .delay-N), 스크롤에는 data-reveal + IntersectionObserver, 카운터에는 data-count. 내용은 기본적으로 보인다. **첫 화면에 보이는 내용에는 data-reveal을 NEVER 쓰지 않는다** — 대신 `.animate` 클래스를 쓴다. `data-reveal`은 첫 화면 아래 내용에만 아껴서 쓴다 (최대 3~4개 섹션).
- **접근성:** Skip-to-content, aria-label, 랜드마크 역할, :focus-visible, 차트 데이터용 sr-only. 전체 점검 목록은 참조 문서를 볼 것.
- **아이콘:** 인라인 SVG만, 이모지는 절대 쓰지 않는다. Lucide 스타일 24x24, 선 기반.
- **Chart.js:** 정식 패턴 하나만 쓴다 — 아래 **Chart.js Integration Rules** 절을 볼 것. 차트 카드는 모서리 반지름 12px, 내부 여백 24px, 차트 영역 높이 360px로 존재감을 준다. 라이브러리 기본값 그대로 두지 않는다.
- **타이포그래피 위계:** MANDATORY 글꼴 크기가 내림차순이어야 한다: h1 > h2 > h3 > 본문. **REQUIRED MINIMUMS:** h1: 3rem 이상 (48px), h2: 2rem 이상 (32px), h3: 1.5rem 이상 (24px), body: 1rem (16px). **EVALUATION CRITICAL:** 각 제목 단계는 앞 단계보다 눈에 띄게 작아야 하며 단계 간 차이가 최소 0.5rem이어야 한다. 유효한 위계 예시: h1: 3rem, h2: 2.5rem, h3: 1.5rem, body: 1rem.
- **시각적 절제:** 떠다니는 구, 그라디언트 테두리, 제목의 그라디언트 텍스트, 확대 변형, 발광 효과, 장식용 애니메이션을 쓰지 않는다.
- **통계 수치의 색:** 색을 입힌 숫자는 의미를 담아야 한다 (초록/positive = 좋은 지표, 빨강/negative = 나쁜 지표, accent = 주요·중립 강조). 분명한 의미가 없으면 `var(--text)`를 쓴다. 통계 수치에 임의로 색을 입히지 않는다 (Never). **카드 4개 이상인 KPI 그리드:** 수치에 강조색을 최대 2개만 쓴다 — 가장 중요한 지표 하나에 `var(--accent)`, 나머지는 전부 `var(--text)`. `var(--positive)`/`var(--negative)`는 변화량 표시(화살표, 퍼센트)에만 쓰고 카드의 주요 수치에는 쓰지 않는다.
- **배경 분위기:** 파일당 은은한 기법 하나만 (방사형 그라디언트, 노이즈 질감, 점 격자 중 하나). **분위기를 내용에 맞춘다** — 게임 대시보드는 재무 보고서와 다른 느낌이어야 한다. 강조색과 그라디언트 색조를 주제에 맞게 조정한다.
- **AI-NATIVE INFORMATION ARCHITECTURE:** 현대적 디자인은 통찰 중심의 위계를 우선한다. 가장 중요한 지표·통찰을 첫 화면에 둔다. 단계적 공개 패턴을 쓴다 — 핵심 데이터는 바로 보여 주고, 호버·클릭으로 상세를 제공한다. 맥락에 맞는 동작은 관련 내용 가까이에 둔다. 결론을 먼저 말하고 세부로 뒷받침한다.
- **진입 애니메이션 필수:** 모든 카드·섹션에 fadeInUp + 시차.
- **단일 화면 포스터:** 크기가 고정된 body에 overflow:hidden + justify-content:space-between. 9:16, 1:1, 4:5 크기는 참조 문서를 볼 것.


## Critical Implementation Requirements — 구현 필수 요구사항

**MANDATORY: [references/skeleton.md](references/skeleton.md)의 스켈레톤 템플릿에서 시작한다.**
아래 요구사항이 전부 내장돼 있다. 각 항목은 평가 검사기가 자동으로 확인한다.

- **최상위 변수는 전부 `var`를 MUST 써야 한다** (`let`/`const`가 아니다) — 함수 호이스팅으로 인한 TDZ 오류를 피한다
- **`cycleTheme()`** — html 클래스를 바꿔 테마를 전환한다. 스켈레톤에 올바른 `applyTheme()` 구현이 함께 있다
- **`toggleMenu()`** — 바깥 클릭과 Escape 키로 드롭다운이 닫혀야 한다. 스켈레톤에 처리가 들어 있다
- **`function onThemeChange() {}`를 정의한다** — 테마 변경 시 차트를 다시 그린다
- **375px 뷰포트에서 가로 오버플로가 없어야 한다** — 카드 그리드에는 CSS Grid `minmax(320px, 1fr)`를 쓴다
- **기본 메뉴 외에 인터랙티브 요소가 있어야 한다** — 호버 상태, 차트 인터랙션 등

## Visualization Types — 시각화 형식

알맞은 형식을 고른다. 자세한 패턴은 [references/types.md](references/types.md)를 보라.

| Type | 언제 쓰는가 | 핵심 특징 |
|------|-------------|-------------|
| **Slide Deck** | 발표, 피치 | 16:9, 키보드 내비게이션, 전환 효과 |
| **Infographic** | 데이터 요약, 시각적 이야기 | 긴 스크롤, 큰 숫자, 구역 나누기 |
| **Dashboard** | 지표, KPI | 카드 격자 + 차트 |
| **Flowchart** | 프로세스, 아키텍처 | Mermaid 또는 SVG 다이어그램 |
| **Timeline** | 시간순 사건 | 좌우 교차 배치, 스크롤 연동 |
| **Comparison** | 나란히 놓고 분석 | 기능 매트릭스, 장단점 |
| **Data Viz** | 차트, 데이터 이야기 | Chart.js 또는 D3 |
| **One-Pager** | 요약, 브리프 | 화면 한 장, 인쇄 친화적 |
| **Mind Map** | 개념 관계 | 방사형 SVG 배치 |
| **Kanban** | 상태 추적 | 열 기반 카드 |
| **Carousel Cards** | 소셜미디어 (IG/LinkedIn) | 카드당 1080×1080, 스와이프, 굵은 글씨 |
| **Event Poster** | 컨퍼런스, 모임, 웨비나 | 세로 A4/레터, 굵은 제목, 날짜·장소 |
| **Resume/CV** | 입사 지원 | 한 장, 2단, 인쇄 최적화 |
| **Banner/Header** | 이메일, 블로그, 소셜 커버 | 1200×630 또는 1500×500, 시각적 배경 위 가운데 글자 |
| **Quote Card** | 사회적 증거, 후기 | 세로·정사각형, 큰 인용문, 출처 |
| **Process Guide** | 방법 안내, 단계별 | 번호가 붙은 단계, 아이콘, 명확한 흐름 |
| **Status Report** | 경영진 보고 | KPI + 진행 바 + 주요 사항, 한 장 |
| **Org Chart** | 조직 구조 | 계층 트리, 사진·아바타, 직책 |
| **Data Story** | 서사 + 데이터 | 스크롤리텔링, 서술 사이에 차트를 엮음 |
| **Product Card** | 기능 소개, 출시 | 대표 이미지 영역, 기능 칩, 행동 유도 |

### Carousel Card Rules — 캐러셀 카드 규칙

캐러셀 카드는 소셜미디어에서 비중이 크다. 아래를 지킨다:

- **정사각형 형식** — `1080×1080px` (또는 CSS 변수로 조정 가능하게)
- **카드 하나에 아이디어 하나** — 굵은 제목 + 뒷받침 항목 최대 1~2개
- **스와이프 내비게이션** — 화살표 + 점 + 터치 스와이프 + 키보드
- **카드 카운터** — "3 / 8"이 보이게
- **전체 내려받기** — 개별 카드 또는 전체 세트를 PNG로 내보내기
- **타이포그래피가 주인공** — 제목 2.5~4rem, 본문은 최소로
- **시맨틱 구조 MANDATORY** — 카드는 `<section>`, 첫 카드 제목은 `<h1>`, 나머지 카드 제목은 `<h2>`로 만든다. 제목을 `<div>`로 만들지 않는다 (NEVER) — 글꼴 크기·굵기 위계 검사가 통과할 수 없다.
- **색 구분** — 카드마다 강조색을 은은하게 달리할 수 있다
- **인쇄 레이아웃** — 인쇄용으로 전체 카드를 격자에 배치
- **최대 10장** — 초점을 잃지 않게

### Event Poster Rules — 이벤트 포스터 규칙

- **세로 방향** — A4/레터 비율 또는 정사각형
- **시각적 위계** — 행사명(가장 크게) → 날짜·시각 → 장소 → 설명 → 행동 유도
- **굵은 제목** — 3~5rem, 최대 6단어
- **날짜·시각을 눈에 띄게** — 배지나 강조 블록으로 처리
- **QR 코드 자리** — 등록 링크용 자리표시자 상자
- **인쇄 우선** — 인쇄했을 때 보기 좋아야 하고, 다크·라이트 모두

### Quote Card Rules — 인용 카드 규칙

- **큰 따옴표** — 강조색으로 크게 넣은 장식용 " "
- **인용문** — 1.5~2.5rem, 대비를 위해 세리프나 이탤릭
- **출처 표기** — 인용문 아래에 이름, 직함, 회사
- **정사각형 또는 세로형** — 소셜 공유에 맞춰 최적화
- **최소한의 디자인** — 인용문이 주인공이고 나머지는 은은하게

### Single-Screen / Mobile-Fit Rules — 단일 화면·모바일 맞춤 규칙 (포스터, 카드, 한 장 요약)

사용자가 "한 화면", "휴대폰 화면", "9:16", "모바일에 맞게"를 요청하면 스크롤하는 페이지가 아니라 (NOT) **크기가 고정된 단일 뷰포트** 시각화를 만든다.

**크기:**
- **9:16 세로 (휴대폰):** `width: 1080px; height: 1920px;` — 표준 인스타그램 스토리 / 휴대폰 화면
- **1:1 정사각형:** `width: 1080px; height: 1080px;` — 인스타그램 게시물
- **4:5 세로:** `width: 1080px; height: 1350px;` — 인스타그램 세로 게시물
- **16:9 가로:** `width: 1920px; height: 1080px;` — 발표 슬라이드

**MANDATORY — 래퍼가 flex 체인을 끊지 않게 한다:** 스켈레톤의 `<main>` 래퍼를 쓰면 `main { flex: 1; display: flex; flex-direction: column; min-height: 0; }`을 반드시 넣는다. 이 규칙을 빠뜨리면 (NEVER) `<main>`이 콘텐츠 높이에서 멈춰 캔버스 하단이 비어 버린다. `body`에만 flex를 걸어서는 부족하다.

**핵심 CSS 패턴:**
```css
body {
  width: 1080px; height: 1920px; /* or chosen ratio */
  overflow: hidden; /* MUST — prevents scroll, enforces single screen */
  display: flex; flex-direction: column; /* Flex column fills canvas completely */
}
/* MANDATORY when a <main> wrapper sits between body and the sections: the wrapper
   MUST forward the flex chain, or it stops at its natural height and the bottom of
   the poster stays empty. */
main { flex: 1; display: flex; flex-direction: column; min-height: 0; }
.poster-header { padding: 44px 48px 0; }
.poster-grid { flex: 1; padding: 24px 48px 0; } /* flex:1 expands to fill remaining space */
.poster-footer { padding: 16px 48px 36px; }
```

**레이아웃 규칙:**
- body에 `overflow: hidden` — 이것이 "한 화면"을 만드는 핵심이다. 협상 불가.
- 주 컨테이너에 `justify-content: space-between` — 빈 틈 NO 없이 섹션을 고르게 분배한다.
- **주 내용 영역에 `flex: 1`을 쓴다** (그리드, 본문 등). 그래야 헤더와 푸터 사이의 남은 공간을 ALL 전부 채운다. 빈 공간을 남기는 고정 `height` 값은 쓰지 않는다 (Never).
- 논리적 구역마다 `<div>`로 감싸서 flexbox가 블록 단위로 분배하게 한다.
- **빈 공간 0 규칙:** 포스터 캔버스를 100% 활용해야 한다. 아래나 옆에 큰 빈 여백이 없어야 한다. 눈에 띄는 빈 공간이 있으면 내용을 늘려 채우거나 여백을 줄인다. 내용이 틀에 꼭 맞는 느낌이어야 한다.
- **머릿속으로 확인:** 섹션 수를 세고 1920px를 나눠 본다. 섹션당 약 200~300px이 된다. 내용이 빈약하면 요소를 키운다 (글꼴 크게, 여백 늘리기, 아이콘 크게).
- 크기가 고정된 포스터에는 **햄버거 메뉴를 넣지 않는다** — 공간을 낭비하고, 포스터는 인터랙션이 아니라 스크린샷·내보내기용이다.

**9:16의 내용 밀도:**
- 대표 영역 (제목 + 부제): 높이의 약 25%
- 본문 섹션 2~3개: 높이의 약 55%
- 푸터·행동 유도: 높이의 약 10%
- 숨 쉴 공간 (간격): 높이의 약 10%
- **비어 보인다면 내용이 너무 작은 것이다.** 글꼴을 키우고, 격자 항목을 늘리고, 아이콘을 크게 한다.

**너비 1080px 포스터의 글꼴 크기:**
- 대표 h1: `68-80px` (웹보다 크게 — 이건 포스터다)
- 섹션 라벨: `15-18px` 대문자, letter-spacing `0.06em`
- 카드 글자: `16-20px`
- 본문: `20-24px`

**흔한 실수:** 스크롤하는 페이지를 만들고 스크린샷을 찍는 것. 그건 포스터가 NOT 아니라 웹페이지 스크린샷이다. 포스터는 픽셀 하나하나가 의도된 고정 캔버스다.

## Slide Deck Rules — 슬라이드 덱 규칙

슬라이드는 가장 흔한 요청이다. 아래를 지킨다:

- **16:9 화면비** — `100vw × 100vh`, 내용은 가운데 정렬
- **반응형 분기점** — 모바일에서도 쓸 수 있게 `clamp()`와 컨테이너 쿼리를 쓴다:
  ```css
  .slide-container { container-type: inline-size; }
  .slide-title { font-size: clamp(2rem, 8vw, 4rem); }
  @container (width < 768px) { .slide-content { padding: 1rem; } }
  ```
- **슬라이드 하나에 아이디어 하나** — 생각이 하나 더 필요하면 슬라이드를 하나 더 만든다
- **슬라이드당 최대 40단어** — 그보다 많으면 나누거나 시각물을 쓴다
- **제목은 최대 6단어** — 짧고 강하고 기억에 남게
- 통계 슬라이드는 **큰 숫자 + 작은 라벨** — 숫자 3~5rem, 라벨 0.875rem
- **키보드 내비게이션** — ← → 방향키, Space, Enter
- **터치 내비게이션** — 좌우 스와이프
- **클릭 내비게이션** — 왼쪽 1/3 = 이전, 오른쪽 2/3 = 다음
- **진행 바** — 위쪽에 위치를 보여 주는 가는 그라디언트 바
- **슬라이드 카운터** — 아래 내비게이션에 "3 / 12"
- **모바일 내비게이션의 존재감** — 모바일에서 내비게이션 컨트롤이 분명히 보이게 한다. 터치 대상을 크게 잡고(최소 44px), 대비되는 색을 쓰고, 떠 있는 내비게이션에는 backdrop-blur를 쓴다
- **부드러운 전환** — `transform: translateX()`에 500ms cubic-bezier
- **진입 애니메이션** — 슬라이드 안 요소들이 시차를 두고 등장한다
- **발표자 노트** — `data-notes` 속성, 인쇄할 때만 보인다

- **테마별 그라디언트** — 제목·구분 슬라이드는 다크(깊고 진하게)와 라이트(부드러운 파스텔) 짝을 따로 정의한다. 본문 슬라이드 배경은 `var(--bg)`/`var(--surface)`를 쓰고 어두운 색을 하드코딩하지 않는다 (Never).
- **차트 슬라이드** — 차트 영역 높이 **400px 이상** (대시보드 360px보다 크게). 구조와 JS는 아래 **Chart.js Integration Rules**와 같다.
- **투자 발표·경영진 브리핑** — 첫 슬라이드 제목 4~6rem, 5초 안에 가치 제안이 전달되게 한다.

슬라이드 유형 9가지, 그라디언트 CSS, 내비게이션 패턴은 [references/types.md](references/types.md) §Slide Deck을 보라.


## Data Ingestion — 데이터 받아들이기

사용자가 데이터를 줄 때:
- **CSV** — JS로 파싱하고, 헤더를 자동 감지하고, 알맞은 차트 유형으로 그린다
- **JSON** — 키를 라벨로, 값을 데이터로, 중첩 객체를 계열로 뽑는다
- **표** — 시각적 비교나 차트로 바꾼다
- **글 속의 숫자** — 뽑아내서 통계 카드로 강조한다
- **URL** — 크롤링하고, 핵심 정보를 뽑아, 요약으로 시각화한다

## Context Awareness — 맥락 활용

이 스킬은 대화 도중에 쓰인다. 가진 것을 전부 활용한다:

- **대화 맥락** — 지금까지 나눈 이야기를 요약하거나 구조화하거나 시각화한다
- **URL·링크** — 크롤링해서 내용을 뽑은 뒤 시각화한다
- **붙여 넣은 데이터** — CSV, JSON, 표 → 차트, 대시보드
- **아이디어·개념** — 추상적인 논의를 시각적 도식으로 바꾼다
- **코드·아키텍처** — 시스템 설계와 데이터 흐름을 시각화한다

항상 실제 내용을 쓴다. 실제 맥락이 있는데 자리표시자 데이터를 만들지 않는다 (Never).

## Type-Specific Interactivity — 형식별 인터랙션 (Mandatory)

모든 파일에는 테마 전환과 메뉴 외에 의미 있는 인터랙션이 최소 ONE 하나는 있어야 한다 (MUST). 정적인 느낌의 페이지는 인터랙션 점수가 낮다.

| Type | 필요한 인터랙션 |
|------|---------------------|
| **Cheatsheet** | 검색·필터 입력 + 코드 블록 클립보드 복사. 접이식 묶음에는 `<details name="...">`를 쓴다. |
| **Dashboard** | 필터 도구 모음 또는 지표 상세 보기. 최소한 기간이나 범주 필터. |
| **Status Report** | 접이식 상세 구역 (`<details>` 사용). 진행 바가 스크롤 시 애니메이션. |
| **Quote Card** | 인용문 자동 순환 또는 스와이프 캐러셀. 공유·복사 버튼. |
| **Event Poster** | 애니메이션 카운트다운 타이머 (일/시/분/초). 참석 신청·등록 버튼. |
| **Process Guide** | 단계를 배타적 아코디언으로 (`<details name="steps">`). 또는 인터랙티브 진행 추적기. |
| **Architecture** | 클릭하면 팝오버로 상세가 뜨는 노드 (Popover API 사용). 호버 시 연결선 강조. |
| **Timeline** | 시기·범주로 필터. 또는 클릭하면 사건 상세가 펼쳐짐. |
| **Comparison** | 범주 켜고 끄기. 또는 행마다 우세한 쪽 강조. |
| **Carousel** | 터치 스와이프 + 키보드 + 자동 넘김 옵션. 카드 카운터는 항상 보이게. |
| **Slide Deck** | 이미 인터랙티브함(내비게이션). 추가할 것: 발표자 타이머, 슬라이드 개요 격자. |

표에 없는 형식이라면 최소한 필터, 검색, 정렬, 펼치기·접기 중 하나는 넣는다.

## Layout Variation — 레이아웃 변주 (CRITICAL)

모든 파일이 글자만 바꾼 템플릿이 아니라 UNIQUE 고유한 디자인처럼 느껴져야 한다. 파일 형식에 따라 아래를 달리한다:
- **그리드 구조**: 1열, 2열, 3열을 섞는다. 강조할 카드에는 CSS Grid `span 2`를 쓴다. **CRITICAL: 항상 768px와 375px에서 확인한다 - 가로 오버플로는 허용되지 않는다.**

**모바일 우선 반응형 패턴 (MANDATORY):**
```css
.grid { 
  display: grid; 
  gap: 24px; 
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); 
}
@media (max-width: 768px) { 
  .grid { grid-template-columns: 1fr; gap: 16px; }
  .container { padding: 24px 16px; }
}
@media (max-width: 375px) {
  .card { padding: 16px; }  
  .stat-value { font-size: 2rem; }
}
```
- **섹션 리듬**: 전체 너비 섹션, 카드 그리드, 하나에 집중한 섹션을 번갈아 배치한다.
- **내용 밀도**: 큰 크기로 듬성듬성한 것보다 작은 크기로 내용이 많은 편이 더 전문적으로 보인다. KPI 카드 8개 + 차트 4개인 대시보드는 진짜처럼 느껴지고, KPI 카드 4개 + 차트 2개는 데모처럼 느껴진다.
- **시각적 초점**: 모든 파일에는 시각적으로 지배적인 요소가 ONE 하나 필요하다 (대표 통계, 핵심 차트, 주요 메시지) — 전부가 같은 무게여서는 안 된다.
- **격자에 홀로 남는 항목 금지**: 항목 수가 홀수라 마지막 줄이 차지 않으면, 마지막 항목에 `grid-column: span 2`를 주거나 `grid-template-columns`를 조정해 카드 하나가 한 줄에 덩그러니 남지 않게 한다.

## Anti-Patterns — 안티패턴

- ❌ 빽빽한 글 — 문서처럼 읽힌다면 그건 시각화가 아니다
- ❌ 너무 작은 글꼴 — 본문 최소 14px, 발표 제목은 20px 이상
- ❌ 무지개 색 — 팔레트에서 2~3색 + 중립색만 쓴다
- ❌ 자리표시자 내용 — "Lorem ipsum"이나 가짜 데이터를 쓰지 않는다 (never)
- ❌ 과한 설계 — 멋져 보이는 가장 단순한 방법을 쓴다
- ❌ 답답한 레이아웃 — 망설여지면 여백을 더 넣는다
- ❌ 특징 없는 디자인 — 시각화마다 템플릿이 아니라 의도된 것으로 느껴져야 한다
- ❌ 메뉴 누락 — 모든 결과물에 햄버거 메뉴가 필요하다
- ❌ 깨진 인쇄 — `@media print` 스타일을 항상 넣는다

## Advanced Techniques — 고급 기법

도움이 될 때만 쓴다. 코드 조각은 [references/css-techniques.md](references/css-techniques.md)를 보라.

- **Glass morphism** — 떠 있는 카드에 `backdrop-blur-md bg-white/5 border border-white/10`
- **Gradient text** — 대표 제목에 `background: linear-gradient(...); -webkit-background-clip: text`
- **Scroll-snap** — 슬라이드 내비게이션의 대안으로 `scroll-snap-type: y mandatory` (JS 불필요)
- **Conic gradients** — 순수 CSS 원·도넛 차트에 `conic-gradient()`
- **Number animations** — 스크롤 시 카운터를 0에서 목표값까지 애니메이션
- **Spring easing** — 경쾌한 마이크로 인터랙션에 `cubic-bezier(0.34, 1.56, 0.64, 1)`
- **Animate to auto** — `:root`에 `interpolate-size: allow-keywords`를 주면 `height: auto` 전환이 부드러워진다 (Chrome 129+)
- **CSS counters** — 단계별 절차의 자동 번호 매기기
- **View Transitions API** — 부드러운 테마 전환 애니메이션
- **Inline SVG icons** — 단순한 아이콘을 `<svg>` 경로로 직접 그린다. 아이콘 라이브러리 불필요

## Mandatory HTML Skeleton — 필수 HTML 스켈레톤

**EVERY visualization MUST start from the skeleton.** 복사한 다음 내용을 추가한다.

**스켈레톤 전체 코드:** 테마, 인쇄 스타일, Inter 글꼴, 애니메이션, 메뉴, 호버 효과가 들어간 복사·붙여넣기용 HTML 템플릿은 [references/skeleton.md](references/skeleton.md)를 보라.

스켈레톤이 제공하는 것:
- 클래스 기반 다크/라이트 테마 (첫 방문 시 OS 설정 감지, localStorage에 저장)
- CSS @keyframes 애니메이션 (fadeInUp, fadeIn, slideInLeft, slideInRight) + .animate/.delay-N 클래스
- data-reveal 속성 + IntersectionObserver를 이용한 스크롤 등장
- data-count 속성을 이용한 숫자 카운터
- 테마 전환, PNG 내려받기(html-to-image), 인쇄/PDF가 있는 햄버거 메뉴
- Popover와 details 아코디언 CSS (Chrome 114+/120+)
- @page 여백 상자가 있는 인쇄 스타일
- prefers-reduced-motion 지원

## Minimum Sizing Rules — 최소 크기 규칙

요소는 읽히고 존재감이 있을 만큼 커야 한다:

- **타임라인 카드:** 최소 너비 280px, 최소 여백 20px
- **타임라인 레이아웃:** 큰 빈틈이 생기지 않도록 항목을 고르게 분배한다. 항목이 5개인데 세로 공간의 60%만 찬다면, 남은 40%를 채울 내용 구역(투자 내역이나 성과 지표 같은 것)을 더 넣는다. 마지막 타임라인 항목 아래에 거대한 빈 공간을 남기지 않는다 (Never).
- **차트 컨테이너:** 부모 너비의 최소 60%, 최소 높이 300px (대시보드는 360px 이상). 그리드 레이아웃에서는 차트에 `flex-grow: 1`을 줘서 남는 공간을 채운다 — 300px는 목표가 아니라 하한이다.
- **통계 숫자:** 최소 글꼴 크기 2rem (32px), bold/extrabold 굵기
- **카드 내용 영역:** 최소 여백 24px
- **섹션 간격:** **MANDATORY 주요 섹션 사이 최소 48px** — 섹션 요소에 `margin-bottom: 48px` 이상을 준다
- **슬라이드 제목:** 최소 2rem (32px), 최대 6단어
- **본문:** 최소 1rem (16px), 그보다 작게는 절대 쓰지 않는다

**내용이 너무 작아 보인다면 실제로 작은 것이다. 망설여지면 더 크게 간다.**

## Text Visibility Rules — 글자 가시성 규칙

**글자는 ALWAYS 항상 보여야 한다.** 결과물이 깨지는 원인 1위다.

- 다크 테마: 글자는 `var(--text)`를 MUST 써야 하며 `#f9fafb`(거의 흰색)로 해석된다
- 라이트 테마: 글자는 `var(--text)`를 MUST 써야 하며 `#0f172a`(거의 검정)로 해석된다
- 그라디언트 배경 위: 가독성을 위해 `text-shadow: 0 1px 3px rgba(0,0,0,0.3)`을 넣는다
- 그라디언트·이미지 배경의 대표 슬라이드: 어두운 오버레이(`rgba(0,0,0,0.5)`)를 쓴다
- 배경색과 가까운 값으로 글자 색을 정하지 않는다 (NEVER)
- 머릿속으로 확인: "이 글자가 다크(#030712)와 라이트(#f8fafc) 배경 BOTH 양쪽에서 보이는가?"

## Chart.js Integration Rules — Chart.js 연동 규칙 (CRITICAL)

아래가 유일한 정식 패턴이다. 다른 방식을 쓰지 않는다 (Never). CDN과 UMD 빌드 요구사항은
위 Critical Requirements 5번을 볼 것.

### 컨테이너 구조 (REQUIRED)

```html
<div class="chart-card" role="img" aria-label="차트가 보여 주는 내용과 핵심 통찰">
  <h3>Revenue Growth</h3>
  <div class="sr-only">Jan $98K, Feb $102K, Mar $108K, ... Dec $142K.</div>
  <div class="chart-wrap"><canvas id="revenueChart"></canvas></div>
</div>
```
```css
.chart-card { padding: 24px; border-radius: 12px; background: var(--surface); border: 1px solid var(--border); }
.chart-wrap { position: relative; height: 360px; }   /* 대시보드 360px, 그 외 300px 이상 */
```

- **`role="img"`와 `aria-label`은 바깥 래퍼에 둔다** — canvas에 두면 차트를 다시 그릴 때 사라질 수 있다.
- **canvas의 직계 부모(`.chart-wrap`)에 `height`를 준다.** 평가 시스템은 이 요소의 실제 렌더 높이를 잰다. `min-height`가 아니라 `height`를 쓰고, 인라인 style로 300px 아래로 덮어쓰지 않는다 (Never).
- **`position: relative`가 없으면** `responsive: true` + `maintainAspectRatio: false` 조합에서 높이가 계속 늘어난다.
- `.sr-only` 블록에 차트 데이터를 글로 적는다 — 화면 낭독기 접근성.

### 만들고 다시 그리기 (MANDATORY PATTERN)

```javascript
var revenueChart;   // 최상위 변수는 전부 var (let/const는 TDZ 오류)

function getColors() {
  var s = getComputedStyle(document.documentElement);
  return {
    text: s.getPropertyValue('--text').trim(),
    sub: s.getPropertyValue('--text-secondary').trim(),
    border: s.getPropertyValue('--border').trim(),
    surface: s.getPropertyValue('--surface').trim(),
    accent: s.getPropertyValue('--accent').trim()
  };
}

// REQUIRED: 그릴 때마다 새로 찾는다. 최상위에서 캐시하면 참조가 상해서
// "can't acquire context from the given item" 오류가 난다.
function getCanvas(id) { return document.getElementById(id); }

function buildCharts() {
  if (typeof Chart === 'undefined') { console.error('Chart.js not loaded'); return; }
  var c = getColors();
  Chart.defaults.color = c.sub;
  Chart.defaults.animation = false;                  // MANDATORY

  try { if (revenueChart) revenueChart.destroy(); } catch (e) {}   // REQUIRED: 다시 그리기 전에 파기

  revenueChart = new Chart(getCanvas('revenueChart'), {
    type: 'line',
    data: { /* your data */ },
    options: {
      responsive: true,
      maintainAspectRatio: false,                    // MANDATORY
      plugins: {
        tooltip: { enabled: true, padding: 12, cornerRadius: 8,   // NEVER disable
                   titleFont: { size: 14 }, bodyFont: { size: 13 } },
        legend: { labels: { font: { size: 13 } } }
      },
      layout: { padding: 20 },
      scales: {
        x: { ticks: { color: c.sub, font: { size: 13 }, maxRotation: 0 }, grid: { color: c.border } },
        y: { ticks: { color: c.sub, font: { size: 13 } }, grid: { color: c.border } }
      }
    }
  });
}

// skeleton.md의 applyTheme()이 테마를 바꿀 때 이 함수를 부른다
function onThemeChange() { requestAnimationFrame(function () { setTimeout(buildCharts, 100); }); }
document.addEventListener('DOMContentLoaded', buildCharts);
```

격자선 불투명도, 글꼴 크기, 점 반지름 등 **보이는 것을 좌우하는 값**은
[references/design-system.md](references/design-system.md) §Chart.js 전문가용 스타일링에,
차트 유형별 요령은 [references/libraries.md](references/libraries.md) §Chart.js에 있다.


## Process — 작업 절차

1. **이해한다** — 메시지가 무엇인가? 대상이 누구인가? 어떤 형식이 맞는가?
2. **스켈레톤에서 시작한다** — 위의 Mandatory HTML Skeleton을 복사한다. 빈 파일에서 시작하지 않는다 (NEVER).
3. **구조를 잡는다** — 스켈레톤을 채우기 BEFORE 전에 내용과 구역의 개요를 짠다
4. **만든다** — 내용, 차트, 스타일을 넣는다. 색은 전부 CSS 변수로 유지한다.
5. **점검 목록으로 확인한다:**
   - [ ] `html.theme-dark`와 `html.theme-light` 클래스 기반 테마 선택자를 썼는가 (NO @media prefers-color-scheme)?
   - [ ] JS가 첫 방문 시 OS 설정을 감지하고 localStorage에 저장하는가?
   - [ ] 모든 글자가 `var(--text)` 또는 `var(--text-secondary)`를 쓰는가?
   - [ ] `@media print`가 메뉴를 숨기고 모든 내용을 보여 주는가?
   - [ ] `@media (prefers-reduced-motion: reduce)`가 있는가?
   - [ ] `.viz-menu`에 토글, 테마, 내려받기, 인쇄가 있는가?
   - [ ] 알맞은 글꼴을 불러왔는가? (기본은 Inter, 한글은 Noto Sans KR 등)
   - [ ] 라틴 문자가 아닌 내용에 알맞은 CJK/RTL 글꼴이 있는가?
   - [ ] `.animate` 클래스로 진입 애니메이션을 넣었는가 (CSS @keyframes)?
   - [ ] 스크롤 구역이 `data-reveal`을 쓰는가 (JS 없이도 보이는가)?
   - [ ] `.card:hover`에 transform 효과가 있는가?
   - [ ] 최상위 JS 변수가 전부 `var`인가 (`let`/`const`가 아닌가)?
   - [ ] 차트가 `var` 선언 + `onThemeChange` 훅을 쓰는가?
   - [ ] **MANDATORY:** 모든 차트를 `role="img" aria-label="..."`로 감쌌는가?
   - [ ] **MANDATORY:** 모든 차트에 호버 툴팁이 켜져 있는가 (끄지 않았는가)?
   - [ ] 통계가 있는 곳에 `data-count` 숫자 카운터 애니메이션을 넣었는가?
   - [ ] 시맨틱 HTML을 썼는가: `<main>`, `<section>`, `<header>`, `<article>`?
   - [ ] 모든 차트에 컨테이너 크기를 명시했는가 (높이 300px 이상)?
   - [ ] 대표·제목 글자가 두 테마 모두에서 보이는가?
   - [ ] 최소 크기 규칙을 지켰는가 (카드 280px 이상, 글자 16px 이상)?
   - [ ] 로드 시 콘솔 오류가 0건인가?

품질 기준: **"good, period"** (그냥 좋다) — "AI가 만든 것치고 좋다"가 아니다.
