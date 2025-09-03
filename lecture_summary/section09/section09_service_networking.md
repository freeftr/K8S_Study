# Section 09 Service Networking

## Service Networking
- Pod 네트워킹은 각 노드에 브리지 네트워크를 만들고, Pod마다 네임스페이스, 인터페이스 IP를 붙여주어 Pod간 통신을 가능하게 함.
- 하지만 일반적으로 Service 객체를 통해 통신.
- ClusterIP: 클러스터 내부에서만 접근 가능한 가상 IP를 부여. Pod들이 클러스터 내에서 이 IP를 통해 서로 접근.
- NodePort: CluterIP와 동일하게 Pod 간 접근 가능. 각 노드의 특정 포트를 통해 외부에서도 접근 가능.
- kube-proxy가 각 노드에서 iptables/ipvs 규칙을 만들어 트래픽을 Pod로 DNAT 시킴.