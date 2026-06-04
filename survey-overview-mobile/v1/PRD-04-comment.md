# PRD-04: 코멘트 💬

> 기획 스펙: SO-07 (Comment Sheet — List View), SO-08 (Comment Sheet — Detail View)
> 역할별 CRUD 권한: `_shared/roles.md` 참조 — Edit/Delete는 본인 작성분만 (역할 무관)

---

## 한줄 요약

> 스레드 기반 코멘트 시스템. 카테고리·상태(All/Unresolved/Resolved) 필터 리스트 + 스레드 디테일(답글·미디어·해결 처리). @멘션 지원.

**기존 활용**: `SurveyOverviewCommentContent`, `SurveyOverviewCommentDetailView`, `SurveyOverviewCommentDelegate`, `MentionUseCase` + `MentionTextField`, 상태 필터 `DropdownMenu` 동작 중.

---

## 📋 리스트 뷰 (SO-07)

### 헤더
- "Comment" 레이블 + ➕ 추가 버튼 (온라인/오프라인 모두 활성)
- ➕ 탭 → 코멘트 생성: ① 도면에서 위치 지정 ② 전체 페이지 입력
- 📌 **카테고리 선택은 선택사항** — 카테고리 없이 생성 가능

### 카테고리 필터 바
- 태그와 동일한 가로 스크롤 칩 · 동일 카테고리 데이터셋
- 기본: 필터 없음

### 카운트 + 상태 필터
- `"{N} listings"` — 현재 뷰 코멘트 수
- 상태 필터 (DropdownMenu 3옵션):

| 옵션 | 표시 |
|------|------|
| **All** | 전체 |
| **Unresolved** | 미해결만 |
| **Resolved** | 해결됨만 |

### 코멘트 아이템

| 구성요소 | 내용 |
|---------|------|
| 헤드 행 | 프로필 아이콘 · 전체 이름 · 날짜 |
| 설명 | 코멘트 본문 (긴 경우 잘라냄) |
| 미디어 | 첨부 미디어 썸네일 그리드 |
| 카테고리 | 카테고리 레이블 |
| Thread 버튼 | 탭 → 디테일 뷰 |
| Resolve 버튼 | 탭 → 해결됨 표시 |

> 📌 **코멘트 아이템 전체 영역 탭** → 디테일 뷰 진입 (Thread 버튼뿐 아니라 아이템 어디든 탭) — ⚠️ 현재 구현은 Thread pill 버튼만 탭 가능, 전체 영역 탭 미구현

> 📅 **날짜 형식**: 오늘 → `Today HH:mm` / 7일 이내 → `요일 HH:mm` / 7일 초과 → `YYYY-MM-DD`

> 🔐 **Resolve 권한**: Collaborator, Surveyor, Team Admin, Site Manager, Super Admin (**Viewer 불가**)

### 위치 없는 코멘트 (블로커 해소)
- 모바일 P1 생성 시 위치 필수 / 웹 생성은 위치 없을 수 있음
- 📌 **정책**: 리스트에 정상 표시 · 캔버스 마커 없음 · 포커스 조용히 건너뜀
- ⚠️ `(0,0,0)`은 유효 좌표 → 위치 없음은 **null 또는 별도 플래그** 사용

### 코멘트 삭제 처리 순서
- **태그와 동일 플로우** (PRD-03 참조): 온라인 클라우드 → 서버 삭제 후 로컬 삭제 / 오프라인 로컬 → 즉시 로컬 삭제

### 오프라인
- 오프라인 시 **클라우드 코멘트 리스트 미표시** → 네트워크 없음 일러스트로 대체
- ➕ 활성 유지 → 오프라인 생성 → 업로드 큐
- 로컬 생성(큐) 아이템은 오프라인에서도 리스트에 표시
- 업로드 큐 상태는 **태그와 동일** (PRD-03 참조): Pending → Uploading → Failed → Succeeded

---

## 🔍 디테일 뷰 (SO-08)

### 진입점
- 코멘트 리스트에서 아이템/Thread 버튼 탭
- 캔버스에서 코멘트 아이콘 탭

### 레이아웃
- 리스트 + 피처 바 **오버레이** · 열린 시점 시트 크기

### 헤더 (sticky)
- 프로필 아이콘 + 전체 이름 + 날짜
- 점 세 개 메뉴: **Edit** / **Delete** → 삭제 시 리스트 복귀
- 📌 **본인이 작성한 코멘트에만 표시** (역할 무관). 스레드 답글도 동일 — 본인 답글만 Edit/Delete 가능
- 📌 **Edit 시 위치(도면 좌표) 변경 불가** — 콘텐츠(텍스트, 미디어, 카테고리)만 수정 가능
- ✕ 닫기 → 리스트 복귀 + 캔버스 아이콘 idle

### 본문

