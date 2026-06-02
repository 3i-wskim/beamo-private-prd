# PRD 개발 질문 목록

> 기획자 피드백 불필요, 개발팀 내부에서 논의·결정할 항목.
>
> 결정 완료 (PRD 반영):
> - ~~아이콘 겹침 탭 우선순위~~ → PRD-01: 최신 생성순 Z-order
> - ~~오프라인 배너 복구 타이밍~~ → PRD-07: 네트워크 복구 즉시 사라짐 (현행)
> - ~~업로드 큐 최대 크기~~ → PRD-07: 제한 없음, FIFO 순차 처리
> - ~~앱 삭제 시 큐~~ → PRD-07: 로컬 DB 삭제 = 큐 소실 (현행)
> - ~~앱 재시작 후 큐 재개~~ → PRD-07: 자동 재개 (현행)
> - ~~태그 삭제 처리 순서~~ → PRD-03: 직렬(서버→로컬) 확정, 코멘트도 동일 플로우
> - ~~Box 삭제 컨펌~~ → PRD-05: 기존 BeamoBox 코드 따름
> - ~~시트·캔버스 터치 분리~~ → 현행 NestedScrollConnection 방식으로 처리
> - ~~3DW 인앱 브라우저 복귀 상태 유지~~ → 현행 유지
> - ~~3DW 버튼 활성화 조건~~ → spec-review #18로 이관 (클라이언트 데이터 기반 판단 제안)
> - ~~Custom Order API 필드~~ → spec-review #19로 이관 (서버 개발자 확인 필요)
> - ~~D-01 온라인→오프라인 아이콘 전환~~ → PRD-01 반영: `remapCanvasIconVisualStates()` 실시간 즉시 전환
> - ~~D-06 줌 min/max~~ → PRD-01 반영: min=fitZoomLevel(≥0.1f), max=10.0f
> - ~~D-07 500+ 포인트 culling~~ → PRD-01 반영: 뷰포트 컬링 구현됨, 클러스터링 없음
> - ~~D-08 그레이스케일 방식~~ → PRD-01 반영: WHITEOpacity50 오버레이 (ColorMatrix 미사용)
> - ~~D-09 Mid 높이~~ → PRD-02 반영: 이미 50% (`normalFraction = 0.5f`)
> - ~~D-10 scroll-to-resize 상단 충돌~~ → PRD-02 반영: Full 상태에서 위 스크롤 시 변화 없음
> - ~~D-11 디테일 닫기 시 시트 복원~~ → PRD-02 반영: 복원 없음, 현재 크기 유지
> - ~~D-12 인라인 폴더 IME~~ → PRD-02 반영: BeamoDraggableBottomSheet IME insets 미처리 (실기기 테스트 필요)
> - ~~D-14 카테고리 오프라인 캐시~~ → PRD-03 반영: Room DB 기반, 오프라인에서도 캐시 있으면 표시
> - ~~D-15 HTML→plain text~~ → PRD-04 반영: 현행 strip 파이프라인 충분
> - ~~D-16 입력 바 sticky~~ → PRD-04 반영: 현행 LazyColumn 내 배치 (구현 필요)
> - ~~D-17 Resolve 낙관적 업데이트~~ → PRD-04 반영: 낙관적 + 실패 롤백 (구현 완료)
> - ~~D-18 카테고리 필터 공유 범위~~ → PRD-05 반영: SurveyOverview 전용, 외부 서피스 미사용
> - ~~D-19 체크리스트 재사용~~ → PRD-05 반영: 아이템 composable 재사용 OK, Content 래퍼 파라미터화 필요
> - ~~D-20 FAB 역할~~ → PRD-06 반영: `CreateAndEditSurvey` = Surveyor 이상 (구현 완료)
> - ~~D-21 오프라인 배너~~ → PRD-07 반영: BaseScreen opt-in, SurveyOverview 적용 완료
> - ~~D-22 큐 타입~~ → PRD-07 반영: 8개 타입 통합 큐
> - ~~D-23 서베이 컨테이너 큐잉~~ → PRD-07 반영: UnifiedFlowWorkerQueue 확장 + waitForQueueCompletion 패턴, BE API 확정 후 구현
> - ~~D-24 서베이 포인트 UI~~ → PRD-07 반영: QueueStatusBar만으로 충분
> - ~~D-25 코멘트 소유권~~ → PRD-04 반영: `comment.userId` vs 현재 사용자 비교 (구현 필요)
> - ~~D-13 폴더 삭제 cascade~~ → PRD-03 반영: 자식 삭제 안 함, 폴더 ID 제거하여 루트로 이동
>
> **모든 개발 질문 해소 완료.**
