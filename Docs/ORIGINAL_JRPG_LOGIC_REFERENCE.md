# Gridventure: Original C++ JRPG Logic Reference

이 문서는 기존 `2ndConsoleGame` (C++ JRPG) 프로젝트의 핵심 로직과 아키텍처 분석 내용을 통합한 참조 가이드입니다. 이 내용은 UE5 블루프린트 이식 작업의 기술적 근거로 활용됩니다.

---

## 1. 엔진 구조 (Engine Structure)

기존 엔진(`Wannabe` 네임스페이스)은 컴포넌트 기반 액터 시스템과 상태 머신 기반 레벨 관리 방식을 채택하고 있습니다.

### 1.1 핵심 아키텍처
- **Engine 클래스:** 게임 루프(`Run`, `Tick`, `Draw`) 및 레벨 전환 관리.
- **Level 클래스:** 액터 리스트(`m_vecActors`)를 소유하고 개별 액터의 생명주기 제어.
- **Actor & Component:** 위치/팀 정보를 가진 액터에 `Stat`, `Skill`, `Inventory` 등의 컴포넌트를 부착하여 기능 확장.

### 1.2 렌더링 및 데이터
- **Renderer:** 콘솔 화면 버퍼(`ScreenBuffer`) 및 카메라 좌표 변환 관리.
- **DataManager:** JSON 파일에서 로드된 플레이어, 몬스터, 스킬/아이템 데이터를 캐싱.

---

## 2. 전투 구조 (Battle Structure)

턴 기반 명령 선택 방식과 상태 머신 기반의 페이즈 전환 로직을 기반으로 설계되었습니다.

### 2.1 전투 흐름 (BattleLevel Phases)
- **Phase_TurnCheck:** `TurnManager`를 통해 다음 행동 액터 선정.
- **Phase_CommandSelect:** 플레이어의 명령(공격, 스킬, 아이템) 입력 대기.
- **Phase_Animation:** 결정된 명령의 실행 및 시각적 연출(Cutscene).
- **Phase_Result:** 승패 판정 및 보상 처리.

### 2.2 핵심 컴포넌트
- **BattleResolver:** 대미지 계산(`CalcDmg`), 명중/크리티컬 판정 수행.
- **BattleContext:** 전투 중 발생하는 모든 서브시스템(로그, 컷신 등)의 저장소.

---

## 3. 필드 구조 (Field Structure)

플레이어가 월드 맵을 탐험하며 이벤트나 몬스터와 조우하는 공간입니다.

### 3.1 필드 시스템
- **TileMap:** 배경 및 충돌 정보(`CanMove`)를 담은 격자 데이터 구조.
- **FieldState:** `Phase_Idle`, `Phase_Move`, `Phase_EnemyTurn` 등의 상태 순환.

### 3.2 주요 기능
- **이동 범위:** 캐릭터 스탯에 기반한 도달 가능 타일 하이라이트(`CalcMoveRange`).
- **인카운터:** 필드 몬스터 액터와의 충돌 시 `BattleLevel`로 전환(`StartBattleTransition`).

---

## 4. 액터 생명주기 (Actor Lifecycle)

전투 레벨에서 액터가 생성되고 소멸되는 프로세스입니다.

### 4.1 생성 및 초기화
- `SetupBattle()` -> `Level::AddNewActor()` -> `BeginPlay()` 순으로 초기화.

### 4.2 상태 관리 및 소멸
- **Active 상태:** HP가 0이 되면 `m_bIsActive = false` 처리.
- **정리 로직:** `CleanupDeacActor()`가 매 틱마다 비활성화된 액터를 검사하여 메모리 해제(`m_vecPendingDestroy`).

---

## 5. 스킬 및 아이템 구조 (Skill & Item Structure)

`ActionData`라는 공통 데이터 구조를 통해 스킬과 아이템을 통합 관리합니다.

### 5.1 데이터 정의 (`ActionData`)
- **TID:** 고유 식별자.
- **CombatEffectData:** 발동 시 효과 리스트(대미지, 힐, 상태 이상).
- **StatModifier:** 장비 착용 시 부여되는 스탯 보정치.

### 5.2 사용 프로세스
- **SkillComponent / InventoryComponent:** 액터가 보유한 스킬과 아이템 인스턴스를 관리.
- **ResolveAction:** `BattleResolver`가 `ActionData` 내의 효과들을 순회하며 대상에게 적용.
