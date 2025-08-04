# Section 04 Monitoring Cluster Components

### 모니터링 주요 대상
- Node 레벨
  - Node 수
  - 건강 상태
  - CPU, 메모리, 네트워크, 디스크 사용률
- Pod 레벨
  - Pod 수
  - 각 파드의 CPU 및 메모리 사용률

쿠버네티스에는 완전한 내장형 모니터링 솔루션은 없다 -> 오픈소스 사용.

## 모니터링 도구
***

### Metrics Server
- 클러스터당 한 대
- 노드와 파드의 메트릭들을 집계해서 **인 메모리**에 저장한다.
  - 디스크에 저장하지 않기 때문에 과거 이력은 볼수 없음.
- 쿠버네티스는 각 노드에서 Kublet의 cAdvisor 컴포넌트를 통해 Pod의 메트릭들을 받아와서 Metric Server에 보낸다.

### 명령어
- `kubectl top node`: 노드 리소스 사용량 확인