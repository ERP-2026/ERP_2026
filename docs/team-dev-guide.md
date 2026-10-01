# 팀 개발 가이드

## 작업 흐름

```text
main
  ↓
feat/*, fix/*, docs/*, refactor/*, chore/*
  ↓
implementation
  ↓
validation
  ↓
Pull Request
  ↓
review
  ↓
main
```

`main`에서 직접 개발하는 일을 지양하고, 변경은 작업 브랜치의 PR로 통합합니다. 예를 들어 기능은 `feat/parking-space-analysis`, 수정은 `fix/...`, 문서는 `docs/...`, 구조 변경은 `refactor/...`, 저장소 관리는 `chore/...`를 사용합니다.

작업 전에는 현재 브랜치와 작업 트리를 확인하고, PR에는 변경 목적·범위·검증 결과·남은 한계를 기록합니다. 리뷰에서 지적된 사항을 반영한 뒤 통합합니다. 이식 작업은 레거시 전체를 한 번에 들여오지 않고 기능별로 나누어, 각 PR에서 입력·출력과 실제 동작을 확인합니다. 세부 원칙은 [CONTRIBUTING.md](../CONTRIBUTING.md)를 참고하세요.
