# 시작하기

현재 저장소에는 Bootstrap 문서·디렉터리와 메시지 전용 `erp2026_msgs` 패키지가 있습니다. 아래 명령은 Git과 저장소 구조 확인에 한정됩니다.

## 저장소 받기

```bash
git clone https://github.com/ERP-2026/ERP_2026.git
cd ERP_2026
```

접근 권한과 인증 방식은 팀의 GitHub 설정을 따릅니다. 다른 방식으로 clone했다면 해당 디렉터리에서 다음 단계를 진행하세요.

## 브랜치와 구조 확인

```bash
git branch --show-current
git status --short --branch
ls -la
find docs config scripts src -maxdepth 2 -print
```

새 작업은 `main`을 기준으로 목적에 맞는 브랜치를 만들어 진행합니다. 브랜치 이름과 PR 흐름은 [팀 개발 가이드](../team-dev-guide.md)를 참고하세요. 기존 작업 브랜치가 있다면 변경 사항을 먼저 확인하세요.

## 개발 시작 전

1. [프로젝트 README](../../README.md)와 [시스템 개요](../architecture/system-overview.md)에서 현재 상태와 목표 구조를 확인합니다.
2. 레거시 기능을 다룰 경우 [인수인계 메모](../handoffs/ERP_2024_handover.md)의 TODO와 실제 소스를 대조합니다.
3. 변경 범위와 검증 방법을 정하고, 기능 단위로 작업합니다.
4. 확인한 결과를 PR에 남깁니다.

프로젝트의 ROS1 배포판, 설치·빌드 명령은 아직 확정되지 않았습니다. 실제 환경을 확인한 뒤 검증된 명령을 이 문서에 추가합니다. 현재 ERP_2026의 실행 명령은 [런타임 명령](../operations/runtime-commands.md)을 참고하세요.
