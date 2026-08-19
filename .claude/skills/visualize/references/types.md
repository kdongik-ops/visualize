# Visualization Type Patterns — 시각화 형식별 패턴

시각화 형식마다의 상세 패턴. 지금 작업에 해당하는 절만 읽으면 된다.

형식 이름(## 제목)은 SKILL.md의 형식 표 및 Layer 1 형식 판정과 짝을 이루므로 원문 그대로 둔다.

## 목차
- [Slide Deck](#slide-deck)
- [Infographic](#infographic)
- [Dashboard](#dashboard)
- [Flowchart / Diagram](#flowchart--diagram)
- [Timeline](#timeline)
- [Comparison](#comparison)
- [Data Visualization](#data-visualization)
- [One-Pager](#one-pager)
- [Mind Map](#mind-map)
- [Kanban Board](#kanban-board)

---

## Slide Deck

### 구조 (Structure)
```html
<div class="deck">
  <section class="slide" data-notes="Speaker notes here">
    <!-- slide content -->
  </section>
</div>
```

### 내비게이션 패턴 (Navigation Pattern)
```javascript
// Keyboard: ← → arrows, Space, Enter
// Click: left third = prev, right two-thirds = next
// Touch: swipe left/right
// URL hash: #slide-3 for direct linking
```

### 슬라이드 유형 (Slide Types)
1. **Title** — 테마에 반응하는 그라디언트 배경, 큰 제목, 부제. 가운데 정렬. 강한 인상.
2. **Content** — 제목 + 항목, 또는 제목 + 시각물. 빽빽한 글과 시각물을 한꺼번에 넣지 않는다.
3. **Section divider** — 화면을 꽉 채운 강조색이나 그라디언트에 구역 제목만. 흐름을 끊어 준다.
4. **Stat** — 큰 숫자 하나, 라벨 하나, 통찰 문장 하나.
5. **Chart** — 제목과 핵심 요지가 있는 Chart.js 시각화.
6. **Image/Visual** — 화면을 꽉 채운 이미지나 큰 SVG 도식에 글은 최소로.
7. **Two-Column** — 비교, 글+이미지, 코드+설명을 위한 분할 레이아웃.
8. **Quote** — 출처가 붙은 큰 인용문. 우아한 타이포그래피.
9. **Closing** — 행동 유도, 연락처, 또는 요약 + 소셜 링크. 기억에 남게.

### 모범 사례 (Best Practices)
- 첫 슬라이드가 시선을 붙잡는다 — 단정적인 문장이나 질문
- 슬라이드 하나에 아이디어 하나
- 슬라이드 안에서는 단계적 등장을 쓴다 (CSS 애니메이션 지연)
- 위치를 일관되게: 제목은 항상 같은 자리, 본문은 항상 같은 영역
- 슬라이드 전환: `transform: translateX()` + `transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1)`


### 임팩트가 큰 발표 슬라이드 (비즈니스 맥락)

투자 발표, 스타트업 피치, 경영진 브리핑에서는:
- **첫 슬라이드의 시각적 무게** — 더 강한 그라디언트, 더 큰 타이포그래피(4~6rem), 설득력 있는 통계를 눈에 띄게 배치한다
- **가치 제안의 명료함** — 첫 슬라이드가 5초 안에 핵심 가치를 전달해야 한다
- **전문적인 신뢰감** — 타이포그래피, 간격, 색 선택이 기업·투자 수준의 기대치에 맞아야 한다
- **데이터로 이야기하기** — 차트 슬라이드마다 원시 데이터 나열이 아니라 분명한 통찰을 짚어 준다

### 테마에 반응하는 슬라이드 그라디언트 (CRITICAL)

슬라이드 덱은 다크와 라이트에서 시각적으로 확실히 달라 보여야 한다 (MUST).

```css
/* Dark theme: deep, saturated gradients */
.theme-dark .slide-title { background: linear-gradient(135deg, #1e1b4b 0%, #312e81 50%, #1e3a5f 100%); }
.theme-dark .slide-content { background: var(--bg); }

/* Light theme: soft, pastel gradients */
.theme-light .slide-title { background: linear-gradient(135deg, #e0e7ff 0%, #c7d2fe 50%, #dbeafe 100%); }
.theme-light .slide-content { background: var(--bg); }
```

- 제목·구분 슬라이드: 테마별 그라디언트 짝을 쓴다 (다크=깊고 진하게, 라이트=부드럽고 파스텔로). **내용의 주제를 연상시키는 색을 고른다** — 기술 피치는 차가운 파랑, 게임 피치는 선명한 보라·청록, 헬스케어 덱은 차분한 초록·틸.
- 본문 슬라이드: `var(--bg)` 또는 `var(--surface)`를 쓴다 — 하드코딩한 어두운 배경은 NOT 안 된다
- 슬라이드 위 데이터 카드: `var(--surface)`와 `var(--border)`를 쓴다 — 자동으로 맞춰진다
- 슬라이드 내용에 `#1a1a2e` 같은 어두운 색을 하드코딩하지 않는다 (Never) — CSS 변수를 쓴다
- 확인 방법: 테마를 전환했을 때 모든 슬라이드가 그 모드에 맞게 의도적으로 설계된 것처럼 보여야 한다

### 차트 슬라이드 (CRITICAL)

구조와 JS는 SKILL.md의 **Chart.js Integration Rules**와 같다. 슬라이드에서만 다른 점:

- **차트 영역 높이 400px 이상** — 발표에서 멀리서도 읽히도록 대시보드(360px)보다 크게 잡는다
- 슬라이드마다 차트는 하나, 제목과 핵심 요지를 함께 둔다

---

## Infographic

### 구조 (Structure)
- 길게 스크롤하는 한 페이지
- 섹션으로 나뉜 분명한 시각적 위계
- 아이콘(인라인 SVG)으로 글의 흐름을 끊어 준다
- 통계는 숫자를 크게 뽑아 강조한다
- 섹션마다 색을 구분한다

### 레이아웃 패턴 (Layout Pattern)
```
┌─────────────────────────┐
│      HERO / TITLE       │
├─────────────────────────┤
│   Key Stat  │  Key Stat │
├─────────────────────────┤
│     Section 1           │
│  ┌────┐ ┌────┐ ┌────┐  │
│  │Icon│ │Icon│ │Icon│  │
│  │Text│ │Text│ │Text│  │
│  └────┘ └────┘ └────┘  │
├─────────────────────────┤
│     Chart / Visual      │
├─────────────────────────┤
│     Section 2           │
│     Timeline/Flow       │
├─────────────────────────┤
│     CTA / Source        │
└─────────────────────────┘
```

### 모범 사례 (Best Practices)
- 최대 너비 800px, 가운데 정렬
- 스크롤 연동 애니메이션을 쓴다 (IntersectionObserver)
- 큰 숫자: 48px 이상, 굵게, 강조색
- 출처는 맨 아래에
- 공유 가능하게: 스크린샷으로 찍었을 때 보기 좋아야 한다

---

## Dashboard

### 구조 (Structure)
- 카드를 배치한 CSS Grid 레이아웃
- 제목 + 날짜·시각이 있는 헤더
- 위쪽에 KPI 카드 (핵심 지표 3~5개)
- 아래쪽 그리드에 차트·표
- 필요하면 필터용 사이드바

### KPI 카드 패턴 (KPI Card Pattern)
```html
<div class="kpi-card">
  <span class="kpi-label">Revenue</span>
  <span class="kpi-value">$1.2M</span>
  <span class="kpi-change positive">↑ 12.3%</span>
</div>
```

### 차트 패턴 (SVG)
- **막대 차트**: `<rect>` 요소에 height CSS 전환
- **선 차트**: `<polyline>` 또는 `<path>`에 stroke-dasharray 애니메이션
- **도넛 차트**: `<circle>`에 stroke-dasharray/stroke-dashoffset
- **스파크라인**: KPI 카드 안의 작은 `<polyline>`

### 모범 사례 (Best Practices)
- 반응형 카드에는 CSS Grid의 `auto-fit`과 `minmax()`를 쓴다
- 카드 그림자는 은은하게, 테두리는 없이
- 증가(초록)와 감소(빨강)를 색으로 구분한다
- 데이터 지점에 호버 툴팁을 단다
- 자동 갱신 표시 (정적이어도 살아 있는 느낌을 준다)

---

## Flowchart / Diagram

### 접근 방식 (Approach)
다이어그램은 SVG로 그린다. 노드는 CSS Grid나 SVG viewBox 안의 절대 위치로 배치한다.

### 노드 유형 (Node Types)
```svg
<!-- Rounded rectangle (process) -->
<rect rx="8" />
<!-- Diamond (decision) -->
<polygon points="50,0 100,50 50,100 0,50" />
<!-- Circle (start/end) -->
<circle />
<!-- Parallelogram (input/output) -->
<polygon points="20,0 100,0 80,50 0,50" />
```

### 연결선 (Connection Lines)
- 부드러운 연결에는 3차 베지어 곡선을 쓴 `<path>`를 쓴다
- 화살표 표시: `<polygon>` 화살촉을 담은 `<marker>` 요소
- 직각 레이아웃에는 꺾은선 연결자를 쓴다

### 모범 사례 (Best Practices)
- 흐름은 왼쪽에서 오른쪽, 또는 위에서 아래
- 노드 크기를 일정하게
- 라벨은 노드 안 가운데에
- 경로마다 색을 구분한다 (성공=초록, 오류=빨강)
- 단순하게 유지한다: 15~20개를 넘으면 하위 다이어그램으로 나눈다

---

## Timeline

### 레이아웃 선택지 (Layout Options)
1. **세로형** — 가운데 선을 두고 사건을 좌우로 번갈아 배치
2. **가로형** — 사건 수가 적을 때 쓰는 스크롤 타임라인
3. **압축형** — 점과 선만 있는 단일 열

### 세로 타임라인 패턴 (Vertical Timeline Pattern)
```
     ┌──────────┐
     │ Event 1  │──── ●
     └──────────┘     │
                      │
          ● ────┌──────────┐
                │ Event 2  │
                └──────────┘
                      │
     ┌──────────┐     │
     │ Event 3  │──── ●
     └──────────┘
```

### 모범 사례 (Best Practices)
- 시각적 균형을 위해 좌우를 번갈아 배치한다
- 날짜를 눈에 띄게 표시한다
- 사건 유형을 색·아이콘으로 구분한다
- 스크롤에 연동된 진입 애니메이션
- 로드맵에는 "현재" 표시를 넣는다

---

## Comparison

### 레이아웃 선택지 (Layout Options)
1. **나란한 카드** — 선택지 2~3개를 열로 배치
2. **기능 매트릭스** — 행 = 기능, 열 = 선택지, 체크 표시나 값
3. **전/후 비교** — 화면 분할

### 기능 매트릭스 패턴 (Feature Matrix Pattern)
- 헤더 행을 고정한다
- 행 배경색을 번갈아 준다
- 글자 대신 ✓ / ✗ 아이콘(SVG)을 쓴다
- 추천 선택지는 강조 테두리나 배지로 표시한다

### 모범 사례 (Best Practices)
- 비교 열은 최대 4개 (그 이상은 부담스럽다)
- 핵심 차이점을 강조한다
- 빠르게 훑을 수 있게 아이콘을 쓴다
- 추천 선택지로 시선이 가도록 색을 쓴다 (은은하게, 밀어붙이지 않게)

---

## Data Visualization

### 차트 유형 (전부 SVG 기반)
- **막대**: 세로 또는 가로, 그룹형 또는 누적형
- **선**: 단일 또는 다중 계열, 영역 채우기
- **원/도넛**: 최대 6조각, 퍼센트 라벨 표기
- **산점도**: 상관관계용, 크기로 세 번째 차원을 표현
- **히트맵**: 색이 입혀진 격자

### SVG 차트 필수 사항 (SVG Chart Essentials)
- 축과 축 라벨은 항상 넣는다
- 격자선은 은은하게 (`stroke: #eee`, `stroke-dasharray: 4`)
- 범례 위치를 일관되게 둔다 (오른쪽 위 또는 아래)
- 반응형 viewBox: `viewBox="0 0 600 400"` + `preserveAspectRatio`
- 로드 시 애니메이션: 선은 stroke-dasharray, 막대는 scaleY

### 모범 사례 (Best Practices)
- 데이터가 아니라 통찰을 앞세운다
- 핵심 데이터 지점은 차트 위에 직접 주석을 단다
- 3D 효과는 쓰지 않는다
- 막대 차트는 y축을 0에서 시작한다 (선 차트는 예외를 둘 수 있다)
- 차트 하나에 데이터 계열은 최대 5~7개

---

## One-Pager

### 구조 (Structure)
- 큰 제목과 보조 문구가 있는 대표 영역
- 본문 섹션 3~4개
- 분명한 행동 유도나 결론
- 화면 한 장: 스크롤 없이(또는 최소한의 스크롤로) 완결된 느낌

### 모범 사례 (Best Practices)
- 큰 대표 문구 (48px 이상)
- 기능은 아이콘 + 글 짝으로
- 가운데 정렬, 최대 너비 960px
- 전문적이되 지루하지 않게 — 과감한 디자인 선택 하나
- 스크린샷이나 PDF로도 통해야 한다

---

## Mind Map

### 접근 방식 (Approach)
- 중심 노드에서 가지가 뻗어 나가는 형태
- SVG의 `<path>`로 곡선 연결
- 갈래마다 범주별로 색을 구분한다
- 클릭하면 노드가 펼쳐진다 (선택적 인터랙션)

### 배치 알고리즘 (단순화)
- 중심 노드를 viewBox 가운데에 둔다
- 1단계 노드는 중심 주위에 원형으로 배치한다
- 2단계 노드는 부모에서 바깥쪽으로 뻗는다
- 위치 계산에는 극좌표를 쓴다

### 모범 사례 (Best Practices)
- 읽기 쉽도록 깊이는 최대 2~3단계
- 곡선의 자연스러운 연결 (베지어)
- 노드 크기로 중요도를 나타낸다
- 호버하면 해당 갈래를 강조하고 나머지를 흐리게 한다

---

## Kanban Board

### 구조 (Structure)
```html
<div class="board">
  <div class="column">
    <h3>To Do</h3>
    <div class="card">Task</div>
  </div>
  <div class="column">
    <h3>In Progress</h3>
    <div class="card">Task</div>
  </div>
  <div class="column">
    <h3>Done</h3>
    <div class="card">Task</div>
  </div>
</div>
```

### 모범 사례 (Best Practices)
- 열은 3~5개 (필요하면 가로 스크롤)
- 카드에는 제목, 필요하면 태그·라벨(색 구분 칩), 담당자 아바타
- 열 제목에 항목 수를 표시한다
- 은은한 드래그 손잡이 표시 (실제로 동작하지 않아도)
- 진행 중 작업 수 제한 표시
- 단계 구분을 위한 열 배경색 (아주 은은하게)
