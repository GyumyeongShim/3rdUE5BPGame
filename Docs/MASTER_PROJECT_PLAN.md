# Gridventure: Master Integration Plan & System Map

이 문서는 프로젝트의 전체 시스템 구조(System Map)와 이를 구현하기 위한 단계별 로드맵(Roadmap)을 통합한 마스터 가이드입니다. 이 문서를 기준으로 프로젝트의 범위를 조정하고 진행 상황을 관리합니다.

---

## 🏗️ 1. 전체 시스템 구조 (Full System Map)

### 1.1 Core Engine (기초 엔진 및 캐릭터)
- **[Stat System]**: 캐릭터/몬스터 스탯 관리 (HP, ATK, DEF, SPD, Accuracy, Evasion, Crit).
- **[Damage Logic]**: 로그 스케일 방어력 감쇄 공식을 포함한 최종 대미지 산출.
- **[Status Effect]**: 버프(AtkUp 등) 및 디버프(Stun, Poison 등) 상태 이상 시스템.
- **[Leveling]**: 경험치 획득 및 성장에 따른 스탯 상승 로직.

### 1.2 Town System (마을 건설 및 메타 성장)
- **[Construction]**: 건물 건설 및 배치 (가방 그리드 확장 연동).
- **[Resource/Worker AI]**: 자원(나무, 돌) 채집 및 주민 NPC 자동화.
- **[Farming]**: 버프 아이템 생산을 위한 농경 시스템.

### 1.3 Field System (전투 및 실시간 생존)
- **[Movement/Wave]**: 실시간 이동 및 시간별 적 스폰 관리자.
- **[Projectile]**: 탄막 및 스킬 투사체 발사/판정 로직.
- **[Drop/Pooling]**: 테트리스 아이템 드랍 및 수백 명의 적 처리를 위한 최적화.

### 1.4 Inventory System (그리드 전략)
- **[Grid Management]**: 2D Array 기반의 가방 격자 (모양, 회전 포함).
- **[Synergy Logic]**: 아이템 간 인접 위치에 따른 추가 스탯 보너스 (Backpack Hero 스타일).
- **[Equipment]**: 가방 내 배치 위치에 따른 장비 효력 발생 판정.

---

## 📅 2. 실행 로드맵 (Execution Roadmap)

### Phase 1: 기반 데이터 및 구조체 (Foundation) - [진행 예정]
- **목표**: 모든 시스템의 뼈대가 되는 데이터 형식을 BP로 정의.
- **세부 작업**:
    - [ ] `EItemType`, `ECombatEffectType` 등 핵심 열거형(Enum) 생성.
    - [ ] `FStatData`, `FActionData`, `FStatModifier` 등 BP 구조체(Struct) 생성.
    - [ ] `DT_ItemData`, `DT_CharacterData` 데이터 테이블 초안 구축.

### Phase 2: 코어 컴포넌트 이식 (Core Logic)
- **목표**: 기존 C++ JRPG의 핵심 기능을 블루프린트 컴포넌트로 변환.
- **세부 작업**:
    - [ ] `BPC_StatComponent`: 실시간 스탯 및 레벨업 관리.
    - [ ] `BPL_BattleCalculator`: 로그 스케일 대미지 수식 함수 라이브러리.
    - [ ] `BPC_StatusComponent`: 상태 이상 틱 대미지 및 지속시간 관리.

### Phase 3: 그리드 인벤토리 및 시너지 (Inventory)
- **목표**: 가방 시스템 구축 및 스탯 컴포넌트와 연동.
- **세부 작업**:
    - [ ] 그리드 배치 로직 및 아이템 회전 기능.
    - [ ] **[Synergy Logic]**: 인접 아이템 체크 후 `StatComponent`에 보정치 전달.

### Phase 4: 실시간 전투 및 적 AI (Field Combat)
- **목표**: Survivors 스타일의 필드 루프 완성.
- **세부 작업**:
    - [ ] 웨이브 스폰 시스템 및 적 풀링(Pooling).
    - [ ] 플레이어 투사체 스킬 (`ActionData` 연동) 구현.

### Phase 5: 마을 건설 및 통합 (Town & Meta)
- **목표**: 마을 성장 데이터가 전투 스탯에 영구적으로 반영되는 루프 완성.
- **세부 작업**:
    - [ ] 건물 건설 효과를 통한 가방 그리드 확장.
    - [ ] 마을 보너스 스탯을 `StatComponent`에 영구 적용.

---

## 🛠️ 3. 전략적 범위 관리 (Scope Control)

- **축소 가능 항목**: Worker AI (단순 타이머로 대체), Farming System (단순 상점으로 대체).
- **확장 가능 항목**: 아이템 시너지 종류 증대, 보스 패턴 다양화.
- **필수 유지 항목**: 로그 스케일 대미지 수식, 그리드 인벤토리 전략성.

---

## 🔬 4. 기술 명세 부록 (Technical Appendix)
이 프로젝트의 일관된 구현을 위해 다음 기술 사양을 반드시 준수합니다.

### 4.1 핵심 대미지 및 판정 공식 (The Math)
- **최종 대미지 (`FinalDamage`)**: 
  1. `BasePower = (Atk * (Power * 0.01) * (Ratio * 0.01))`
  2. `Mitigation = (100.0 / (100.0 + Def))` (방어력 100당 대미지 50% 감소)
  3. `FinalDamage = Max(1, BasePower * Mitigation * Random(0.9, 1.1))`
- **명중 판정 (`HitChance`)**: `Max(5%, Accuracy - Evasion)`
- **치명타 판정 (`CritChance`)**: `Max(0%, CritChance - CritResist)` (성공 시 1.5배 대미지)

### 4.2 시스템 아키텍처 및 제약 (Architecture & Constraints)
- **제한 사항**: 모든 기능은 **순수 블루프린트(BP Only)**로 구현하며, C++ 클래스 생성을 금지함.
- **데이터 영속성 (`Data Persistence`)**:
  - `GI_Gridventure (GameInstance)`: 마을 건설 상태, 인벤토리 아이템 리스트, 현재 스탯 데이터를 저장 및 레벨 간 공유.
  - `DT_ItemData (DataTable)`: 모든 아이템의 데이터 소스.
- **핵심 컴포넌트**:
  - `BPC_StatComponent`: 캐릭터의 현재 HP/Exp/Level 관리.
  - `BPL_BattleCalculator (Function Library)`: 위 수학 공식을 담은 순수 함수 라이브러리.

### 4.3 추천 폴더 구조 (Project Structure)
- `/Content/Gridventure/Blueprints/Core/`: `GI`, `StatComponent`, `BattleLib` 등 핵심 로직.
- `/Content/Gridventure/Blueprints/Data/`: `Struct`, `Enum`, `DataTable` 등 데이터 정의.
- `/Content/Gridventure/Blueprints/UI/`: 인벤토리 그리드, 스탯 창, HUD 등 시각 요소.
- `/Content/Gridventure/Blueprints/Actors/`: 플레이어, 적, 투사체, 건물 액터.
