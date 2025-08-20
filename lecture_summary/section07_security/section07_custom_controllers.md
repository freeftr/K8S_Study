# Section 07 Custom Controllers

- Controller: 루프를 돌면서 클러스터 상태를 감시하고, 변경 이벤트에 따라 액션을 수행하는 프로세스

### Custom Controller 개발
- 보통 Go로 작성
  - Shared Informer: 효율적인 캐싱 + 이벤트 큐 관리
  - Kubernetes와의 통합성

1. 샘플 컨트롤러 클론
2. Controller.go 수정
3. 빌드 및 실행

- Docker 이미지로 패키징 후 Pod로 배포
- CRD와 함께 배포하면 CR을 생성했을 때 컨트롤러가 자동으로 동작

*CKA에는 직접적으로 Custom Controller를 구현하는 문제는 안나온다.

---

## Operator Framework
- Operator: CRD + Controller
  - Operator가 CRD 생성
  - Controller를 Deployment로 실행
  - CR을 자동으로 감시하고 동작 수행

### 목적
- 운영 작업의 자동화
  - 설치/업데이트
  - 백업/복구
  - 상태 점검/장애 복구
