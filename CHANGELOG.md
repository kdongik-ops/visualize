# 변경 이력

Visualize 플러그인의 주요 변경 사항을 기록합니다.
형식은 [Keep a Changelog](https://keepachangelog.com/)를 따릅니다.

## v0.3.0 — 평가 체계 성숙

### 추가
- eval 라운드 39회 완료 (R1~R39, R39 시점 평균 8.1점)
- 캐러셀 카드 형식: 인스타그램/링크드인용 다중 슬라이드 캐러셀
- 시각화 형식 추가: 이벤트 포스터, 인용 카드, 절차 안내, 상태 보고서
- `skills/visualize/references/`에 상세 참조 문서 8개 (design-system, skeleton, types, libraries, menu, css-techniques, animations, eval)
- 모든 결과물이 따라야 하는 필수 HTML 템플릿 `skeleton.md`
- 형식별 인터랙션 요구사항 (Popover API, 배타적 아코디언, 앵커 포지셔닝)
- 모바일 화면에 맞추는 단일 화면 포스터 규칙
- Noto Sans를 통한 CJK 글꼴 지원 (한글 캐러셀 예시)
- 시각적 절제 지침 (떠다니는 광구, 그라디언트 텍스트, 확대 변형, 발광 효과 금지)

### 변경
- 클래스 기반 테마 (`html.theme-dark` / `html.theme-light`)로 전환, `@media prefers-color-scheme` 제거
- TDZ 오류를 막기 위해 최상위 JS 선언을 전부 `var`로 통일
- Chart.js 연동 개선: `Chart.defaults.animation = false`, 테마 전환 시 파기 후 재생성
- `color-mix()`를 쓴 테마 대응 슬라이드 그라디언트
- `plugin.json`과 마켓플레이스를 지원하는 Claude Code 플러그인 구조로 재편
- 카드 호버를 그림자 전용 효과로 변경 (`translateY` / `scale` 변형 제거)

### 수정
- Chart.js 캔버스 재사용 크래시 (resetCanvas 패턴 도입)
- 다크 모드가 과하게 어두워지던 테마 상속 버그
- ai-timeline의 라이트 테마 깨짐
- 빈약한 슬라이드 내용, 깨진 SVG 경로, 잘린 도넛 차트 범례

## v0.2.0 — 평가 루프 기반 구축

### 추가
- 비전 모델 채점을 사용하는 평가 루프 방법론 (LOOP.md)
- 모든 시각화 형식을 아우르는 예시 HTML 15개
- 디자인 시스템 참조 문서 (`design-system.md`)
- 모든 예시에 공통으로 적용되는 표준 메뉴 시스템
- CSS 우선 애니메이션 전략 (Motion.js 의존성 대체)
- 스크롤 등장 효과, 호버 효과, 진입 애니메이션
- 시각적 검증을 위한 평가 루프 스크린샷 캡처
- 모든 예시에 모바일 분기점과 반응형 레이아웃 적용

### 변경
- 모든 예시를 CSS 우선 스켈레톤 기반으로 재작성 (Motion.js 제거)
- 크기 규칙과 텍스트 가시성 규칙을 담은 필수 HTML 스켈레톤 도입

### 수정
- 최상위 `let`/`const`로 인한 TDZ 오류 (`var`로 전환)
- Motion.js `animate().finished` 프로미스 체인 실패
- 불안정한 `Motion.finished`에 의존하던 피치덱 슬라이드 내비게이션

## v0.1.0 — 최초 배포

### 추가
- 디자인 시스템, 출력 규칙, 작업 절차를 담은 스킬 본체 (`SKILL.md`)
- 시각화 형식 10종: 슬라이드 덱, 인포그래픽, 대시보드, 순서도, 타임라인, 비교, 데이터 시각화, 한 장 요약, 마인드맵, 칸반
- 햄버거 메뉴 컴포넌트 (다크/라이트/자동 테마, PNG 내려받기, PDF 인쇄)
- CDN 라이브러리 참조 (Chart.js, D3.js, Three.js, Mermaid, Reveal.js, Leaflet)
- 시작용 템플릿 3종 (슬라이드 덱, 대시보드, 인포그래픽)
- 5개 항목 품질 루브릭을 갖춘 평가 스킬
- 7개 범주에 걸친 스트레스 테스트 40건
- 반복 개선 루프 방법론 (LOOP.md)
