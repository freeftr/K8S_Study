# Section 07 Authentication

### 인증의 대상
- 관리형 인증 대상
  - 관리자, 개발자
  - 자동화 프로세스
- 클러스터 안에서 동작하는 애플리케이션의 사용자들은 애플리케이션 단에서 자체적으로 진행한다.

### 인증 흐름
- 모든 접근은 API 서버를 거침. => 여기서 인증
1. 요청 수신
2. 인증 메커니즘을 통해 사용자 식별
3. 성공 시 인가 단계

### 인증 메커니즘
- Static file
  - Static Password File
    - CSV 형식: `password, username, userId[,group]`
    - 실행 옵션에 파일 경로 지정
  - Static Token File
    - CSV 형식: `token, username, userId[,group]`

위의 방식들은 권장되지 않는다.
- 보안적으로 취약.