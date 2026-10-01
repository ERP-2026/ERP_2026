# 시스템 개요

## 현재 상태

아래 구조는 **ERP_2026의 목표 아키텍처**입니다. 현재는 `erp2026_msgs`의 세 메시지 정의만 이식했습니다. 토픽·실행 노드·차량 통신 구현은 없고, 기존 ERP_2024의 실행 코드는 이식되지 않았습니다.

## 목표 파이프라인

```text
LiDAR / Camera / GNSS / IMU
             ↓
Perception / Localization
             ↓
Parking Decision & Planning
             ↓
Vehicle Control
             ↓
Vehicle Interface
             ↓
ERP42
```

| 계층 | 목표 책임 |
| --- | --- |
| LiDAR / Camera / GNSS / IMU | 센서 측정값을 상위 계층에 제공 |
| Perception / Localization | 주차 환경 인지와 차량 상태·위치 추정 |
| Parking Decision & Planning | 주차 공간 판단과 주차 경로·동작 계획 |
| Vehicle Control | 계획을 차량 구동에 필요한 제어 요청으로 변환 |
| Vehicle Interface | 제어 요청을 차량 통신 방식에 맞게 전달하고 통신 세부사항을 격리 |
| ERP42 | 실제 제어 대상 차량 |

계층 간 연결은 ROS1 토픽·메시지 기반으로 설계할 예정입니다. 각 계층이 다른 계층의 내부 구현에 직접 의존하지 않도록 인터페이스를 정하고, 데이터 형식·좌표계·주기·오류 처리 규칙은 실제 통합 시 검증해 문서화합니다. 이식된 세 메시지 외의 구체적인 토픽명, 메시지 정의와 노드 연결은 **TBD**입니다.

## Vehicle Interface 방향

```text
Vehicle Control
      ↓
Vehicle Interface
   ├─ Serial (ERP_2024의 현재 참고 방식)
   └─ CAN    (향후 확장 후보)
```

Vehicle Control이 통신 방식의 세부사항에 직접 의존하지 않도록 Vehicle Interface의 경계를 정의할 계획입니다. Serial은 레거시의 참조 방식이며 ERP_2026에서 아직 구현되지 않았습니다. CAN은 추후 도입 여부와 요구사항을 검토할 대상이고, 현재 구현 또는 지원 기능이 아닙니다. 실제 ERP42 통신 사양과 안전 동작은 조사·검증 전까지 **TBD**입니다.
