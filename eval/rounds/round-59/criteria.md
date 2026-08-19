# Round 59 — 판정 기준 (생성 전에 미리 정한다)

이번 라운드의 목적은 **"SKILL.md 정리(커밋 `c1a0b47`)가 해를 끼쳤는가"** 하나다.
차트 실패의 원인이던 CDN 파일명은 `f01c7a1`이 이미 고쳤고, 그 사실은 편집 전에
따로 확인했다 (8월 생성물 10건: avg 9.94/9.90, 콘솔 오류 0건).

## 기준선

| 항목 | 값 | 출처 |
|---|---|---|
| R58 평균 | 8.44 (ACCEPTABLE) | `loop-state.json` |
| `examples/` L2 (편집 전) | 9.65 / min 8.90 / SHIP | 0단계 (a) |
| `examples/` L2 (편집 후) | 9.65 / min 8.90 / SHIP, 17건 전부 동일 | 검증 B |
| 8월 생성물 10건 (CDN 수정 후) | 9.94 / 9.90, 오류 0 | 0단계 (b) |

## 통과 조건

1. 전체 평균 **8.44 이상** — 라운드 편차가 크므로(R44 6.13 ~ R47 9.03) **±0.5는 잡음**으로 본다
2. `consoleErrors`에 `Cannot use import statement outside a module` **0건**
3. 차트 체크 6개(`chart-animation-disabled`, `chart-tooltips-enabled`,
   `chart-container-dimensions`, `chart-maintain-aspect-ratio`, `chart-theme-colors`,
   `chart-accessibility`) 전부 통과
4. R57형 실패 없음 — 메뉴·테마 클래스·인쇄 스타일 체크가 통과할 것
   (파일이 짧아져 스켈레톤 지시가 잘 보이기를 기대하지만 **주장이 아니라 관찰 대상**)

## 해석에서 조심할 것

차트가 잘 나오더라도 **정리 덕분이라고 기록하지 않는다.** CDN 수정이 이미 원인을
제거했다. 이 라운드가 말해 주는 것은 "정리가 해를 끼치지 않았다"까지다.

## 이번 라운드에서 하지 않는 것

`loopsSinceResearch = 5`로 연구 단계 조건에 걸리지만 **건너뛴다.** 연구 단계는
SKILL.md에 내용을 추가하는 작업이라 위 판정을 오염시킨다. 다음 라운드로 미룬다.