| 섹션 | 내용 |
|------|------|
| **설명** | 전체 코멘트 본문 (스크롤 가능) · HTML → `displayContent` plain text 변환 (멘션 span 추출 → `<p>` strip → `</p>`→`\n` → 잔여 태그 strip → 엔티티 unescape) |
| **미디어** | 첨부 썸네일 (aspect-fill) · 탭 → 전체 뷰어 · 📌 **최대 5장** |
| **카테고리** | 할당된 카테고리 레이블 |

> ⚠️ **서버 데이터 모델 주의**: 코멘트 첨부파일은 서버에서 `isMedia=false`로 내려옴 (서버가 코멘트 첨부를 파일로 취급). 미디어 표시 여부는 `isMedia` 필드가 아닌 **`attachType`** 필드(`"image"`, `"video"`)로 판단해야 함. 로컬 pending 아이템은 `attachType="media"`로 저장됨.

### 스레드 섹션

- **`"{N} Threads"`** — 자식 답글만 (루트 미포함)
- **Resolve 버튼** — Viewer 제외 모든 역할
- 스레드 아이템 (시간순 ↑):
  - 프로필 + 이름 + 날짜 + 점 세 개 (Edit/Delete)
  - 답글 텍스트 + 미디어 썸네일 스트립 · 📌 **최대 5장**

### 스레드 입력 바

| 버튼 | 동작 |
|------|------|
| 🖼️ 갤러리 | 디바이스 갤러리 → 미디어 선택 → 첨부 |
| ✏️ 텍스트 | 자유 입력 |
| ➤ Send | 콘텐츠 있을 때만 표시 → 답글 게시 → 입력 초기화 |

- 스레드와 함께 스크롤 (웹 벤치마킹 기반 변경 — spec-review #20)
- ✅ `LazyColumn` 내부 마지막 아이템으로 배치 — `Column > LazyColumn(weight(1f), imePadding) { ...threads, InputBar }` 구조

### 오프라인
- 캐시된 디테일 → 표시 가능 / 캐시 없으면 → 네트워크 없음
- 오프라인 스레드 답글 → 업로드 큐 + 낙관적 즉시 표시
- 클라우드 코멘트: 오프라인 시 리스트 미표시
- 로컬 큐된 코멘트: Edit/Delete 가능 (pending) / failed는 Retry/Delete만
- 경쟁 조건: Save/Delete 확인 시점에 현재 상태 확인 후 라우팅

---

## 🔧 기존 코드

| 기능 | 파일 | 상태 |
|------|------|------|
| 코멘트 리스트 | `SurveyOverviewCommentViews.kt` (`SurveyOverviewCommentContent` 함수) | ✅ 동작 중 · ⚠️ 리스트 아이템 소유권 미체크 (디테일은 체크 완료) |
| 코멘트 디테일 | `SurveyOverviewCommentDetailView` | ✅ 동작 중 |
| 코멘트 Delegate | `SurveyOverviewCommentDelegate` | ✅ 동작 중 |
| @멘션 입력 | `MentionUseCase` + `MentionTextField` | ✅ 동작 중 — 스레드 답글 입력에 완전 연동됨 (SO-08은 Out of Scope로 표기하나 구현 완료) |
| 상태 필터 | DropdownMenu 3옵션 | ✅ 동작 중 |
| HTML→plain text | `CommentLocalDNM.displayContent` | ✅ 동작 중 |
| 답글 큐 상태 | 아이콘 기반 (Pending/Progress/Failed) | ✅ 동작 중 |
| 카테고리 칩 | `OverviewCategoryChipRow` | ✅ 공유 |
| Edit/Delete 소유권 체크 | `CommentDetailNavigationBar`, `ThreadReplyItem` | ✅ 구현 완료: `currentUserId` 비교, 본인 코멘트만 Edit/Delete 메뉴 표시 |

---

## ❓ Open Questions

| # | 질문 | 상태 |
|---|------|------|
| 1 | Resolve 권한: 자격 역할 누구나 (Viewer 제외). **되돌리기(Unresolve) 가능 여부?** | 🟡 Edit/Delete 소유권 해소, Unresolve는 BE 확인 중 (spec-review #15) |
| 2 | 부모 삭제 시 플레이스홀더? 자식 삭제 시 완전 제거? | 🔴 BE 확인 |
| 3 | `GET /v2/com/comments/list`에 `parentId` 누락 — 답글이 루트로 나타남 | 🔴 BLK-10 |
| 4 | ~~스레드 답글당 최대 미디어 첨부 수~~ | ✅ 코멘트·스레드 답글 각각 미디어 최대 5장 |
| 5 | ~~Resolve 낙관적 업데이트 vs 서버 확인~~ | ✅ 낙관적 업데이트 + 실패 시 롤백 (구현 완료) |
