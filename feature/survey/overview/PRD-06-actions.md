# PRD-06: 액션 버튼 🎯

> 기획 스펙: SO-11 (Start Survey FAB), SO-12 (3DW Button)

---

## 한줄 요약

> 캔버스 오버레이 버튼 2개. **Start Survey FAB** (우하단, 포인트 캡처) + **3DW 버튼** (우상단, 모바일 3DWorkspace).

---

## 🟢 Part A: Start Survey FAB (SO-11)

### 위치·표시
- 캔버스 **우하단 모서리** (Site Detail / Plan Detail FAB과 동일 위치)
- min/mid 시트에서 보임
- full 시트에서는 명시적으로 숨김 (`isSheetFull` 조건 토글)

### 탭 동작
- 탭 → **포인트 캡처 플로우** 시작
- 기존: `GoToSurveyContinueCreate(siteId, floorId, surveyId, surveyName, isMasterScale)` Effect

### 오프라인 동작
- ⚡ **연결 상태 무관하게 항상 보임** (Surveyor+ 역할 전용)
- 포인트 캡처는 오프라인 생성 지원 → 로컬 큐 후 연결 복구 시 업로드
- 📌 오프라인 시 서베이 컨테이너 자체도 큐잉 → 연결 복구 시 서버 생성 후 포인트 업로드 진행 (`_shared/product_decisions.md` "Survey as Standalone Container" 참조)

### 디자인
- 기존 FAB 디자인 따름 (아이콘, 크기, 고도, 모양)
- 모든 캔버스 콘텐츠 위에 렌더링
- 특화 숨김/표시 애니메이션 불필요

### FAB vs ➕ 버튼 구별

| 요소 | 역할 | 위치 |
|------|------|------|
| **FAB** | 🎯 포인트 캡처 세션 시작 (화면 레벨) | 캔버스 우하단 |
| **➕ 버튼** | ✏️ 활성 탭 콘텐츠 생성 (태그/코멘트/Box) | 시트 헤더 |

---

## 🔵 Part B: 3DW 버튼 (SO-12)

### 위치·표시
- 캔버스 **우상단 모서리** 오버레이, 모든 캔버스 아이콘 위
- "3D" 레이블의 **원형 버튼**

### 표시 조건 (두 가지 모두 충족)

| 조건 | 설명 |
|------|------|
| ✅ 디바이스 온라인 | — |
| ✅ 포털 데이터(`isPortalDataYN`) 있는 스윕 ≥ 1 | 서버 업로드 완료된 스윕 기준 |

> 조건 미충족 → **완전히 숨김** (회색 처리 아님, 빈 공간 없음)

### 탭 동작
- 탭 → **모바일 3DWorkspace 인앱 브라우저** 열기
- 기존: `GoToSurveyViewer(siteId, planId, surveyId)` Effect
- 🟡 Nice-to-have: 캔버스에서 선택된 포인트로 3DW 열기 (웹 3DW URL 파라미터 필요)

---

## 🔧 기존 코드

| 기능 | 파일 | 상태 |
|------|------|------|
| FAB 렌더링 | `SurveyOverviewScreen` 우하단 오버레이 | ✅ 동작 중 |
| 캡처 진입 | `GoToSurveyContinueCreate` Effect | ✅ 동작 중 |
| 3DW 버튼 | `SurveyOverviewScreen` 우상단 오버레이 | ✅ 구현 완료 (`isPortalDataYN + isOnline` 조건) |
| 3DW 진입 | `GoToSurveyViewer` Effect | ✅ 동작 중 |
| 역할 기반 표시 | `UserRoleAccessActionUseCase` (`CreateAndEditSurvey` = Surveyor 이상) | ✅ 동작 중 — 확장 불필요 |

---

## ❓ Open Questions

| # | 질문 | 상태 |
|---|------|------|
| 1 | FAB과 3DW 버튼 Z-order 겹침 여부 | 🟡 물리적 겹침 없을 것 |
| 2 | 3DW URL에 포인트 ID 파라미터 지원 여부 | 🔴 FE 확인 |
| 3 | "위치 데이터가 있는 스윕" 판별 — 클라이언트 로컬 데이터 기반 판단 제안 | ✅ spec-review #18 Resolved — 기준 확정, 구현 방법 엔지니어 위임 |
