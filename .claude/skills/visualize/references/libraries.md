# CDN Library Reference — CDN 라이브러리 참조

권장 CDN 라이브러리와 각각을 언제 쓰는지 정리한다. 로딩 속도와 일관성을 위해 항상 jsDelivr를 쓴다.

## 목차
- [Motion](#motion) ⭐ (애니메이션 — 스켈레톤에 포함)
- [Chart.js](#chartjs)
- [D3.js](#d3js)
- [Three.js](#threejs)
- [Mermaid](#mermaid)
- [Reveal.js](#revealjs)
- [Leaflet](#leaflet)

---

## Motion

**적합한 용도:** 모든 애니메이션. 스프링 물리, 스크롤 등장 효과, 시차를 둔 진입, 숫자 카운터, 호버 마이크로 인터랙션. 순수 CSS @keyframes와 IntersectionObserver를 대체한다.

```html
<script src="https://cdn.jsdelivr.net/npm/motion@12/dist/motion.js"></script>
```

**필수 스켈레톤에 포함되어 있다.** 전역 `Motion` 객체를 노출한다.

```javascript
// Spring-animated card entrance
Motion.animate('.card',
  { opacity: [0, 1], y: [40, 0], scale: [0.95, 1] },
  { delay: Motion.stagger(0.08), duration: 0.5, ease: Motion.spring({ stiffness: 200, damping: 22 }) }
);

// Scroll-triggered reveal
Motion.inView('.section', (info) => {
  Motion.animate(info.target, { opacity: 1, y: 0 }, { duration: 0.6 });
});
```

전체 API 참조와 사용 예시는 [animations.md](animations.md)를 보라 (gzip 기준 약 15KB).

---

## Chart.js

**적합한 용도:** 기본값이 이미 아름다운 표준 차트. 막대, 선, 원, 도넛, 레이더, 극좌표, 산점도, 버블.

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.7/dist/chart.umd.min.js"></script>
<script>Chart.defaults.animation = false;</script>
```

### 언제 쓰는가
- 설정을 최소로 하고 빠르게 데이터를 시각화할 때
- 표준 차트 유형(막대, 선, 원, 도넛, 레이더)
- 깊이 손대지 않고도 좋은 기본값을 원할 때
- 반응형·애니메이션 차트가 바로 필요할 때

### 패턴
```html
<canvas id="myChart"></canvas>
<script>
new Chart(document.getElementById('myChart'), {
  type: 'bar', // line, pie, doughnut, radar, polarArea, scatter, bubble
  data: {
    labels: ['Jan', 'Feb', 'Mar'],
    datasets: [{
      label: 'Revenue',
      data: [12, 19, 3],
      backgroundColor: 'hsla(220, 80%, 55%, 0.7)',
      borderColor: 'hsl(220, 80%, 55%)',
      borderWidth: 2,
      borderRadius: 6,
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,          // MANDATORY
    plugins: {
      tooltip: { enabled: true },        // NEVER disable
      legend: { position: 'bottom' },
      title: { display: true, text: 'Monthly Revenue' }
    },
    scales: { y: { beginAtZero: true } }
  }
});
</script>
```

### 요령
- 막대 모서리를 둥글게 하려면 `borderRadius`를 쓴다
- 선 데이터셋에 `tension: 0.4`를 주면 곡선이 부드러워진다
- 차트 유형 혼합: `{ type: 'bar', datasets: [{ type: 'line', ... }, { ... }] }`
- 범례 위치: 가로형 차트는 `'top'`, 공간이 있는 세로형은 `'right'`
- 라벨이 넘치면 `maxTicksLimit`으로 눈금 개수를 줄인다
- 툴팁 글꼴: `titleFont: { size: 14 }`, `bodyFont: { size: 13 }`
- `responsive: true`는 기본이지만 컨테이너에 크기가 명시돼 있어야 동작한다

만들고 다시 그리는 정식 코드는 SKILL.md의 **Chart.js Integration Rules**에 있다.

---

## D3.js

**적합한 용도:** 맞춤형이거나 복잡하거나 통상적이지 않은 데이터 시각화. 포스 디렉티드 그래프, 지리 지도, 트리맵, 선버스트.

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
```

### 언제 쓰는가
- Chart.js로 감당이 안 되는 맞춤형 시각화
- 포스 디렉티드 네트워크 그래프
- 지리·지도 시각화 (topojson과 함께)
- 트리맵, 선버스트, 코드 다이어그램
- SVG를 완전히 제어해야 할 때

### 패턴
```html
<div id="viz"></div>
<script>
const data = [30, 86, 168, 281, 303, 365];
const width = 600, height = 400, margin = { top: 20, right: 20, bottom: 30, left: 40 };

const svg = d3.select('#viz').append('svg')
  .attr('viewBox', `0 0 ${width} ${height}`);

const x = d3.scaleBand()
  .domain(data.map((_, i) => i))
  .range([margin.left, width - margin.right])
  .padding(0.2);

const y = d3.scaleLinear()
  .domain([0, d3.max(data)])
  .range([height - margin.bottom, margin.top]);

svg.selectAll('rect').data(data).join('rect')
  .attr('x', (_, i) => x(i))
  .attr('y', d => y(d))
  .attr('width', x.bandwidth())
  .attr('height', d => y(0) - y(d))
  .attr('rx', 4)
  .attr('fill', 'hsl(220, 80%, 55%)');
</script>
```

---

## Three.js

**적합한 용도:** 3D 시각화, 몰입형 데이터 표현, 건축·공간 표현.

```html
<script src="https://cdn.jsdelivr.net/npm/three@0.170/build/three.module.min.js" type="module"></script>
```

**`type="module"`은 Three.js에만 쓴다.** 다른 라이브러리와 같은 파일에서 `import`/`export`를 쓰면 스크립트 전체가 죽는다 — Chart.js UMD 빌드와는 절대 섞지 않는다 (NEVER).

### 언제 쓰는가
- 3D 데이터 시각화 (3D 산점도, 지형)
- 제품·건축 시각화
- 몰입감 있고 인상적인 대표 비주얼
- 2D로는 개념 전달이 부족할 때

---

## Mermaid

**적합한 용도:** 텍스트 정의로 만드는 다이어그램. 순서도, 시퀀스 다이어그램, 간트 차트, ER 다이어그램, 클래스 다이어그램.

```html
<script src="https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js"></script>
<script>mermaid.initialize({ startOnLoad: true, theme: 'neutral' });</script>
```

### 언제 쓰는가
- 빠르게 만드는 순서도와 프로세스 다이어그램
- API·시스템 상호작용을 나타내는 시퀀스 다이어그램
- 프로젝트 일정용 간트 차트
- 맞춤 스타일보다 다이어그램의 정확성이 중요할 때

### 패턴
```html
<pre class="mermaid">
graph TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Action 1]
    B -->|No| D[Action 2]
    C --> E[End]
    D --> E
</pre>
```

### 요령
- Mermaid 문법에서 주석은 `%%`를 쓴다
- 테마: `default`, `neutral`, `dark`, `forest`
- 맞춤 스타일: `style A fill:#f9f,stroke:#333`

---

## Reveal.js

**적합한 용도:** 기본 템플릿으로 부족할 때 쓰는 본격적인 슬라이드 덱.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/theme/white.css">
<script src="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.js"></script>
```

### 언제 쓰는가
- 슬라이드가 중첩된 복잡한 발표 (세로 + 가로)
- 마크다운 기반 슬라이드 내용
- 내장 발표자 노트, PDF 내보내기, 개요 모드
- 기본 슬라이드 템플릿으로 부족할 때

### 요령
- 테마: `white`, `black`, `league`, `beige`, `moon`, `night`, `serif`, `simple`, `solarized`
- 단계별 등장에는 fragment를 쓴다
- 코드 강조는 highlight.js 플러그인으로 한다

---

## Leaflet

**적합한 용도:** 마커, 다각형, 히트맵이 있는 인터랙티브 지도.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet@1/dist/leaflet.css">
<script src="https://cdn.jsdelivr.net/npm/leaflet@1/dist/leaflet.js"></script>
```

### 언제 쓰는가
- 위치 데이터 시각화
- 지역 간 비교
- 이동 경로 시각화
- 위도·경도 좌표가 있는 모든 데이터

### 패턴
```html
<div id="map" style="height: 500px; border-radius: 12px;"></div>
<script>
const map = L.map('map').setView([37.5, -122.3], 10);
L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap'
}).addTo(map);
L.marker([37.5, -122.3]).addTo(map).bindPopup('Location');
</script>
```
