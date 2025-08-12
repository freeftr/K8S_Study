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

## 공식문서 활용법
***

## 📌 업그레이드 순서 (kubeadm 기준)
**항상** Control Plane → Worker 순으로 진행.

### 1. Control Plane 업그레이드
1. **kubeadm 최신 버전 설치** (예: 1.29.3)
   ```bash
   apt-get update
   apt-get install -y kubeadm=1.29.3-1.1
   kubeadm version
   ```
2. **업그레이드 계획 확인**
   ```bash
   sudo kubeadm upgrade plan
   ```
    - 현재/목표 버전, 자동 업그레이드 가능 컴포넌트, 수동 업그레이드 필요 항목(kubelet) 확인.
3. **Control Plane 구성요소 업그레이드**
   ```bash
   sudo kubeadm upgrade apply v1.29.3
   ```
    - API Server, Controller Manager, Scheduler 등 업그레이드.
4. **kubelet 업그레이드 전 drain**
   ```bash
   kubectl drain <control-plane-node> --ignore-daemonsets
   ```
5. **kubelet / kubectl 업그레이드 & 재시작**
   ```bash
   apt-get install -y kubelet=1.29.3-1.1 kubectl=1.29.3-1.1
   systemctl restart kubelet
   ```
6. **uncordon**
   ```bash
   kubectl uncordon <control-plane-node>
   ```

> 다중 Control Plane 환경이라면 각 노드마다 **`kubeadm upgrade node`** 후 kubelet 업그레이드 반복.

---

### 2. Worker 노드 업그레이드
1. **kubeadm 업그레이드**
   ```bash
   apt-get install -y kubeadm=1.29.3-1.1
   sudo kubeadm upgrade node
   ```
2. **drain**
   ```bash
   kubectl drain <worker-node> --ignore-daemonsets
   ```
3. **kubelet / kubectl 업그레이드 & 재시작**
   ```bash
   apt-get install -y kubelet=1.29.3-1.1 kubectl=1.29.3-1.1
   systemctl restart kubelet
   ```
4. **uncordon**
   ```bash
   kubectl uncordon <worker-node>
   ```
5. 모든 워커 노드에 대해 1~4단계 반복.

---

## 📌 업그레이드 후 확인
```bash
kubectl get nodes
```
- 모든 노드의 VERSION이 목표 버전(예: 1.29.3)인지 확인.
