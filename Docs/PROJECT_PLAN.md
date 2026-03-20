# Project: Crop-Out Survivors (Backpack Strategy)
## Hybrid Management & Combat RPG

### 1. Overview
이 프로젝트는 **UE5 Cropout**의 건설/경영 요소, **Vampire Survivors**의 대규모 전투, 그리고 **Backpack Hero**의 전략적 인벤토리 시스템을 결합한 하이브리드 장르 게임입니다.

### 2. Core Systems

#### 2.1 Town System (Based on Cropout)
- **Role:** Meta-progression 및 영구적 업그레이드 기지.
- **Key Features:**
    - 건물 건설을 통한 가방 격자(Grid) 확장.
    - 농작물 재배를 통한 소모성 버프 아이템 생산.
    - 주민 AI를 활용한 자동 자원 채집.

#### 2.2 Field System (Based on Vampire Survivors)
- **Role:** 핵심 액션 및 자원 획득처.
- **Key Features:**
    - 실시간 탄막 생존 전투.
    - 적 처치 시 테트리스 블록 형태의 장비/아이템 드랍.
    - 시간에 따라 강해지는 웨이브 시스템.

#### 2.3 Inventory System (Based on Backpack Hero)
- **Role:** 전투 중 실시간 전략 수립.
- **Key Features:**
    - **Grid-based Placement:** 제한된 공간에 아이템 효율적 배치.
    - **Shape & Rotation:** 아이템마다 다른 모양과 회전 기능.
    - **Synergy Logic:** 아이템 간 인접 위치에 따른 보너스 스탯 (예: 방패 옆에 배치된 갑옷의 방어력 상승).

### 3. Architecture & Data Flow
1. **Game Instance:** 맵 이동(Town <-> Field) 시 플레이어의 가방 데이터와 마을 상태 저장.
2. **Enhanced Input:** 맵 특성에 맞는 입력 매핑 컨텍스트(IMC) 자동 교체 (Town: RTS Mouse / Field: Direct Movement).
3. **Item Data Asset:** 아이템의 모양, 스탯, 시너지 로직을 정의하는 데이터 구조.

### 4. Technical Goals
- **Optimization:** 수백 명의 적을 처리하기 위한 가벼운 AI 및 풀링 시스템.
- **UI/UX:** 직관적인 드래그 앤 드롭 그리드 시스템 (PC/Mobile 공용).
- **Extensibility:** 새로운 아이템 모양과 효과를 쉽게 추가할 수 있는 데이터 중심 설계.

### 5. Next Steps
- [ ] Town과 Field 간의 Level Streaming/Travel 시스템 구현.
- [ ] 기초 Grid Inventory Component 제작 (2D Array 기반).
- [ ] Survivor 스타일의 기본 적 스폰 로직 및 캐릭터 이동 구현.
