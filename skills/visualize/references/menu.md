# 햄버거 메뉴 컴포넌트 (Hamburger Menu Component)

모든 시각화는 오른쪽 위에 햄버거 메뉴를 반드시 포함해야 하며, 다음 기능을 갖춘다.
1. **테마 전환** — 다크/라이트 모드 (또는 여러 테마 중 선택)
2. **이미지로 내려받기** — html-to-image를 이용한 PNG 내보내기
3. **PDF로 인쇄** — 인쇄 스타일이 최적화된 브라우저 인쇄

## 필수 CDN

```html
<script src="https://cdn.jsdelivr.net/npm/html-to-image@1.11.11/dist/html-to-image.js"></script>
```

## 메뉴 전체 구현

모든 시각화에 그대로 붙여 넣는다. 테마 색은 해당 시각화의 팔레트에 맞춰 조정한다.

### HTML (`<body>` 바로 안에 배치)

```html
<!-- Hamburger Menu -->
<div class="viz-menu">
  <button class="viz-menu-toggle" onclick="toggleMenu()" aria-label="Menu">
    <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
      <line x1="3" y1="5" x2="17" y2="5"/>
      <line x1="3" y1="10" x2="17" y2="10"/>
      <line x1="3" y1="15" x2="17" y2="15"/>
    </svg>
  </button>
  <div class="viz-menu-dropdown" id="vizMenuDropdown">
    <button onclick="cycleTheme()">
      <span class="viz-menu-icon">◐</span>
      <span>Theme: <span id="themeLabel">Dark</span></span>
    </button>
    <button onclick="downloadImage()">
      <span class="viz-menu-icon">⤓</span>
      <span>Download as PNG</span>
    </button>
    <button onclick="printPDF()">
      <span class="viz-menu-icon">⎙</span>
      <span>Print / Save PDF</span>
    </button>
  </div>
</div>
```

### CSS

```css
/* === Hamburger Menu === */
.viz-menu {
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 9999;
  font-family: var(--font, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif);
}

.viz-menu-toggle {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid var(--menu-border, rgba(128,128,128,0.3));
  background: var(--menu-bg, rgba(0,0,0,0.5));
  color: var(--menu-text, #fff);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  transition: all 0.2s ease;
}

.viz-menu-toggle:hover {
  background: var(--menu-bg-hover, rgba(0,0,0,0.7));
  transform: scale(1.05);
}

.viz-menu-dropdown {
  position: absolute;
  top: calc(100% + 8px);
  right: 0;
  min-width: 200px;
  background: var(--menu-bg, rgba(0,0,0,0.85));
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid var(--menu-border, rgba(128,128,128,0.2));
  border-radius: 12px;
  padding: 6px;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-8px) scale(0.96);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
}

.viz-menu-dropdown.open {
  opacity: 1;
  visibility: visible;
  transform: translateY(0) scale(1);
}

.viz-menu-dropdown button {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 14px;
  border: none;
  background: transparent;
  color: var(--menu-text, #fff);
  font-size: 0.875rem;
  cursor: pointer;
  border-radius: 8px;
  transition: background 0.15s;
  text-align: left;
  white-space: nowrap;
}

.viz-menu-dropdown button:hover {
  background: var(--menu-item-hover, rgba(255,255,255,0.1));
}

.viz-menu-icon {
  font-size: 1rem;
  width: 20px;
  text-align: center;
  flex-shrink: 0;
}

/* Hide menu in print */
@media print {
  .viz-menu { display: none !important; }
}
```

### JavaScript

```javascript
// === Menu toggle ===
function toggleMenu() {
  document.getElementById('vizMenuDropdown').classList.toggle('open');
}

// Close menu when clicking outside
document.addEventListener('click', (e) => {
  if (!e.target.closest('.viz-menu')) {
    document.getElementById('vizMenuDropdown').classList.remove('open');
  }
});

// Close menu on Escape
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') document.getElementById('vizMenuDropdown').classList.remove('open');
});

// === Theme system ===
const themes = [
  { name: 'Dark', class: 'theme-dark' },
  { name: 'Light', class: 'theme-light' },
  { name: 'Auto', class: 'theme-auto' },
];
let currentTheme = 0;

function cycleTheme() {
  // Remove all theme classes
  themes.forEach(t => document.documentElement.classList.remove(t.class));
  // Advance to next
  currentTheme = (currentTheme + 1) % themes.length;
  document.documentElement.classList.add(themes[currentTheme].class);
  document.getElementById('themeLabel').textContent = themes[currentTheme].name;
  // Persist
  localStorage.setItem('viz-theme', currentTheme);
}

// Restore saved theme on load
(function() {
  const saved = localStorage.getItem('viz-theme');
  if (saved !== null) {
    currentTheme = parseInt(saved);
    document.documentElement.classList.add(themes[currentTheme].class);
    document.getElementById('themeLabel').textContent = themes[currentTheme].name;
  }
})();

// === Download as PNG ===
async function downloadImage() {
  const btn = event.target.closest('button');
  const origText = btn.querySelector('span:last-child').textContent;
  btn.querySelector('span:last-child').textContent = 'Generating...';

  try {
    // Hide menu during capture
    const menu = document.querySelector('.viz-menu');
    menu.style.display = 'none';

    const dataUrl = await htmlToImage.toPng(document.body, {
      quality: 1,
      pixelRatio: 2, // 2x for retina quality
      cacheBust: true,
      filter: (node) => !node.classList?.contains('viz-menu'),
    });

    menu.style.display = '';

    // Trigger download
    const link = document.createElement('a');
    link.download = `${document.title || 'visualization'}.png`;
    link.href = dataUrl;
    link.click();
  } catch (err) {
    console.error('Image export failed:', err);
    alert('Image export failed. Try Print → Save as PDF instead.');
  }

  btn.querySelector('span:last-child').textContent = origText;
}

// === Print as PDF ===
function printPDF() {
  window.print();
}
```

