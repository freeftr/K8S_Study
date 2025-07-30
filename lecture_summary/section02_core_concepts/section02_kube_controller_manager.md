# Section 02 Kube Controller Manager
쿠버네티스의 컨트롤러들을 관리한다.
1. 상태를 지속적으로 감시하고
2. 필요한 조치를 자동으로 수행

### Node Controller
노드 상태를 감시하는 컨트롤러이다.
1. Node Monitor Period: 5초마다 노드 상태를 확인
2. Node Monitor Grace Period: 노드로부터 heartbeat를 받지 못하면, 40초 정도 대기한 후 UNREACHABLE 마킹을 한다.
3. POD Eviction Timeout: 다시 살아날 5분의 유예 기간을 준다.
4. 다시 살아나지 않으면 Pod들을 제거하고, Pod들이 ReplicaSet의 일부였다면, 정상적인 다른 노드에서 재생성한다.

### Replication Controller
ReplicaSet의 상태를 감시하는 컨트롤러이다. 항상 지정된 수의 Pod가 존재하도록 유지한다.
- 수보다 적으면? 생성
- 수보다 많으면? 제거

이러한 컨트롤러들은 Kube Controller Manager로 묶여 제공된다.

### 설치 방법
1. 바이너리 다운로드
~~~
wget https://storage.googleapis.com/kubernetes-release/release/v1.27.0/bin/linux/amd64/kube-controller-manager
~~~
2. 압축 해제 후 마스터에서 서비스 형태로 실행.

다양한 옵션을 설정할 수 있는데 `kube-controller-manager.service`에서 위의 감시 주기, 유예 기간등 설정을 할 수 있다.
기본 옵션으로 모든 컨트롤러가 활성화 상태로 실행되며 `--controllers` 지정할 수 있다.

Kubeadm을 사용했다면 `kube-system` 네임스페이스의 Pod로 실행된다.
- `/etc/kubernetes/manifests/kube-controller-manager.yaml`

수동인 경우 다음과 같다.
- `/etc/systemd/system/kube-controller-manager.service`