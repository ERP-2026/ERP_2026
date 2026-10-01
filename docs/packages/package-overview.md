# 패키지 개요

`erp2026_msgs`는 현재 생성된 메시지 전용 ROS1 패키지입니다. 나머지 이름은 ERP_2024 분석 후 결정할 **ROS1 패키지 후보**이며 아직 생성되지 않았습니다. 책임·경계 역시 구현 및 검증 과정에서 조정할 수 있습니다.

| 패키지 또는 후보 | 현재 상태 / 목표 책임 |
| --- | --- |
| `erp2026_msgs` | 현재: [실차 명령 경로의 최소 메시지 계약](../interfaces/vehicle-message-contracts.md)만 정의 |
| `erp2026_vehicle_interface` | 차량 통신 방식과 제어 계층 사이의 경계 |
| `erp2026_lidar` | LiDAR 데이터 입력·처리 경계 |
| `erp2026_camera` | Camera 데이터 입력·처리 경계 |
| `erp2026_localization` | GNSS/IMU 등으로부터 차량 상태·위치 추정 |
| `erp2026_parking` | 주차 판단, 계획, 제어 기능의 초기 묶음 후보 |
| `erp2026_bringup` | 검증된 노드 구성과 실행 절차 연결 |

## 의존 방향

공유 메시지 패키지는 필요한 각 기능 패키지가 참조할 수 있는 공통 계약입니다. 센서·위치 추정 결과는 주차 판단·계획으로, 계획 결과는 제어로, 제어 요청은 Vehicle Interface로 흐르도록 설계합니다. `erp2026_bringup`은 확정된 구성 요소를 조합하는 역할을 목표로 하며, 기능 로직을 담는 패키지로 취급하지 않습니다. 현재 세 메시지 외의 ROS 의존성, 토픽과 메시지 정의는 **TBD**입니다.

주차 기능의 책임이 명확해지면 `erp2026_parking`을 `parking_perception`, `parking_planner`, `control` 등으로 분리할 수 있습니다. 이 이름들도 현재 생성된 패키지를 뜻하지 않습니다. 통신 방식은 Vehicle Interface 내부의 확장 지점으로 두고, Serial 레거시를 먼저 분석하며 CAN 도입은 별도로 검토합니다.
