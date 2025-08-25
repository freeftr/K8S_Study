# Section 09 Docker Networking

---
## Docker 네트워크 모드
- 컨테이너를 실행할 때 지정할 수 있는 대표 네트워크 옵션

### none
- 네트워크 미연결 상태
- 외부, 컨테이너와 통신 불가

### host
- 호스트와 동일 네임스페이스 사용 => 호스트와 포트 공유
- 포트 충돌 발생 가능

### bridge
- docker가 설치될 때 자동으로 bridge 네트워크 생성
- 호스트에선 `docker0`라는 가상 브리지 인터페이스로 보임.
  - `172.17.0.1`
- 컨테이너들은 이 브리지에 연결되어 서로 통신 가능

### Docker bridge 네트워크 동작
- `docker network ls` -> bridge 네트워크 확인 가능
-  `docker0` = bridge
  - 호스트에겐 인터페이스
  - 컨테이너에겐 스위치

1. 컨테이너 생성 시 Docker가 자동으로 새로운 네트워크 네임스페이스 생성
2. veth pair 생성
3. 한쪽 끝을 컨테이너 네임스페이스, 다른 쪽을 `docker0`에 연결
4. 컨테이너에 `172.17.0.x` IP 할당

=> 컨테이너간 통신 가능

### Port Mapping
- 브리지 네트워크의 컨테이너는 외부에서 접근 불가
- 포트 매핑으로 접근
~~~
docker run -d -p 8080:80 nginx
~~~
- 호스트의 8080을 80포트로 포워딩
- 외부 클라이언트는 `<HOST_IP>:8080`으로 접근 가능

### iptables NAT
- Docker는 iptables NAT 규칙을 자동 생성
- `-p 8080:80`옵션 시:
  - iptables PREROUTING 체인에 규칙 추가 -> 목적지 포트를 80으로
  - 컨테이너 내부 IP로 DNAT 수행

외부 요청 -> 호스트:8080 -> iptables NAT -> 컨테이너 IP:80