## 테마 CSS 패턴

다크(기본값)용 테마 변수를 `:root`에 정의하고, `.theme-light`에서 덮어쓴다.

```css
:root, .theme-dark {
  --bg: hsl(220, 20%, 8%);
  --surface: hsl(220, 15%, 14%);
  --text: hsl(220, 10%, 92%);
  --text-secondary: hsl(220, 10%, 60%);
  --accent: hsl(220, 85%, 68%);
  /* Menu overrides for dark */
  --menu-bg: rgba(0, 0, 0, 0.7);
  --menu-bg-hover: rgba(0, 0, 0, 0.85);
  --menu-border: rgba(255, 255, 255, 0.1);
  --menu-text: #fff;
  --menu-item-hover: rgba(255, 255, 255, 0.1);
}

.theme-light {
  --bg: hsl(220, 15%, 97%);
  --surface: hsl(0, 0%, 100%);
  --text: hsl(220, 20%, 15%);
  --text-secondary: hsl(220, 10%, 45%);
  --accent: hsl(220, 80%, 50%);
  /* Menu overrides for light */
  --menu-bg: rgba(255, 255, 255, 0.85);
  --menu-bg-hover: rgba(255, 255, 255, 0.95);
  --menu-border: rgba(0, 0, 0, 0.1);
  --menu-text: #333;
  --menu-item-hover: rgba(0, 0, 0, 0.06);
}

.theme-auto {
  /* Uses prefers-color-scheme */
}

@media (prefers-color-scheme: light) {
  .theme-auto {
    --bg: hsl(220, 15%, 97%);
    --surface: hsl(0, 0%, 100%);
    --text: hsl(220, 20%, 15%);
    --text-secondary: hsl(220, 10%, 45%);
    --accent: hsl(220, 80%, 50%);
    --menu-bg: rgba(255, 255, 255, 0.85);
    --menu-bg-hover: rgba(255, 255, 255, 0.95);
    --menu-border: rgba(0, 0, 0, 0.1);
    --menu-text: #333;
    --menu-item-hover: rgba(0, 0, 0, 0.06);
  }
}
```

## 인쇄 스타일

최적화된 인쇄 스타일을 항상 포함한다.

```css
@media print {
  .viz-menu,
  .nav,
  .progress { display: none !important; }

  body {
    background: white !important;
    color: black !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* For slide decks: print all slides */
  .slide {
    position: relative !important;
    opacity: 1 !important;
    transform: none !important;
    page-break-after: always;
    height: 100vh;
    display: flex !important;
  }

  .slide.prev { display: flex !important; }
}
```

## 모범 사례

1. **html2canvas 대신 html-to-image** — SVG 지원이 낫고, 번들이 작고, CSS 사용자 정의 속성을 제대로 처리한다
2. **`pixelRatio: 2`** — 레티나 품질로 내보내기 위함
3. **캡처 중 UI 요소 숨기기** (메뉴, 내비게이션, 진행 바)
4. **`cacheBust: true`** — 오래된 이미지 캐시 문제를 피한다
5. **`filter` 함수** — 인터랙티브 요소를 내보내기에서 제외한다
6. **대체 수단** — html-to-image가 실패하면(CORS, 복잡한 SVG) 인쇄 → PDF로 저장을 안내한다
7. **`localStorage`** — 페이지를 새로 고쳐도 테마가 유지되도록 한다
8. **`backdrop-filter`** — 메뉴에 서리 유리 효과를 준다
9. **`print-color-adjust: exact`** — 인쇄할 때 색을 보존한다
10. **페이지 나누기** — 슬라이드 덱에서 각 슬라이드가 PDF 한 페이지가 되도록 한다

## 슬라이드 덱 예외 처리

슬라이드 덱에서는 내려받기 버튼이 페이지 전체가 아니라 **현재 슬라이드만** 캡처해야 한다.

```javascript
async function downloadImage() {
  // For slide decks, capture only the active slide
  const target = document.querySelector('.slide.active') || document.body;
  const menu = document.querySelector('.viz-menu');
  const nav = document.querySelector('.nav');

  menu.style.display = 'none';
  if (nav) nav.style.display = 'none';

  const dataUrl = await htmlToImage.toPng(target, {
    quality: 1,
    pixelRatio: 2,
    width: target.scrollWidth,
    height: target.scrollHeight,
  });

  menu.style.display = '';
  if (nav) nav.style.display = '';

  const link = document.createElement('a');
  const slideNum = document.querySelector('.slide.active')?.dataset?.slide || '';
  link.download = `${document.title}-slide${slideNum}.png`;
  link.href = dataUrl;
  link.click();
}
```
