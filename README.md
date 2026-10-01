# ERP_2026

ERP_2026은 ERP42 차량을 이용한 실제 환경 자율주차 프로젝트입니다. ROS1을 기반으로 개발할 예정입니다. Repository Bootstrap 이후, ERP_2024의 실차 명령 경로에서 사용된 최소 ROS1 메시지 계약을 `erp2026_msgs`로 옮겼습니다. 실행 가능한 자율주차 시스템은 아직 없습니다. ERP_2024의 실행 코드는 이 저장소에 이식되지 않았습니다.

## 목표 아키텍처

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

Vehicle Interface는 차량 제어 로직과 실제 통신 방식을 분리하는 목표 계층입니다. Serial은 기존 시스템을 참고할 통신 방식이며, CAN은 향후 검토할 확장 방향입니다. 두 방식 모두 ERP_2026에 구현되어 있지 않습니다. 자세한 설계 방향은 [시스템 개요](docs/architecture/system-overview.md)를 참고하세요.

## 저장소 구조

```text
ERP_2026/
├── README.md, CONTRIBUTING.md
├── .gitignore, .gitattributes
├── .github/PULL_REQUEST_TEMPLATE.md
├── docs/                 # 설계, 인수인계, 개발 및 운영 문서
├── config/               # lidar, camera, localization, vehicle 설정 자리
├── scripts/run/          # 향후 검증된 실행 스크립트 자리
└── src/erp2026_msgs/     # 메시지 정의만 포함한 ROS1 패키지
```

`config/`와 `scripts/run/`은 자리 표시자만 있습니다. `src/`에는 메시지 패키지만 있으며, 캘리브레이션 값과 실행 스크립트는 추가되지 않았습니다.

## 현재 상태와 향후 작업

| 구분 | 내용 |
| --- | --- |
| 현재 | Repository Bootstrap과 `erp2026_msgs`의 최소 메시지 계약 |
| 향후 | ERP_2024 기능 분석, 추가 ROS1 패키지 설계, 기능 단위 이식 및 검증 |
| 미구현 | 센서 처리, 위치 추정, 주차 판단·경로 계획, 차량 제어, Vehicle Interface, 런타임 구성, CAN |

현재 패키지와 향후 후보는 [패키지 개요](docs/packages/package-overview.md)에 구분해 정리했습니다. 메시지의 확인 범위는 [차량 메시지 계약](docs/interfaces/vehicle-message-contracts.md)을 참고하세요.

## 문서 안내

- [문서 목차](docs/README.md): 전체 문서 탐색
- [시스템 개요](docs/architecture/system-overview.md): 목표 파이프라인과 계층 책임
- [ERP_2024 인수인계](docs/handoffs/ERP_2024_handover.md): 확인된 레거시 구성과 분석 과제
- [패키지 개요](docs/packages/package-overview.md): 향후 패키지 책임과 의존 방향
- [차량 메시지 계약](docs/interfaces/vehicle-message-contracts.md): 이식한 필드와 레거시 동작의 확인 상태
- [시작하기](docs/onboarding/getting-started.md): 저장소 확인과 개발 준비
- [런타임 명령](docs/operations/runtime-commands.md): 현재 실행 가능 상태와 향후 절차 기록 위치
- [팀 개발 가이드](docs/team-dev-guide.md), [기여 가이드](CONTRIBUTING.md): 브랜치, 검증, PR 규칙

## 기본 개발 흐름

`main`에서 작업 브랜치(`feat/*`, `fix/*`, `docs/*`, `refactor/*`, `chore/*`)를 만들고, 범위를 정해 변경한 뒤 해당 변경에 맞는 검증 결과를 PR에 기록합니다. 리뷰 후 `main`에 통합합니다. 기존 코드 이식은 기능 단위 PR로 진행합니다. 현재 설치·빌드·실행 명령은 확정되지 않았으므로 임의 명령 대신 [시작하기](docs/onboarding/getting-started.md)와 [런타임 명령](docs/operations/runtime-commands.md)의 상태를 확인하세요.
