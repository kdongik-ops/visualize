# ⚡ Visualize

**어떤 아이디어든 프롬프트 한 줄로 아름다운 HTML 시각화로 바꿉니다.**

자연어로 설명하면 독립 실행 가능한 HTML 시각화를 만들어 주는 [Claude Code](https://code.claude.com) 플러그인입니다. 슬라이드 덱, 대시보드, 인포그래픽, 순서도, 타임라인 등을 모두 **어디서나 열리는 단일 HTML 파일**로 만듭니다.

> HTML은 "웹사이트"가 아니다. 시각화 도구다. 코드는 값싸다. 누구든 무엇이든 시각화할 수 있어야 한다.

## 무엇을 하는가

시각화하고 싶은 것을 설명하면 → 바로 쓸 수 있는 품질의 HTML 파일이 나옵니다.

```
당신: "우리 AI 스타트업 피치덱 만들어줘"
→ pitch-deck.html (인터랙티브 슬라이드, 다크/라이트 테마, 키보드 내비게이션, PNG/PDF 내보내기)

당신: "이 CSV 데이터를 대시보드로 시각화해줘"
→ sales-dashboard.html (KPI 카드, Chart.js 그래프, 반응형 그리드)

당신: "원격근무 트렌드 인포그래픽 만들어줘"
→ remote-work-infographic.html (큰 숫자, 스크롤 애니메이션, 인쇄 대응)
```

### 지원하는 시각화 형식

| 형식 | 설명 |
|------|-------------|
| 🎯 **Slide Deck** | 키보드 내비게이션, 전환 효과, 발표자 노트가 있는 프레젠테이션 |
| 📊 **Dashboard** | KPI 카드, 차트, 지표 — Chart.js 기반 |
| 📈 **Infographic** | 애니메이션이 있는 스크롤형 비주얼 스토리텔링 |
| 🔀 **Flowchart** | 프로세스 다이어그램, 의사결정 트리, 아키텍처 도식 |
| 📅 **Timeline** | 시간순 사건, 로드맵, 마일스톤 |
| ⚖️ **Comparison** | 나란히 놓는 기능 비교, 장단점 매트릭스 |
| 📉 **Data Viz** | 원시 데이터로 만드는 막대·선·원·레이더 차트 |
| 📄 **One-Pager** | 랜딩 페이지, 요약, 브리프 |
| 🧠 **Mind Map** | 개념 관계도, 브레인스토밍 시각물 |
| 📋 **Kanban** | 상태 보드, 항목 분류 추적 |

## Gamma / Canva / PowerPoint 대신 쓸 이유

| | Gamma/Canva | PowerPoint | **Visualize** |
|---|---|---|---|
| **비용** | 월 $10~40 | 라이선스 $100+ | **무료** |
| **결과물** | 독자 포맷 | .pptx | **표준 HTML** |
| **커스터마이징** | 템플릿 범위 안 | 수작업 | **제한 없음** |
| **인터랙션** | 제한적 | 없음 | **HTML/CSS/JS 전부** |
| **AI 친화성** | 나중에 붙임 | Copilot 애드온 | **핵심 워크플로** |
| **오프라인** | 불가 | 가능 | **가능** |
| **버전 관리** | 불가 | 사실상 불가 | **가능 (텍스트니까)** |
| **파일 크기** | 해당 없음 | 10MB 이상 | **약 20KB** |

## 기능

모든 시각화에 기본으로 들어갑니다.

- 🌙 **다크 / 라이트 / 자동 테마** — 햄버거 메뉴에서 전환, localStorage에 저장
- 📥 **PNG 내려받기** — html-to-image로 2배 레티나 품질
- 🖨️ **인쇄 / PDF 저장** — `@media print` 스타일 최적화
- 📱 **반응형** — 데스크톱·태블릿·모바일 모두 대응
- ⌨️ **키보드 내비게이션** — 슬라이드 덱에서 방향키 사용
- 🎨 **기본값이 이미 아름다움** — 전문적인 타이포그래피, HSL 색 체계, 8px 간격 그리드
- 📦 **단일 파일** — 전부 인라인, 선택적 CDN 라이브러리 외에 의존성 없음
- ♿ **접근성** — 시맨틱 HTML, WCAG AA 명도 대비

### CDN 라이브러리 (선택 — 도움이 될 때만 사용)

- [Chart.js](https://www.chartjs.org/) — 설정 없이 바로 쓰는 차트
- [D3.js](https://d3js.org/) — 복잡한 맞춤 데이터 시각화
- [Mermaid](https://mermaid.js.org/) — 텍스트로 정의하는 다이어그램
- [Three.js](https://threejs.org/) — 3D 시각화
- [Leaflet](https://leafletjs.com/) — 인터랙티브 지도
- [Reveal.js](https://revealjs.com/) — 본격적인 슬라이드 엔진

## 설치

### Claude Code 플러그인 (권장)

```bash
# 1단계: 마켓플레이스 등록 (최초 1회)
claude plugin marketplace add kdongik-ops/visualize

# 2단계: 플러그인 설치
claude plugin install visualize@careerhackeralex
```

나중에 업데이트할 때:
```bash
claude plugin update visualize@careerhackeralex
```

> 저장소 경로(`kdongik-ops`)와 플러그인 식별자의 마켓플레이스 이름(`careerhackeralex`)이
> 다른 것은 의도된 것입니다. 저장소는 fork로 옮겼지만 `.claude-plugin/marketplace.json`의
> `name`은 그대로 두었습니다. 이 이름을 바꾸면 이미 설치된 사본이 깨집니다.

### 수동 설치

```bash
# 저장소 복제
git clone https://github.com/kdongik-ops/visualize.git

# Claude Code는 .claude-plugin/plugin.json 이 있는 플러그인을 자동으로 인식합니다.
# 복제한 디렉터리에서 Claude Code를 열거나, 플러그인 디렉터리로 등록하세요:
claude plugin install --plugin-dir /path/to/visualize
```

## 사용법

설치한 뒤에는 Claude Code에 그냥 시각화를 요청하면 됩니다.

```
"우리 4분기 실적으로 발표 자료 만들어줘"
"이 데이터를 대시보드로 시각화해줘: [CSV/JSON 붙여넣기]"
"이 글 요약해서 인포그래픽으로 만들어줘: [URL 붙여넣기]"
"우리 배포 프로세스를 순서도로 보여줘"
"AI 발전사를 타임라인으로 만들어줘"
```

시각화와 관련된 요청이면 스킬이 자동으로 발동합니다.

## 예시

결과물 샘플은 [`examples/`](examples/) 디렉터리를 보세요.

## 저장소 구조

```
visualize/
├── .claude-plugin/
│   └── plugin.json             # 플러그인 매니페스트
├── skills/
│   └── visualize/
│       ├── SKILL.md            # 스킬 본체 지시문
│       └── references/         # 디자인 시스템, 스켈레톤, 패턴
├── examples/                   # 샘플 HTML 결과물 15개
├── eval/                       # 품질 검증 및 개선 루프
├── research/                   # 디자인 리서치 노트
├── CLAUDE.md                   # Claude Code 프로젝트 지침
├── README.md                   # 이 파일
└── LICENSE                     # MIT
```

## 동작 방식

1. 시각화하고 싶은 것을 설명한다
2. Claude Code가 스킬 지시문(디자인 시스템, 패턴, 모범 사례)을 읽는다
3. CSS/JS가 인라인으로 들어간 `.html` 파일 하나를 생성한다
4. 아무 브라우저로 열면 끝

이 스킬은 전문 디자인 지식(타이포그래피 배율, 색 이론, 간격 리듬, 애니메이션 모범 사례)을 지시문에 담아 두었기 때문에, 손으로 디자인하지 않아도 결과물이 다듬어져 나옵니다.

## 기여하기

품질 개선에는 체계적인 평가 루프를 씁니다.

1. **생성** — `eval/stress-tests.md`의 테스트 케이스를 실행한다
2. **평가** — 8개 항목 루브릭으로 결과물을 채점한다
3. **스킬을 고친다** — 개별 결과물이 아니라 `SKILL.md`나 references를 고친다
4. **재평가** — 수정이 여러 테스트 케이스에서 통하는지 확인한다
5. **출시** — 종합 점수 9.0 이상이고 어느 항목도 8 미만이 아닐 때

전체 방법론은 [`eval/LOOP.md`](eval/LOOP.md)를 보세요.

### 품질 기준

사람들이 이렇게 반응하는 시각화를 목표로 합니다.

- 스크린샷을 찍어 소셜미디어에 공유한다
- 실제 회의에서 민망하지 않게 쓴다
- Gamma나 Canva 결과물보다 이쪽을 고른다
- "이거 어떻게 만들었어요?"라고 묻는다

기준은 "AI가 만든 것치고 괜찮다"가 아닙니다. 기준은 **"그냥 좋다" (good, period)** 입니다.

## 라이선스

MIT — [`LICENSE`](LICENSE) 참조. Copyright (c) 2025 SangHyeon (Alex) Ahn.

## 크레딧

이 저장소는 [Career Hacker Alex](https://youtube.com/@CareerHackerAlex) (커리어해커 알렉스)가
만들어 MIT 라이선스로 공개한 [visualize](https://github.com/careerhackeralex/visualize)의
fork입니다.
