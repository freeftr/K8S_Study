# Section 07 Kubernetes Security Primitives

### Host 보안
- Password 기반 로그인 비활성화
- 루트 접근 차단
- SSH 키 기반 인증 허용

## Kube-api Server 보안
***
- Kube-API 서버는 쿠버네티스의 중심으로 첫 번째 방어선이 되어야 함.

### 누가 접근할 수 있는가?
- Authentication
- 정적 파일에 저장된 ID, 비밀번호 및 토큰
- Certificates
- LDAP
- Machine/Pod => Service Account

### 무엇을 할 수 있는가?
- Authorization
- RBAC
- ABAC
- Node Authorizer
- Webhook

### 컴포넌트 간 통신 보안
- TLS 암호화로 보호
- 각 컴포넌트 사이의 인증서 필요
- NetworkPolicy로 Pod 간 접근 제한.

> **전체 흐름**
> 
> 호스트 -> API 서버 인증/인가 -> 컴포넌트 TLS -> Pod 네트워크 정책