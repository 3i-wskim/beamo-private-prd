# Beamo Private PRD

Engineer-owned PRD (Product Requirement Document) repository for Beamo Android.

## Structure

```
beamo-private-prd/
├── README.md
├── guides/                         ← PRD 프로세스 가이드
│   ├── ai-pipeline-guide.md        ← AI 기반 협업 파이프라인 (v1/v2 사이클)
│   ├── prd-merge-process.md        ← PRD 통합 프로세스 (개별→병합→확정)
│   ├── sprint-lifecycle.md         ← 스프린트 라이프사이클 (워킹데이→블로커→다음사이클)
│   └── role-rules.md               ← 역할별 룰 (기획자/개발자/디자이너/회의주관자)
└── survey-overview-mobile/          ← 도메인별 PRD
    └── v1/                          ← 버전별 폴더
        ├── PRD-01-canvas.md
        ├── PRD-02-sheet-scaffold.md
        ├── PRD-03-tag.md
        ├── PRD-04-comment.md
        ├── PRD-05-box-checklist.md
        ├── PRD-06-actions.md
        ├── PRD-07-offline.md
        ├── PRD-QnA.md
        └── PRD-QnA-dev.md
```

## Conventions

- 도메인명 기반 폴더: `{feature}-{platform}/`
- PM 스펙(SO-*.md)을 엔지니어 관점에서 재구성한 작업 단위 문서
- 기획 문서 원본은 `beamo-workspace` 서브모듈 참조

## PRD Versioning (v1/v2)

- **v1**: 각 작업자가 독립적으로 AI로 PRD 작성 → 통합 리뷰 회의 → 확정
- **v2+**: 워킹데이 중 블로커 발생 → 논의/반영 → PRD 갱신
- 상세: `guides/ai-pipeline-guide.md` 참조
