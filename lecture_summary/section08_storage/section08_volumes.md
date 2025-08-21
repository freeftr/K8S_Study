# Section 08 Volumes

### Volume이 필요한 이유
=> Pod가 영속적이지 않기 때문.
- 삭제 시 데이터도 같이 삭제

---
## Storage Options
- k8s는 다양한 스토리지를 volume에 연결할 수 있음.

### HostPath
- 호스트의 특정 디렉토리를 Pod 마운트
- `/data` -> `/opt`
- 멀티 노드에서는 모두 `/data`를 사용한다. (실질적으로는 다르지만) => 단일 노드에서만 써라.

### 네트워크/클러스터 스토리지
- 멀티 노드 환경을 지원하기 위한 외부 스토리지.
- NFS, CephFS, GlusterFS, Flocker 등
- 노드가 달라도 동일한 스토리지를 바라봄으로 데이터 정합성 유지