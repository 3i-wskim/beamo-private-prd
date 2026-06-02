# Beamo Private PRD — Claude Work Instructions

## 첫 세션 필수: 역할 확인

이 모듈에 접근하는 모든 사용자에게 **반드시** 역할을 먼저 물어볼 것:

> "당신의 역할(룰)이 무엇인가요? (기획자 / 개발자 / 디자이너 / 회의 주관자)"

역할을 확인하지 않고 작업을 진행하는 것은 **프로젝트 이용 가이드 위배**입니다.

---

## 역할별 가이드

역할이 확인되면 해당 역할의 룰에 맞게 프로젝트를 이용하도록 가이드할 것.

| 역할 | 참조 문서 | 할 수 있는 것 | 할 수 없는 것 |
|------|----------|-------------|-------------|
| **기획자** | `guides/role-rules.md`, `guides/ai-pipeline-guide.md` | Requirement 작성, 블로커 확인/해결, ADR 작성 판단 | PRD 직접 수정 (작업자 영역) |
| **개발자** | `guides/role-rules.md`, `guides/ai-pipeline-guide.md`, `guides/sprint-lifecycle.md` | PRD 작성/수정, 블로커 작성, 코드 구현 | 다른 작업자의 PRD 참조하여 자기 PRD 작성 |
| **디자이너** | `guides/role-rules.md`, `guides/ai-pipeline-guide.md` | 디자인 관점 PRD 작성, 디자인 이슈 판단 | 기획/개발 관점 PRD 수정 |
| **회의 주관자** | `guides/prd-merge-process.md`, `guides/ai-pipeline-guide.md` | PRD 통합, 리뷰 회의 진행, 차이점 정리 | 독단적 결정 (회의에서 전원 합의) |

---

## 핵심 규칙

1. **역할 미확인 시 작업 진행 금지** — 역할에 따라 접근 가능한 범위와 책임이 다름
2. **PRD 버전 관리** — `{도메인}/v1/`, `{도메인}/v2/` 폴더 구조 준수
3. **독립성 원칙** — 개별 PRD 작성 시 다른 작업자의 PRD를 참조하지 않음 (기획 문서만 참조)
4. **가이드 문서 숙지** — 작업 전 `guides/` 폴더의 해당 역할 문서를 반드시 읽을 것

---

## 참조 문서

| 문서 | 읽어야 할 때 |
|------|-------------|
| `guides/ai-pipeline-guide.md` | PRD 작성/통합 프로세스, v1/v2 사이클 이해 |
| `guides/prd-merge-process.md` | 개별 PRD → 통합 PRD 병합 절차 |
| `guides/sprint-lifecycle.md` | 워킹데이 → 블로커 → 다음 사이클 흐름 |
| `guides/role-rules.md` | 역할별 책임과 참조 문서 |
