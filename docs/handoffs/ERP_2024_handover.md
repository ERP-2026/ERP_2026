# ERP_2024 인수인계 메모

이 문서는 현재 전달된 레거시 패키지 이름과 분석 방향만 기록합니다. ERP_2024 소스의 실제 의존 관계와 동작은 아직 검증하지 않았고, ERP_2026에는 레거시 코드를 복사하지 않았습니다. 아래 오른쪽 이름은 **향후 이식·리팩터링 후보**입니다.

| ERP_2024 항목 | ERP_2026 방향 | 확인할 사항 |
| --- | --- | --- |
| `Total_msgs` | `erp2026_msgs` | 실제 메시지 정의 및 사용자 확인 |
| `erp42_serial` | `erp2026_vehicle_interface` | 명령·상태 흐름, 통신 경계 확인 |
| `Lidar_ERP_2024` | `erp2026_lidar` | 입력·출력과 의존성 확인 |
| `camera` | `erp2026_camera` | 센서 및 처리 범위 확인 |
| `INS_Integration` | `erp2026_localization` | 좌표계와 추정 흐름 확인 |
| `vectornav` | localization에 포함 또는 vendor driver 유지 검토 | 드라이버 역할 및 라이선스·의존성 확인 |
| `GPSIMU_MORAI` | simulation-only reference | 실제 환경 코드와 분리 범위 확인 |
| `morai_msgs` | simulation dependency | 시뮬레이션 의존성 분리 확인 |
| `ERP2024` | parking/planning/control/bringup 등으로 분해 검토 | 기능·노드별 책임 확인 |
| `Map` | maps/config/data 형태 검토 | 파일 형식과 사용처 확인 |
| `Models` | model asset로 관리 검토 | 모델 종류, 크기, 출처 확인 |

## 우선 분석 대상: `Parking.cpp`

소스 분석 시 다음 항목을 확인해야 합니다. 아래 항목은 분석 과제이며, 내부 동작이 검증되었다는 뜻은 아닙니다.

- parking-space decision: 공간 판단의 입력, 조건, 출력
- parking path generation: 경로 생성의 입력, 결과, 좌표계
- forward/reverse gear switching: 전·후진 전환 조건과 안전 조건
- `PubSerial` command flow: 제어 명령 생성 위치와 Serial 전달 경로

## 남은 TODO

- [ ] 실제 소스·빌드 파일에서 패키지 경계와 의존 관계 확인
- [ ] `Parking.cpp`의 위 네 흐름과 관련 메시지 확인
- [ ] 하드웨어 통신 사양과 실제 환경·시뮬레이션 의존성 구분
- [ ] 이식 가능한 기능을 작은 PR 단위로 분할
