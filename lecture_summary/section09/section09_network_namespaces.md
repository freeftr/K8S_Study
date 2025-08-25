# Section 09 Network Namespaces

- 네임스페이스는 리눅스에서 프로세스/네트워크를 격리하기 위한 기능
- 네트워크 네임스페이스는 인터페이스, 라우팅 , ARP 테이블을 각 컨테이너가 가지게 해줌.

### 명령어
- `ip netns add`:네임스페이스 생성
- `ip netns`: 목록 보기
- `ip netns exec <name> ip link`: 네임스페이스 내부 명령 실행
  - 루프백 인터페이스만 보임
  - 호스트 NIC는 안보임
- `ip link add veth-red type veth peer name veth-blue`: veth pair 생성
  - 가상 이더넷 케이블
- `ip link set veth-red netns red`: 각 끝단을 네임스페이스에 할당
- `ip netns exec red ip addr add 192.168.15.1/24 dev veth-red`: IP 주소부여
- `ip netns exec red ip link set veth-red up`: 인터페이스 활성화

### 여러 네임스페이스 연결 Linux Bridge
~~~
ip link add v-net-0 type bridge
ip link set v-net-0 up
~~~
- 브리지 생성

~~~
ip link add veth-red type veth peer name veth-red-br
ip link set veth-red netns red
ip link set veth-red-br master v-net-0
~~~
- veth로 각 네임스페이스 연결

### 호스트 네임스페이스 연결
- 브리지는 호스트에도 인터페이스로 보임
- 호스트에 IP 할당
~~~
ip addr add 192.168.15.5/24 dev v-net-0
~~~

### 외부 네트워크 연결
- 외부 통신하려면 게이트웨이 설정 필요.
- 네임스페이스 라우팅 테이블에 호스트를 게이트웨이로 추가
~~~
ip netns exec blue ip route add default via 192.168.15.5
~~~
- 호스트에서 iptables NAT 설정
~~~
iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE
~~~

### 외부에서 네임스페이스 접근
- 사설이라 외부에서 직접 접근 불가. 아래의 방법으로 해결 가능
1. 외부 호스트에 라우트 추가
2. 포트 포워딩
~~~
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT \
  --to-destination 192.168.15.2:80
~~~

### 정리
- `ip netns`로 네임스페이스 관리
- veth pair + bridge로 네임스페이스, 호스트 간 연결 설정
- 라우팅 + NAT 설정으로 외부 네트워크 접근
- 포트 포워딩으로 외부에서 오는 네임스페이서 접근 허용