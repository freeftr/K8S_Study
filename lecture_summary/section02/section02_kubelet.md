# Section 02 Kubelet

Kubelet은 노드의 모든 활동을 주도한다. 워커 노드에 설치되서 노드를 클러스터에 등록하고, 마스터와 연결을 유지한다.
- 노드 등록
- Pod 실행
- 컨테이너 런타임 실행
- Pod 상태 보고 -> API 서버에 전달

### 설치 방법
다른 컴포넌트와 다르게 kubeadm은 kubelet을 자동으로 설치해주지 않는다. 워커 노드에 수동으로 설치해야함.

1. 바이너리 다운로드
~~~
wget https://storage.googleapis.com/kubernetes-release/release/v1.27.0/bin/linux/amd64/kubelet
~~~