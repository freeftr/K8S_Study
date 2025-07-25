# Section 02 Kube-Proxy

쿠버네티스 클러스터 안에서는 Pod끼리 서로 접근할 수 있다. 이게 가능한 이유는 Pod 네트워크라는 가상 네트워크 솔루션이
클러스터에 구성되어 있기 때문이다.

### Service
Pod는 IP가 고정되어 있지 않기 때문에 고정된 IP를 갖기 위해 Service를 생성해 사용한다. Service는 사실 실체가 없다.
네트워크 인터페이스도 없고, 직접적으로 요청을 수신하는 프로세스도 없다.

### Kube-Proxy의 역할
Kube-Proxy는 각 노드에서 실행되며, 새로운 서비스를 찾으며, 생성될 때 마다 각 노드에 iptables 규칙을
만들어 트래픽을 라우팅합니다. Pod IP에 들어오는 트래픽을 Service의 IP에 포워딩하는 방식이다.

### 설치 방법
1. 바이너리 다운로드
~~~
wget https://storage.googleapis.com/kubernetes-release/release/v1.27.0/bin/linux/amd64/kube-proxy
~~~

Kubeadm을 사용하면, 각 노드에 DaemonSet 형태로 자동 배포된다. 모든 노드에 1개씩 상시 실행된다.