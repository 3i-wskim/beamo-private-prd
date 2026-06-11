# PRD-05: BeamoBox + 체크리스트 📦

> 기획 스펙: SO-09 (BeamoBox Sheet), SO-10 (Checklist Sheet)
> 역할별 CRUD 권한: `_shared/roles.md` 참조

---

## 한줄 요약

> 기존 BeamoBox·체크리스트 구현을 시트 스캐폴드에 마운트. 새 비즈니스 로직 없이 SurveyOverview 컨텍스트에 노출. Box에 **카테고리 필터 바** 추가.

---

## 📦 Part A: BeamoBox (SO-09)

### 시트 렌더링
- 시트 스캐폴드(PRD-02) 내에서 **기존 BeamoBox 그리드 리스트** 렌더링
- 인터랙션·레이아웃 기존 동일

### 헤더
- "Box" 섹션 레이블

### 🆕 카테고리 필터 바
- 가로 스크롤 카테고리 칩 (태그와 동일 동작)
- 칩 탭 → 해당 카테고리 파일만 · 기본: 필터 없음
- 위치: 헤더와 그리드 사이
- 기존: `OverviewCategoryChipRow` 재사용

### 🆕 필터·정렬 UI 통일 (2026-06-11 추가)

> 📌 **오버뷰 공통 정책**: SurveyOverview는 이미 바텀시트 안이므로, 시트 내 필터/정렬 선택 UI는 **바텀시트를 띄우지 않고 드롭다운(액션) 메뉴**를 사용한다 — 태그 정렬·코멘트 상태 필터와 동일 패턴. (다른 화면들은 바텀시트 사용 가능 — 오버뷰 한정 정책)

| 항목 | 기존 | 변경 |
|------|------|------|
| Box 정렬 | 탭마다 Newest↔Oldest 토글 | **드롭다운 메뉴** (Newest first / Oldest first 선택) |
| Box 필터 | 별도 BoxList 화면 이동 | **현행 유지** — 필터가 다중 선택 구조(타입 Photo/Video/Voice/Document + 사용자)라 단일 드롭다운으로 부적합. 드롭다운 전환은 디자인 정리 시 재검토 |
| 체크리스트 필터 (All/Done/Undone) | 시트 내 전용 필터 패널 | **드롭다운 메뉴** (단일 선택 — All 기본) |

- 디자인 디테일은 추후 정리 예정 — 우선 태그/코멘트와 동일한 기존 드롭다운 컴포넌트로 통일

### 적용 범위 (전체 서피스)

> 📌 카테고리 필터는 **공유 컴포넌트 업데이트**로 구현 → 모든 서피스 자동 적용

| 서피스 | 적용 |
|--------|------|
| Survey Overview Box 시트 (이 스토리) | ✅ 필수 |
| Site Box 탭 | ✅ 필수 |
| 360° Video Survey Box | ✅ 필수 |
| Global Box 화면 | ⚠️ API 지원 시 적용, 미지원 시 생략 |

### 스코프
- **서베이 스코프** — 현재 서베이 연관 파일만 (사이트 레벨 파일 미표시)

### 기존 기능 유지
- 파일 업로드(생성) + 삭제 — 기존 BeamoBox 구현 따름 (삭제 시 컨펌 다이얼로그 포함)

### 오프라인
- 오프라인 시 **클라우드 파일 리스트 미표시** → 네트워크 없음 일러스트로 대체
- 파일 업로드(생성) → 오프라인 큐
- 로컬 생성(큐) 아이템은 오프라인에서도 리스트에 표시
- 업로드 큐 상태는 **태그와 동일** (PRD-03 참조): Pending → Uploading → Failed → Succeeded

---

## ✅ Part B: 체크리스트 (SO-10)

### 시트 렌더링
- 시트 스캐폴드 내에서 **기존 체크리스트 기능** (Site Detail → Checklist 탭) 렌더링
- 🚫 새 체크리스트 특화 기능 없음

### 헤더
- "Checklist" 섹션 레이블

### 기존 동작 보존
- 모든 인터랙션·상태·동작 기존 그대로
- 시트 스캐폴드 적용 (크기 상태, 드래거, scroll-to-resize)
- 시트 높이 내 수직 스크롤

### 오프라인
- 📌 **온라인 전용** — 로컬 캐싱 없음
- 오프라인 시 탭은 표시되나 **no-internet empty state** 렌더링
- 오프라인에서 체크리스트 생성/수정/조회 불가

---

## 🔧 기존 코드

| 기능 | 파일 | 상태 |
|------|------|------|
| Box 콘텐츠 | `SurveyOverviewBoxViews.kt` (`SurveyOverviewBoxContent` 함수) | ✅ + 카테고리 필터 추가 |
| Box Delegate | `SurveyOverviewBoxDelegate` | ✅ 동작 중 |
| 체크리스트 콘텐츠 | `SurveyOverviewChecklistViews.kt` (`SurveyOverviewChecklistContent` 함수) | ✅ 동작 중 |
| 체크리스트 Delegate | `SurveyOverviewChecklistDelegate` | ✅ 동작 중 |
| 체크리스트 추가 | `GoToChecklistPullTemplate` 네비게이션 → 별도 페이지 이동 | ✅ 동작 중 |
| 카테고리 칩 | `OverviewCategoryChipRow` | ✅ 공유 |

---

## ❓ Open Questions

| # | 질문 | 상태 |
|---|------|------|
| 1 | Global Box API가 카테고리 필터링 지원하는지 | 🔴 미지원 시 생략 |
| 2 | ~~공유 컴포넌트 업데이트가 기존 서피스에 회귀 없이 안전한지~~ | ✅ `OverviewCategoryChipRow`는 SurveyOverview 전용 — Site Box/Global Box 미사용, 회귀 없음 |
| 3 | 체크리스트 상세 스펙 문서화 미완 | 🔴 별도 스펙 리뷰 필요 |
| 4 | ~~체크리스트 composable 재사용 범위~~ | ✅ 아이템 composable(`ChecklistListItem` 등) 재사용 완료, `SurveyOverviewContract.State?` 파라미터화 적용됨 |
