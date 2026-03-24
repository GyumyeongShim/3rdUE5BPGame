[MasterPlan_V2] 3D Survivor x Grid Synergy
"Gridventure"
0. 핵심 컨셉 (Core Concept)
장르: 3D 액션 서바이벌 (Survivor-like)

참고 게임:
Deep Rock Galactic: 3D 공간감, 전투 스타일, 타격감.
Backpack Battles / Hero: 그리드 기반 아이템 배치 및 실시간 시너지 전략.
핵심 루프: 아이템 획득 → 그리드 최적 배치 → 스탯 실시간 강화 → 자동/수동 화력 투사.

조건 : UE5 BluePrint로만 구현한다.
조건 : Phase 순서에 맞게 구현한다.
조건 : Phase 하위 목록을 전부 구현한다.
조건 : 실제 노드 작성 및 구조를 작성할 수 있게 각 항목에 대해 자세한 설명을 한다.
조건 : 작성자가 진행 확인을 할 경우 스샷을 공유를 우선한다.

Phase 1. 🟢 Player (플레이어)
-카메라 : Deep Rock Galactic와 유사하게 구현
-스탯 실시간 연동: FinalStatData(8종 스탯)를 이동 속도, 공격력, 방어력 등에 상시 반영.
-이동 (Locomotion): Enhanced Input 기반 8방향 이동 및 애니메이션 블렌딩.
-장착 무기 공격 (Active): 마우스 좌클릭 기반 수동 공격 (높은 데미지 계수).
-인벤토리 자동 공격 (Passive): 그리드 내 무기 아이템의 FireRate에 따라 주변 적을 자동 추적/발사.
-피격 처리: Def 스탯 적용 데미지 계산 및 피격 무적 시간(I-frame) 구현.
-성장 시스템: 경험치 획득에 따른 레벨업 트리거 및 스탯/아이템 선택 로직.

Phase 2. 🔴 Monster (몬스터)
-집단 AI 추적: 플레이어를 향한 최단 거리 추적 및 군집 이동(Swarming).
-웨이브 시스템: 시간에 따른 난이도 곡선 및 특수 몬스터 스폰 매니저.
-피격/사망 리액션: 타격 경직(Stun), 넉백, 사망 시 경험치 구슬 및 재화 드롭.
-몬스터 분화: 근접 돌진형, 원거리 저격형, 자폭형 등 상속 구조 설계.

Phase 3. 🟡 Item & Inventory (아이템/그리드)
-그리드 배치 시스템: 아이템 드래그/드롭, 회전, 칸 차지 여부 판정 로직.
-자동 공격 변수 확장: 아이템 구조체에 ProjectileClass, FireRate, Range 추가.
-실시간 시너지 엔진: 배치 변경 즉시 RefreshFinalStat 호출 → 스탯 재계산.
-확률형 상점: 레벨업 시 등급 확률에 따른 무작위 아이템 3~4종 제안 UI.

Phase 4. 🔵 UI (인터페이스)
-동적 스탯 패널: 보너스 발생 시 초록색 강조 연출 (WBP_StatusPanel).
-인게임 HUD: HP Bar, EXP 게이지, 현재 시간 및 킬 카운트 표시.
-툴팁 시스템: 아이템 오버 시 시너지 효과 및 상세 스탯 팝업.
-선택 UI: 레벨업 시 카드 형태의 아이템 선택 인터페이스.

Phase 5. 🟣 Effect & Juice (연출/효과)
-전투 연출: 투사체 파티클, 타격 이펙트, 사운드 피드백.
-데미지 텍스트: 적 타격 시 Atk 수치 기반 데미지 숫자 플로팅.
-성장 연출: 레벨업 및 아이템 획득 시 시각적 강조 효과.

🛠️ 현재 구현 상태 (Current Progress)
Backend: 8종 스탯 계산기, 태그 기반 시너지 합산 로직 완료.
Frontend: 스탯 변화 실시간 UI 연동 및 보너스 수치 시각화(초록색 텍스트) 완료.
Optimization: Make Array 및 For Each Loop를 활용한 데이터 바인딩 최적화 완료.