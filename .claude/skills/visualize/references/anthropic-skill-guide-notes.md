# Anthropic 스킬 모범 사례 — 핵심 정리

출처: "The Complete Guide to Building Skills for Claude" (Anthropic, 2026년 1월)

## 반드시 지켜야 할 규칙

1. **SKILL.md 파일명** — 정확히 `SKILL.md` (대소문자 구분) ✅ 충족
2. **폴더명** — kebab-case만 사용 ✅ `visualize/`
3. **스킬 폴더 안에 README.md 금지** — 모든 문서는 SKILL.md 또는 references/에 ⚠️ 확인 필요
4. **YAML 프론트매터** — `---` 구분자가 있어야 하고 `name`과 `description`은 필수
5. **프론트매터에 XML 태그(`<`, `>`) 금지** — 보안 제약
6. **description에는 '무엇을'과 '언제'가 들어가야 함** — 트리거 문구가 결정적이다
7. **SKILL.md는 5,000단어 미만** — 상세 문서는 references/로 옮긴다
8. **스킬 이름에 "claude"나 "anthropic" 금지** — 예약어

## 점진적 공개 (3단계)

- **1단계 (YAML 프론트매터)**: 항상 시스템 프롬프트에 들어간다. 발동에 필요한 최소한만 담는다.
- **2단계 (SKILL.md 본문)**: 스킬이 관련 있을 때 로드된다. 전체 지시문.
- **3단계 (references/)**: 링크된 파일로, 필요할 때만 로드된다.

## 이 스킬의 분류

**분류 1: 문서 및 자산 생성(Document & Asset Creation)** — 일관되고 품질 높은 결과물을 만드는 유형.

## 해당하는 패턴

- **패턴 3: 반복 개선(Iterative refinement)** — 이 저장소의 eval 루프 방식과 일치
- **패턴 5: 도메인 특화 지식(Domain-specific intelligence)** — 이 스킬의 디자인 시스템 지식

## 고쳐야 할 것

1. 스킬 폴더 안에 README.md가 있는지 확인 (있으면 금지 위반)
2. SKILL.md가 너무 길 수 있음 (5,000단어 초과) — references/로 더 옮길 것
3. YAML description에 명확한 트리거 문구가 들어갔는지 확인
4. `metadata` 필드 추가 (author, version)
5. 프론트매터에 `license: MIT` 추가 검토
6. 해당된다면 `allowed-tools` 추가
