# Section 07 API Groups

모든 Kubernetes 조작은 Kube Api Server를 통한다.
- `kubectl`로 접근

Kubernetes API는 기능별 그룹으로 나뉨

### Core API Group
- v1
  - pods
  - namespaces
  - replicationcontrollers
  - services
  - persistentvolumes, persisentvolumeclaims
  - configmaps
  - secrets
  - nodes
  - events

### Named API Groups
- apps
- networking.k8s.io
- storage.k8s.io
- certificates.k8s.io
- authentication.k8s.io