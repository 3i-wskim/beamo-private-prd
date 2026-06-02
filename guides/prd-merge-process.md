# PRD Merge Process — PRD 통합 프로세스

> [ai-pipeline-guide.md](./ai-pipeline-guide.md) §2 프로젝트 설계의 상세 문서

---

## 개요

여러 작업자가 독립적으로 AI를 활용해 작성한 PRD를 하나의 통합 PRD로 병합하는 프로세스.

---

## 전체 흐름

```
개별 PRD 수집 (prd-{작업자명}/ 폴더들)
     │
     ▼
AI로 공통점 추출
     │
     ▼
AI로 차이점 식별
     │
     ▼
차이점 분류 + 논의 안건 정리
     │
     ▼
설계 리뷰 회의
     │
     ▼
결정 사항 반영
     │
     ▼
통합 PRD 생성 (prd/ 폴더)
     │
     ▼
PRD v1 태그
```

---

## 단계별 상세

### 1. 개별 PRD 수집

각 작업자의 `prd-{작업자명}/` 폴더에서 PRD를 수집한다.

```
initiatives/{slug}/
├── prd-wskim/          ← 개발자 A
│   ├── PRD-01-canvas.md
│   └── PRD-02-sheet.md
├── prd-parkjh/         ← 개발자 B
│   ├── PRD-01-canvas.md
│   └── PRD-02-sheet.md
└── prd-designer-lee/   ← 디자이너
    └── PRD-01-canvas.md
```

### 2. AI 병합 — 공통점 추출

- 모든 PRD에서 **동일하게 해석한 requirement**를 추출
- 이 부분은 논의 없이 통합 PRD에 바로 반영

### 3. AI 병합 — 차이점 식별

- 작업자 간 **해석이 다른 부분**을 식별
- 차이점을 아래 형식으로 정리:

```markdown
### 차이점 #1: {주제}

| 작업자 | 해석 |
|---|---|
| wskim | (내용) |
| parkjh | (내용) |

**질문**: (어떻게 처리할지에 대한 질문)
```

### 4. 설계 리뷰 회의

- 정리된 차이점을 하나씩 논의
- 전원이 있는 자리에서 결정
- AI가 실시간으로 결정 사항을 기록

### 5. 통합 PRD 생성

```
[입력]                      [처리]                  [출력]
prd-wskim/      ──┐
prd-parkjh/     ──┼──▶  AI 병합 + 회의 결정 반영 ──▶  prd/ (통합 PRD)
prd-designer/   ──┘
```

- 통합 PRD는 `prd/` 폴더에 저장 (기존 구조와 동일한 위치)
- 개별 `prd-{작업자명}/` 폴더는 통합 완료 후 **삭제** (임시 폴더이므로 이력은 git에 남음)

### 6. 태그 생성

```bash
git tag sprint-{slug}-v1
git push origin sprint-{slug}-v1
```

---

## 핵심 원칙

### 독립성 원칙

```
기획자 Requirement (단일 소스)
     │
     ├──▶ 작업자 A의 AI ──▶ PRD-A
     │                         ╳ (참조 금지)
     ├──▶ 작업자 B의 AI ──▶ PRD-B
     │                         ╳ (참조 금지)
     └──▶ 작업자 C의 AI ──▶ PRD-C
```

> 각 작업자의 AI는 **기획 문서(requirement)만** 참조한다.
> 다른 작업자의 PRD를 보고 자신의 PRD를 수정하면 안 된다.
> 이것은 독립적 해석을 보장하고, 설계 리뷰에서 다양한 관점을 모으기 위함이다.

### 통합 후 버전 관리

- 통합 PRD가 확정되면 **v1**
- 이후 사이클에서 갱신될 때마다 버전 증가
- 각 버전은 git tag로 마킹

---

## Requirement도 함께 갱신

PRD 통합 과정에서 requirement 문서(SO-XX)에 **누락이나 모호함**이 발견되면:

1. 회의에서 결정
2. AI로 requirement 문서도 함께 수정
3. 수정 이력을 requirement의 Version History에 기록
