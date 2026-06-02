# Beamo Workspace — Claude Work Instructions

## What This Repo Is

This is the central PM workspace for Beamo product documentation.
It is used to author, maintain, and deliver product requirements, UX flows, and user manuals to engineering, design, QA, sales, and CS.

---

## Workspace Structure

```
claude-workspace/
└── Beamo/
    ├── _shared/                   ← Shared product context (read first, always)
    ├── Beamo_FeatureRequirement/  ← PRDs, initiative docs, sprint plans
    ├── Beamo_UXFlowChart/         ← UX flow diagrams and references
    └── beamo_user_manual/         ← End-user manual content
```

---

## Read These at Session Start

Before any work, read the shared context files:

- `Beamo/_shared/product_overview.md` — company, product, customers, markets
- `Beamo/_shared/data_model.md` — Space → Site → Plan → Survey → Point / Tag / Comment / Beamo Box
- `Beamo/_shared/glossary.md` — all key terms
- `Beamo/_shared/roles.md` — user roles and permissions
- `Beamo/_shared/feature_status.md` — live / upcoming / future features
- `Beamo/_shared/capture_hardware.md` — 360° camera hardware, connection model, capture flows
- `Beamo/_shared/api_reference.md` — live API endpoint list, how to extract the full OpenAPI spec via curl
- `Beamo/_shared/design_map.md` — all Figma frames indexed by feature area with node IDs and direct links

---

## Subproject Instructions

Each subproject has its own `CLAUDE.md` with detailed work instructions:

- `Beamo/Beamo_FeatureRequirement/CLAUDE.md` — PRD authoring workflow, document formats, sprint cadence
- `Beamo/Beamo_UXFlowChart/CLAUDE.md` — UX flow conventions
- `Beamo/beamo_user_manual/CLAUDE.md` — manual structure and authoring rules

When working within a specific subproject, follow that subproject's `CLAUDE.md`.

---

## Decision Routing — When Something Is Unclear

First: check the spec and shared context files. The answer is often already there.

If still unclear, route by type:

**1. Check spec first**
Read the relevant story, shared context, and resolved Open Questions before escalating. Most questions are already answered.

**2. Low-risk, low-complexity, just more work**
If covering it adds value or consistency with no meaningful tech debt — cover it. Note the decision in your commit message.

**3. Engineering implementation judgment** ← own this
Performance, architecture, thresholds, approach — decide and proceed. No PM confirmation needed.

**4. Genuine product/UX intent unclear**
If the spec and context don't give enough signal on what the user experience should be — ask PM.

**5. Design-centric**
Mark the item as `[design]` and flag it for design review. Do not decide independently.

---

## Context Files (FeatureRequirement)

`Beamo/Beamo_FeatureRequirement/context/` 폴더에 AI 파이프라인 및 개발 가이드 문서가 있다.
작업 전 역할에 맞는 문서를 참조할 것.

| 문서 | 내용 |
|------|------|
| `ai-pipeline-guide.md` | AI 기반 협업 파이프라인 전체 흐름 |
| `sprint-lifecycle.md` | 스프린트 사이클 라이프사이클 상세 |
| `prd-merge-process.md` | PRD 통합 프로세스 (AI 병합 절차) |
| `blocker-guide.md` | 블로커 가이드 (작성/삭제 프로세스, 이슈 분류, 커밋 정책) |
| `adr-writing-guide.md` | ADR 작성 가이드 (의사결정 기록, 기획자가 판단 시 작성) |
| `design-how-to-check.md` | Figma 디자인 스펙 확인 방법 |
| `dev-ai-tips.md` | 개발 AI 에이전트 활용 팁 (API, 디자인, PRD 작성) |

---

## First Session — Role Check

첫 세션을 열면 사용자에게 역할을 확인한다:

> "어떤 역할로 작업하시나요? (기획자 / 개발자 / 디자이너 / 회의 주관자)"
>
> 매번 묻는 것이 번거로우시면 메모리에 저장해 드릴 수 있습니다.

역할에 따라 참조할 문서와 작업 흐름이 달라진다:

| 역할 | 주요 참조 | 작업 흐름 |
|------|----------|----------|
| 기획자 | `_shared/`, requirement 폴더, `context/blocker-guide.md`, `context/adr-writing-guide.md` | Requirement 작성 → 블로커 확인·해결 → ADR 작성 판단 → 리뷰 핸드오프 |
| 개발자 | `context/dev-ai-tips.md`, `_shared/api_reference.md`, `_shared/design_map.md` | PRD 작성 → AI 기반 구현 → 블로커/ADR |
| 디자이너 | `context/design-how-to-check.md`, `_shared/design_map.md` | 디자인 관점 PRD 작성 |
| 회의 주관자 | `context/prd-merge-process.md` | PRD 통합 → 리뷰 회의 진행 |

---

## Owner

Product Manager: Taeyeong Moon (taeyeong.moon@3i.ai)
