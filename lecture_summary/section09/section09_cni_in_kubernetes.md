# Section 09 CNI in Kubernetes

### CNI 플러그인 설치 위치
- 바이너리: `/opt/cni/bin`
- 설정: `/etc/cni/net.d`
  - 어떤 플러그인을 사용할지 정의하는 JSON 파일 존재
  - 알파벳 순

~~~
{
  "cniVersion": "0.3.1",
  "name": "mynet",
  "type": "bridge",
  "bridge": "cni0",
  "isGateway": true,
  "ipMasq": true,
  "ipam": {
    "type": "host-local",
    "subnet": "10.244.0.0/16",
    "routes": [
      { "dst": "0.0.0.0/0" }
    ]
  }
}
~~~
- name: 네트워크 이름
- type: 사용할 플러그인 종류
- isGateway: 브리지 게이트웨이 역할 수행 여부
- ipMasq: NAT 규칙 추가 여부
- ipam: IP 할당 방식
  - host-local: 노드 로컬에서 IP 관리
  - dhcp: 외부 DHCP 서버 사용
