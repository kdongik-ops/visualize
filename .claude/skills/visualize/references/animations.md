# Animation Reference — 애니메이션 참조 (CSS 우선, JS 보조)

## 원칙

**CSS 애니메이션이 주 수단이다.** 안정적이고 성능이 좋으며 JS 스코프 문제가 생겨도 깨지지 않는다. JavaScript는 스크롤 등장 효과와 숫자 카운터에만 쓴다.

**규칙: 내용은 CSS 상태에서 ALWAYS 보여야 한다.** JavaScript는 진입 애니메이션을 위해 `.visible` 클래스를 추가할 뿐이다. JS가 실패해도 모든 내용이 그대로 보인다.

## CDN (선택 — 필요할 때만 넣는다)

Motion.js는 고급 스프링 물리 효과를 위해 쓸 수 있지만 필수는 아니다.
```html
<!-- OPTIONAL: only include for spring physics or complex orchestration -->
<script src="https://cdn.jsdelivr.net/npm/motion@12/dist/motion.js"></script>
```

## CSS 애니메이션 (주 수단)

### 진입 애니메이션
```css
/* Base: content is visible by default */
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(24px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
@keyframes slideInLeft {
  from { opacity: 0; transform: translateX(-40px); }
  to { opacity: 1; transform: translateX(0); }
}
@keyframes slideInRight {
  from { opacity: 0; transform: translateX(40px); }
  to { opacity: 1; transform: translateX(0); }
}
@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}

/* Apply with staggered delays */
.animate { animation: fadeInUp 0.6s ease-out both; }
.animate.delay-1 { animation-delay: 0.1s; }
.animate.delay-2 { animation-delay: 0.2s; }
.animate.delay-3 { animation-delay: 0.3s; }
.animate.delay-4 { animation-delay: 0.4s; }
.animate.delay-5 { animation-delay: 0.5s; }
.animate.delay-6 { animation-delay: 0.6s; }

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  .animate, [class*="animate"] { animation: none !important; }
}
```

### 호버 효과 (순수 CSS)
```css
/* Shadow-only hover — no translateY or scale transforms */
.card {
  transition: box-shadow 0.2s ease;
}
.card:hover {
  box-shadow: 0 0 0 1px var(--border), 0 8px 16px rgba(0,0,0,0.08);
}
```

### 스크롤 등장 효과 (CSS + 최소한의 JS)
```css
/* Elements start visible. JS adds .reveal class to opt-in to scroll animation. */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger children */
.reveal.visible .stagger:nth-child(1) { transition-delay: 0.05s; }
.reveal.visible .stagger:nth-child(2) { transition-delay: 0.1s; }
.reveal.visible .stagger:nth-child(3) { transition-delay: 0.15s; }
.reveal.visible .stagger:nth-child(4) { transition-delay: 0.2s; }
.reveal.visible .stagger:nth-child(5) { transition-delay: 0.25s; }
.reveal.visible .stagger:nth-child(6) { transition-delay: 0.3s; }
```

## JavaScript (최소 — 스크롤 감시와 카운터만)

### 스크롤 감시자 (10줄)
```javascript
// Add .reveal class via JS (not CSS) so content is visible without JS
document.querySelectorAll('[data-reveal]').forEach(el => el.classList.add('reveal'));
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); observer.unobserve(e.target); } });
}, { threshold: 0.15 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

**핵심:** `data-reveal` 속성이 스크롤 애니메이션 대상을 표시한다. JS가 `.reveal` 클래스를 붙이고(opacity:0이 된다), 화면에 들어오면 `.visible`이 추가된다(opacity:1이 된다). JS가 실패하면 `.reveal`이 아예 붙지 않으므로 요소는 계속 보인 상태로 남는다.

### 숫자 카운터 (15줄)
```javascript
function animateCounters() {
  document.querySelectorAll('[data-count]').forEach(el => {
    const target = parseFloat(el.dataset.count);
    const prefix = el.dataset.prefix || '';
    const suffix = el.dataset.suffix || '';
    const duration = 1200;
    const start = performance.now();
    function tick(now) {
      const progress = Math.min((now - start) / duration, 1);
      const eased = 1 - Math.pow(1 - progress, 3); // ease-out cubic
      el.textContent = prefix + Math.round(target * eased).toLocaleString() + suffix;
      if (progress < 1) requestAnimationFrame(tick);
    }
    requestAnimationFrame(tick);
  });
}
// Trigger on scroll into view
const counterObserver = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) { animateCounters(); counterObserver.disconnect(); } });
}, { threshold: 0.3 });
const firstCounter = document.querySelector('[data-count]');
if (firstCounter) counterObserver.observe(firstCounter);
```

### 슬라이드 전환 (CSS 기반)
```css
.slide { display: none; opacity: 0; transition: opacity 0.3s ease; }
.slide.active { display: flex; opacity: 1; }
```
```javascript
// Simple slide nav — no animation libraries needed
var current = 0;
var transitioning = false;
function goToSlide(n) {
  if (n === current || n < 0 || n >= slides.length || transitioning) return;
  transitioning = true;
  slides[current].classList.remove('active');
  slides[n].classList.add('active');
  current = n;
  updateUI();
  setTimeout(() => { transitioning = false; }, 350);
}
```

## Motion.js를 언제 쓰는가 (선택)

다음이 꼭 필요할 때만 Motion.js를 넣는다.
- **스프링 물리 효과** — 튕기는 듯한 자연스러운 애니메이션
- **복잡한 연출 제어** — 여러 요소를 정밀한 타이밍 관계로 묶어야 할 때
- **스크롤 연동 애니메이션** — 패럴랙스, 스크롤 위치에 연동된 진행 바

그 외에는 CSS가 더 단순하고 더 안정적이다.

## 안티패턴

- ❌ `Motion.animate().finished.then()` — Motion API가 바뀌면 프로미스 체인이 조용히 깨진다
- ❌ CSS에 `opacity: 0`을 둔 채 `Motion.inView()` 사용 — JS가 실패하면 내용이 안 보인다
- ❌ 초기화 전에 호출되는 차트 변수에 `let` 선언 — 함수 호이스팅 경계를 넘어 참조되는 변수는 `var`로 쓴다
- ❌ JS에서 복잡한 지연 시간 계산 — CSS `nth-child` 지연을 쓴다
- ❌ 기본값으로 내용을 숨기고 JS로 드러내기 — 내용은 JS 없이도 보여야 한다
