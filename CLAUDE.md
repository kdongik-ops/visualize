# CLAUDE.md

**Visualize** 플러그인 저장소다. 어떤 아이디어, 데이터, 내용이든 독립 실행 가능한 HTML 시각화로 바꿔 주는 오픈소스 Claude Code 플러그인이다.

## 저장소 구조

```
visualize/
├── .claude/skills/visualize/
│   ├── SKILL.md               # 스킬 본체 (Claude Code가 로드한다)
│   └── references/            # 상세 규격 (점진적 공개 3단계)
│           ├── design-system.md
│           ├── skeleton.md
│           ├── types.md
│           ├── libraries.md
│           ├── menu.md
│           ├── css-techniques.md
│           ├── animations.md
│           └── eval.md
├── examples/                  # 예시 HTML 15개
├── eval/                      # 평가 기반 구조
│   ├── EVAL.md               # 3계층 채점 규격
│   ├── LOOP.md               # 자가 개선 루프 방법론 (v6)
│   ├── SKILL.md              # 평가자 스킬
│   ├── loop-state.json       # 현재 라운드 + 점수 이력
│   ├── stress-tests.md       # 테스트 케이스 40건
│   ├── OPENCLAW-SETUP.md     # OpenClaw에 cron 작업을 거는 프롬프트
│   ├── pipeline/             # 자동 평가 파이프라인 (Node.js)
│   │   ├── run.js            # 오케스트레이터: node run.js --dir examples/
│   │   ├── format-detect.js  # Layer 1: 시각화 형식 자동 판정
│   │   ├── vision-eval.js    # Layer 3: 육안 평가 보조 (API가 아니라 에이전트가 채점)
│   │   ├── checks/           # Layer 2: 결정적 DOM 체크 45개
│   │   ├── calibration/      # 육안 평가용 기준점 스크린샷
│   │   └── LOOP-PROMPT.md    # 자가 개선 루프 에이전트 프롬프트
│   ├── rounds/               # 라운드별 결과
│   └── archive/              # 라운드 1~38 (과거 기록)
├── docs/plans/               # 설계·구현 문서
├── CLAUDE.md                  # 이 파일
├── README.md                  # GitHub 노출용 문서
└── LICENSE                    # MIT
```

## 스킬을 쓰는 법

스킬이 `.claude/skills/visualize/`에 있으므로 이 저장소에서 Claude Code를 열면 자동으로
로드된다. 설치 명령도, 플러그인 매니페스트도 없다. 스킬 이름은 `visualize`다.

다른 곳에서 쓰려면 그 폴더를 대상 프로젝트의 `.claude/skills/`나, 전역으로 쓰려면
`~/.claude/skills/`로 복사한다.

**여기서 고친 내용은 다음 세션에 바로 반영된다.** 예전에는 플러그인으로 설치해서
`~/.claude/plugins/cache/`의 사본이 로드됐고, 버전을 올려 재설치하기 전에는 수정이
반영되지 않았다. 그 단계는 사라졌다.

플러그인 배포는 나중에 별도로 만든다.

## 주요 결정 사항

- **단일 파일 HTML 출력** — CSS/JS 인라인, 아무 브라우저에서나 열린다
- **다크 테마 기본** — 클래스 기반 테마 (NO @media prefers-color-scheme)
- **CSS 우선 애니메이션** — Motion.js 없이 순수 @keyframes + 바닐라 JS
- **최상위 JS는 전부 `var`** — TDZ 오류를 막는다 (과거 여러 버그의 근본 원인)
- **CDN 라이브러리 권장** — Chart.js, D3, Tailwind, Mermaid, Reveal.js, html-to-image
- **Inter 글꼴** (Google Fonts) + CJK·아랍어용 Noto Sans
- **시각적 절제** — 떠다니는 구, 그라디언트 텍스트, 확대 변형, 발광 효과 금지

## 이 저장소에서 작업할 때

`.claude/skills/visualize/SKILL.md`를 고칠 때:
- **5,000단어** 미만으로 유지한다 (점진적 공개에 관한 Anthropic 모범 사례)
- 상세 규격은 `.claude/skills/visualize/references/`로 옮기고 SKILL.md에서 링크한다
- YAML 프론트매터에는 `name`과, 트리거 문구가 들어간 `description`이 있어야 한다

예시를 고칠 때:
- 항상 `.claude/skills/visualize/references/skeleton.md`의 스켈레톤에서 시작한다
- 다크와 라이트 테마를 모두 확인한다
- 햄버거 메뉴를 확인한다 (테마 전환, PNG 내려받기, 인쇄)

평가를 돌릴 때:
- `eval/loop-state.json`이 라운드 번호와 점수 이력을 추적한다
- 품질 게이트: SHIP 9.0 이상, ACCEPTABLE 8.0 이상, NEEDS WORK 7.0 이상
- **자동 파이프라인:** `cd eval/pipeline && node run.js --dir ../../examples/`
  - Layer 1(형식 판정) + Layer 2(DOM 체크 45개)를 돌리고 스크린샷을 찍는다
  - Layer 3(시각·정보구조 품질)은 AI 에이전트가 스크린샷을 보고 채점한다
- **자가 개선 루프:** `eval/pipeline/LOOP-PROMPT.md`를 Claude Code나 OpenClaw 에이전트에 넘긴다
- **자동 스케줄링:** OpenClaw에 cron 작업(3시간마다)을 거는 방법은 `eval/OPENCLAW-SETUP.md` 참조
- API 키는 필요 없다 — 에이전트 하네스(Claude Code, OpenClaw)가 인증을 처리한다
- 3계층 채점 규격은 `eval/EVAL.md` 참조

## 스킬 문서의 언어

`SKILL.md`와 `references/`의 **산문은 한글, 아래 항목은 영문 그대로**다.

- 프론트매터의 `name`과 `description` — 영어 트리거 문구가 여기 들어 있다.
  번역하면 영어로 요청할 때 스킬이 뜨지 않는다
- 코드 블록과 인라인 코드 전부 — CSS 변수명, 함수명, 클래스명, CDN URL.
  Layer 2의 45개 체크가 이 문자열을 그대로 찾는다
- `MANDATORY` / `CRITICAL` / `NEVER` / `REQUIRED` / `MUST` 대문자 강조어
- 형식 이름 (`Slide Deck`, `Dashboard`, `Timeline` 등) — Layer 1 형식 판정과 짝을 이룬다

번역 전후를 각각 8건씩 생성해 대조한 결과는 `eval/rounds/ko-verify/verdict.md`에 있다.
같은 스킬로 같은 프롬프트를 돌려도 결과가 흔들리므로, **표본 1건으로 회귀를 판단하지 말 것.**

## 자주 걸리는 함정

- Chart.js: 차트를 만들기 전에 `Chart.defaults.animation = false`를 반드시 넣는다
- 테마 전환: 차트는 파기하고 다시 만들어야 한다 (색을 렌더 시점에 읽는다)
- 최상위에서 `let`/`const` → TDZ 오류. 항상 `var`를 쓴다.
- `@media (prefers-color-scheme)`는 `.theme-dark` 클래스와 충돌한다 → 클래스 기반만 쓴다
- 생성된 파일이 현재 폴더가 아니라 `~/Downloads/`나 저장소 루트에 떨어질 수 있다.
  `run.js --dir`로 채점하기 전에 세 곳을 훑어 모을 것 (`eval/pipeline/LOOP-PROMPT.md` 참조)
