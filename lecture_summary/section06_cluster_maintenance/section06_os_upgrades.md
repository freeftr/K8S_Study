# Section 06 OS Upgrades

### 노드 다운에 대한 쿠버네티스 기본 동작
- 5분의 유예를 주고, 그 사이에 복구되면 Kubelet이 다시 pod를 실행.
- 넘어가면, dead로 판단하고 종료.
- ReplicaSet에서 관리하면 다른 노드에서 재생성되지만, 그렇지 않은 경우에는 사라진다.

### Drain & Uncordon
- `kubectl drain <node>`: 해당 노드의 모든 pod를 안전하게 종료 후 다른 노드에서 재생성.
  - 이때 노드는 unschedulable 상태가 된다.
- `kubectl cordon <node>`: 새 pod가 해당 노드에 스케줄되지 않도록 설정
- `kubectl uncordon <node>`: 노드를 다시 스케줄 가능 상태로 변경.
  - 이때 옮겨간 pod는 다시 돌아오지 않는다.