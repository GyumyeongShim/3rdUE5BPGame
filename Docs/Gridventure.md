🧩 Gridventure 통합 마스터 플랜 (BP Only)
0. 🎯 핵심 컨셉
장르: 3D 서바이벌 + 그리드 인벤토리 전략

핵심 루프:

전투 → 아이템 획득 → 그리드 배치 → 스탯 강화 → 더 강한 전투

확장 루프:

전투 → 자원 획득 → 마을 성장 → 영구 스탯 강화 → 다음 런 강화
🏗️ 1. 전체 시스템 구조 (System Map)
1.1 Core Engine (모든 Phase의 기반)
Stat System (8종 스탯)
Damage Logic (로그 스케일)
Status Effect (버프/디버프)
Leveling (EXP → Level)

👉 구현 결과:

BPC_StatComponent
BPL_BattleCalculator
BPC_StatusComponent
1.2 Field System (실시간 전투)
Movement / Locomotion
Wave Spawn
Projectile System
Drop / Pooling

👉 “서바이벌 게임의 코어 루프 담당”

1.3 Inventory System (핵심 차별점)
Grid 기반 배치
아이템 회전
시너지 (인접 기반)
장비 효과 적용

👉 “이 게임의 핵심 재미”

1.4 Town System (메타 성장)
건물 건설
자원 수급
영구 스탯 증가
인벤토리 확장

👉 “런 외부 성장 루프”

1.5 UI System
HUD
인벤토리
툴팁
레벨업 선택 UI
📅 2. 통합 실행 Phase (중요)

👉 기존 두 플랜을 실행 흐름 기준으로 재정렬했다.

Phase 1. 🟢 Foundation (데이터 + 코어)
목표

모든 시스템의 기반 정의

작업
Enum 정의 (EItemType, ECombatEffectType)
Struct 정의
FStatData
FActionData
FStatModifier
DataTable 구축
StatComponent 구현
Damage 공식 BP 함수화

👉 여기까지 하면 “게임 수학 + 데이터 완성”

Phase 2. 🟢 Player (플레이어)
목표

플레이 가능한 캐릭터 완성

작업
카메라 (3인칭)
이동 (Enhanced Input)
스탯 → 실제 능력 반영
수동 공격 (좌클릭)
피격 / 무적 시간
레벨업 트리거

👉 이 시점 = “혼자 돌아다니고 싸울 수 있음”

Phase 3. 🔴 Monster + Field Combat
목표

전투 루프 완성

작업
몬스터 AI (추적 / 군집)
웨이브 시스템
몬스터 타입 분화
사망 → 드랍
Object Pooling

👉 이 시점 = “게임처럼 돌아가기 시작”

Phase 4. 🟡 Inventory (핵심 시스템)
목표

게임의 정체성 구현

작업
그리드 배치
드래그 / 드롭 / 회전
아이템 데이터 연결
자동 공격 시스템
시너지 계산 → StatComponent 반영

👉 핵심:

배치 변경 → RefreshFinalStat → 전투 즉시 반영
Phase 5. 🔵 UI
목표

플레이 피드백 강화

작업
HUD (HP / EXP / 시간)
인벤토리 UI
툴팁
레벨업 선택 UI
스탯 변화 시각화
Phase 6. 🟣 Effect & Juice
목표

“손맛” 만들기

작업
타격 이펙트
데미지 텍스트
레벨업 연출
사운드
Phase 7. 🏠 Town (메타 시스템)
목표

장기 플레이 구조 완성

작업
건설 시스템
자원 시스템
영구 스탯 적용
인벤토리 확장

👉 연결:

Town Bonus → GameInstance → StatComponent
⚙️ 3. 핵심 시스템 연결 구조
🔁 스탯 흐름 (중요)
아이템 배치
→ 시너지 계산
→ StatModifier 생성
→ StatComponent 적용
→ Player 능력 반영
🔁 전투 루프
Attack → DamageCalc → Monster HP 감소
→ 사망 → Drop → Inventory
→ Stat 증가 → 더 강한 Attack
🔁 메타 루프
Run 종료 → 자원 획득
→ Town 성장
→ 영구 스탯 증가
→ 다음 Run 강화
🛠️ 4. 아키텍처 규칙
BP Only (C++ 금지)
데이터 중심 설계
모든 스탯은 StatComponent 단일 책임
UI는 데이터 수정 금지
GameInstance = 영구 데이터
🎯 5. 핵심 유지 요소 (절대 유지)
로그 스케일 데미지 공식
그리드 인벤토리 전략성
실시간 스탯 반영
자동 + 수동 전투 혼합
✂️ 6. 스코프 조절
줄여도 되는 것
Worker AI → 타이머 대체
Farming → 상점
늘려도 되는 것
시너지 종류
몬스터 패턴
아이템 다양성