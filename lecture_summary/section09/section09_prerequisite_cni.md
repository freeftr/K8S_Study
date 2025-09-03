# Section 09 Prerequisite CNI

### CNI
- Container Network Interface: 컨테이너 런타임과 네트워킹 플러그인 간의 표준 인터페이스
- 런타임이 네임스페이스 생성, CNI가 네트워크와 연결

### 컨테이너 런타임
1. 네트워크 네임스페이스 생성
2. 컨테이너가 붙을 네트워크 확인.
3. `ADD`, `DEL`로 플러그인 호출

### 플러그인
1. IP 할당, 라우트 설정 등 네트워킹 처리
2. JSON 파라미터 기반으로 동작.
3. stdout으로 실행 결과 출력

### Docker
- Docker는 CNI를 지원하지 않는다. => CNM 사용
- Docker 컨테이너를 `none`으로 생성 후 CNI 플러그인 호출