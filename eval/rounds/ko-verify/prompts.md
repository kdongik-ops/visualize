# 한글화 검증용 고정 프롬프트 5개

번역 전(`before/`)과 번역 후(`after/`)에 **글자 하나 바꾸지 않고** 동일하게 사용한다.

모델: `sonnet` 고정. 명령: `claude -p --dangerously-skip-permissions --model sonnet "<프롬프트>"`

## 구성 근거

| # | 언어 | 형식 | 차트 | 검증 목적 |
|---|---|---|---|---|
| 1 | English | Dashboard | O | 가장 흔한 형식. 차트 컨테이너·KPI 규칙 |
| 2 | English | Slide deck | X | 16:9 고정, 내비게이션, 테마별 그라디언트 |
| 3 | English | 1:1 Poster | X | `overflow: hidden` 고정 크기, 메뉴 없음 규칙 |
| 4 | 한국어 | Timeline | X | 한글 타이포그래피(Noto Sans KR) 규칙 |
| 5 | 한국어 | Comparison | O | 표 + 차트 혼합 |

**영어 3개는 출력 언어 오염을 잡기 위한 것이다.** 지시문을 한글로 바꾼 뒤에도 영어 요청에는 영어 결과물이 나와야 한다. 하나라도 한글 결과물이 나오면 번역을 기각한다.

---

## P1 (English, dashboard + chart)

```
Build a dashboard for a SaaS company's Q3 2026 metrics: MRR $2.4M (up 18% QoQ), 1,240 active accounts, monthly churn 3.2%, NPS 47. Include a monthly revenue trend chart for Jul/Aug/Sep showing 1.9M, 2.1M, 2.4M. Save as saas-q3-dashboard.html
```

## P2 (English, slide deck)

```
Make a 6-slide deck pitching an internal tool that cuts our deployment time from 45 minutes to 6 minutes. Cover the problem, the cost of waiting, how it works, the numbers, rollout plan, and the ask. Save as deploy-tool-pitch.html
```

## P3 (English, 1:1 poster)

```
Create a 1:1 square social card announcing our engineering blog post titled "Why we moved off Kubernetes". Include the title, a one-line hook, and our handle. Save as blog-announce-card.html
```

## P4 (한국어, timeline)

```
우리 팀의 2026년 상반기 마일스톤을 타임라인으로 정리해줘. 1월 아키텍처 설계 착수, 3월 프로토타입 완성, 4월 사내 베타 오픈, 6월 정식 배포. 각 단계마다 담당 조직과 핵심 지표를 넣어줘. 파일명은 2026-h1-roadmap.html 로 저장해줘
```

## P5 (한국어, comparison + chart)

```
사내 문서 도구 3개를 비교하는 페이지를 만들어줘. Notion, Confluence, Obsidian 을 비용, 검색 품질, 오프라인 지원, 학습 곡선 네 가지 기준으로 비교하고 종합 점수를 차트로 보여줘. 파일명은 docs-tool-comparison.html 로 저장해줘
```
