# Section 09 Cluster Networking

### 기본 네트워킹 요구사항
- 각 마스터 및 워커 노드에는 하나 이상의 네트워크 인터페이스가 있어야 함.
- 각 노드는 반드시 다음을 포함해야 한다.
  - 고유한 호스트 이름
  - 고유한 MAC 주소
  - 올바르게 할당된 IP 주소

### 필수 오픈 포트
- API server: 6443
- kubelet: 10250
- kube-scheduler: 10259
- kube-controller-manager: 10257
- etcd: 2379
  - 다중 마스터 시 + 2380
- NodePort: 30000-32767