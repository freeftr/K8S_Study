# Section 02 Core Concepts

# 쿠버네티스 아키텍처
***
## 쿠버네티스의 목적
애플리케이션을 컨테이너 형식으로 자동화된 방식으로 배포하여 서비스간 통신과 관리를 용이하게 해준다.

## 쿠버네티스 클러스터
쿠버네티스 클러스터는 마스터, 워커 노드로 구성된다.
- Worker node: 애플리케이션을 컨테이너로 적재.
- Master node: 컨테이너 관리
  - 각 노드 정보 저장
  - 적재 계획
  - 컨테이너 및 노드 모니터링

마스터 노드는 이러한 역할을 **컨트롤 플레인 컴포넌트**들을 통해 수행한다.

### Control plane component
- etcd: 컨테이너 관련 정보를 키-값 형태로 저장하는 키-값 저장소
- scheduler: 컨테이너의 리소스 요구사항, 워커 노드의 용량, taints와 tolerations, node affinity와 같은 정책 및 제약 조건을 기반으로
적절한 노드를 찾아 컨테이너를 배치한다.
  - taints: 특정 노드를 오염시켜서 모든 파드가 해당 노드에 스케줄되지 못하게 하는 방법 -> 차단
  - tolerations: taints를 가진 특정 노드에 이 파드는 예외적으로 들어갈 수 있다고 명시하는 것 -> 허용, 예외
  - node affinity: 파드를 특정 노드에 스케줄되도록 유도하는 기능
- controller-manager
  - node-controller: 노드를 관리
    - 새 노드를 클러스터에 온보딩
    - 장애 노드 처리
  - replication controller: 파드가 특정 개수만큼 복제되고 동작하는 것을 보장
- Kube API Server: 클러스터 내 모든 작업 

### Node component
- Container Runtime Engine: 컨테이너의 실행을 담당하는 소프트웨어
  - ex) Docker
- Kubelet: 클러스터의 각 노드에서 실행되는 에이전트.
  - Kube API 서버의 요청 적용.
  - Kube API 서버에서 노드 상태 주기적으로 보고서 가져옴.
- Kube-proxy: 실행 중인 컨테이너 애플리케이션 간 통신을 담당.