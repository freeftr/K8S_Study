# Section 07 Authorization

Authorization => 누가 어떤 일을 할 수 있는지 정하는 것.

## Kubernetes Authorization
***
### Node Authorizer
- kubelet이 API Server에 보내는 요청을 처리한다.
- kubelet이 노드 상태를 보고하고, Pod/Service 정보 조회 가능하도록 변경
- CN: `system:node:<node-name>`
- 그룹: `system:nodes`

### ABAC
- Attribute Based Access Control
- 사용자별 권한 검증
- 정책을 JSON 파일로 정의해 API Server에 전달한다.
- 한계점: 사용자별 권한이 바뀌면 파일 수정 후 API Server 재시작 필요

### RBAC
- Role-Based Access Control
- 역할별 권한을 검증
- 역할 수정 시 해당 역할에 속한 모든 사용자 권한 자동 변경

### Webhook
- 외부 시스템에 인가 위임

### AlwaysAllow / AlwaysDeny
- AlwaysAllow: 모든 요청 허용 (기본값)
- AlwaysDeny: 모든 요청 거부