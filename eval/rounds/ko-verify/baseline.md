# 기준선 (before) — 영문 스킬로 생성한 대조군

측정 시각: 2026-08-18 00:0x KST
스킬 버전: `0.4.1` (설치 캐시 = 작업 트리와 동일함을 사전 확인)
모델: `sonnet`
프롬프트: `prompts.md`의 P1~P5 (글자 그대로)

생성된 HTML은 `.gitignore`의 `*.html` 규칙에 걸려 추적하지 않는다.
판정에 필요한 값은 전부 여기와 `../round-ko-verify-before/scores.json`에 있다.

## Layer 2 (결정적 DOM 체크 45개)

| 파일 | 판정 형식 | Layer 2 | 실패한 체크 |
|---|---|---:|---|
| `2026-h1-roadmap.html` | dashboard | **100** | 없음 |
| `blog-announce-card.html` | generic | **100** | 없음 |
| `deploy-tool-pitch.html` | slide-deck | **100** | 없음 |
| `docs-tool-comparison.html` | comparison | **100** | 없음 |
| `saas-q3-dashboard.html` | dashboard | **95** | `no-overflow-375`, `card-padding` |

```
평균 9.90 / 최솟값 9.50 / gate VIRAL / 콘솔 오류 0건
```

**천장 효과에 유의한다.** 5개 중 4개가 만점이므로 Layer 2는 개선을 보여줄 수 없고
**악화만** 보여줄 수 있다. 이번 검증의 질문이 "번역이 성능을 떨어뜨리는가"이므로
이 성질은 오히려 알맞다.

## 출력 언어 — 가장 중요한 판정 항목

| 프롬프트 언어 | 파일 | 한글 문자 수 | `<title>` |
|---|---|---:|---|
| English | `saas-q3-dashboard.html` | **0** | Q3 2026 SaaS Metrics Dashboard |
| English | `deploy-tool-pitch.html` | **0** | Deploy Tool Pitch — 45 Minutes to 6 |
| English | `blog-announce-card.html` | **0** | Why We Moved Off Kubernetes — Engineering Blog |
| 한국어 | `2026-h1-roadmap.html` | 336 | 2026년 상반기 팀 마일스톤 로드맵 |
| 한국어 | `docs-tool-comparison.html` | 768 | 사내 문서 도구 비교: Notion vs Confluence vs Obsidian |

영어 요청 3건이 **한글 0자**라 경계가 선명하다. 번역 후 이 셋 중 하나라도 0을 넘으면
언어 오염이 발생한 것이며 계획서 6단계에 따라 **즉시 기각**한다.

측정 명령:
```bash
cd eval/rounds/ko-verify/before   # 또는 after
for f in *.html; do echo "$f $(grep -o '[가-힣]' "$f" | wc -l)"; done
```

## `examples/` 기준선 (코드 블록 무결성 검사용)

`round-ko-base-examples/scores.json` — 평균 **9.21**, 최솟값 **4.30**, 17개 파일.

이 값은 스킬 품질 지표가 **아니다.** `examples/`는 현재 지시문 이전에 만들어진 파일들이라
(`baseline-findings.md` 발견 2 참조) 스킬 품질을 대변하지 못한다.
번역이 코드 블록이나 식별자를 깨뜨렸는지 확인하는 용도로만 쓴다.
번역 후 이 값이 **정확히 같아야** 한다.

## P1 재생성에 관한 기록

P1 1차 생성분은 `ChartManager` 초기화 순서 문제로 평가 중 크래시했다
(`baseline-findings.md` 발견 1). 크래시한 파일의 0점은 "0점짜리 품질"이 아니라
"평가하지 못함"이므로 대조군으로 쓸 수 없어 한 번 재생성했다.

2차 생성분은 `ChartManager`를 아예 쓰지 않는 다른 패턴을 택했고 95점으로 정상 평가되었다.
**같은 프롬프트가 실행마다 다른 차트 패턴을 만든다**는 뜻이며, 이 결함은 확률적으로 발생한다.

1차 생성분은 스크래치패드에 `p1-run1-chartmanager-crash.html`로 보관했다.

이 재생성은 기준선을 위쪽으로 고르는 셈이라 한글판에 **불리하게** 작용한다.
번역이 실제로 멀쩡한데도 기각될 가능성을 높이는 방향이므로 보수적으로 안전하다.
