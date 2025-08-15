# Section 07 RBAC

RBAC: 역할별 권한을 검증
- apiGroups: API 그룹
- resources: 권한 부여 대상 리소스
- verbs: 수행 가능한 동작


### 1. Role 생성
~~~
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "create", "delete"]
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
~~~

### 2. Role Binding
- 사용자와 Role 연결
~~~
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
  namespace: default
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
~~~


- Role과 RoleBinding은 네임스페이스 범위에서만 유효하다.

### 권한 확인
- `kubectl auth can-i create pods`: 현재 사용자 권한 확인
- `kubectl auth can-i create pods`: 특정 네임스페이스에서 확인
- `kubectl auth can-i create deployments --as dev-user`: 다른 사용자로 시뮬레이션