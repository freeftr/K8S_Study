# Section 06 Cluster Upgrade Process

### 쿠버네티스 컴포넌트 버전 규칙
- kube-api server: 제일 버전이 높아야 함.
- controller-manager, scheduler: kube-api server보다 한 개까지 낮아도 된다.
- kubectl, kube-proxy: 최대 2개까지 낮아도 된다.
- kubectl: +- 1
- 쿠버네티스는 최근 3개 마이너 버전만 지원한다.

## 업그레이드 방법
***
- 한 번에 하나씩 업그레이드 
  - 1.1 -> 1.3 x
  - 1.1 -> 1.2 o
- `kubeadm upgrade`
- 수동 설치

### kubeadm 방법
1. 마스터 노드 업그레이드
   - `kubeadm upgrade plan`: 업그레이드 가능 범위 확인
   - `apt-get install -y kubeadm=`
   - `kubeadm upgrade apply`: 컨트롤 플레인 업그레이드
   - `apt-get install -y kubelet=`: kubelet 업그레이드 (재시작 필요)
2. 워커 노드 업그레이드
   - 한 노드씩 순차 업그레이드
   - `kubectl drain`
   - 업그레이드
   - `kubectl uncordon`
   - 모든 워커 노드에 반복

- 새 버전 노드를 추가한 다음 워크로드를 옮기고 구버전을 제거하는 방법도 있다.

### 다운타임
- 마스터 노드 업그레이드 시에는 워커 노드에는 영향이 없다. 관리만 안되는 것뿐.
