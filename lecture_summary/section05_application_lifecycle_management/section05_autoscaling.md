# Section 05 Autoscaling

## Scaling이란?
***
- 수직 확장: 장비의 성능 자체를 업그레이드
  - CPU, 메모리 등
  - 기존 앱 중단 필요
- 수평 확장: 서버를 추가해 로드 분산
  - 무중단


### 쿠버네티스에서의 Scaling
- Workload
  - Pod에 할당된 CPU/메모리 증가 => 수직
  - Pod 개수 조절 => 수평
- Cluster
  - 기존 노드 리소스 증가 => 수직
  - 노드 수 조절 => 수평

## 스케일링 방법
***

### Manual
- WorkLoad
  - `kubectl scale`
  - `kubectl edit`으로 리소스 수정
- Cluster
  - VM 추가 후 `kubeadm join`

### Auto
- Workload
  - Horizontal Pod Autoscaler(HPA): CPU/메모리 사용량 기준으로 Pod 수를 자동 조정
  - Vertical Pod Autoscaler(VPA): Pod의 CPU/메모리 요청/제한을 자동 조정
- Cluster
  - Cluster Autoscaler: 사용량 기준으로 노드 수 자동 조정

