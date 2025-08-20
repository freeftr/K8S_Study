# Section 07 KubeConfig

### KubeConfig란?
- `kubectl`로 접근할 때, 어떤 클러스터에 어떤 사용자 인증 정보로, 어떤 네임스페이스를 기본으로 접근할지 정의한 설정 파일
- `~/.kube/config` 기본 위치
- `kubectl --kubeconfig=/path/to/config`로 위치 지정 가능

## 핵심 섹션
### Clusters
- 접근 가능한 Kubernetes 클러스터 목록
- name
- server
- certificate-authority
~~~
clusters:
- name: my-kube-playground
  cluster:
    server: https://1.2.3.4:6443
    certificate-authority: /path/to/ca.crt
~~~

### Users
- 각 클러스터에 접근할 사용자 계정과 인증 정보
~~~
users:
- name: my-kube-admin
  user:
    client-certificate: /path/to/admin.crt
    client-key: /path/to/admin.key
~~~

### Contexts
- 특정 cluster와 user를 매핑하고, 선택적으로 기본 namespace 지정
~~~
contexts:
- name: admin@playground
  context:
    cluster: my-kube-playground
    user: my-kube-admin
    namespace: dev
~~~

### Commands
- `kubectl config view`: 현재 설정 보기
- `kubectl config use-context my-context`: context 전환
- `kubectl config set-context~`: 생성
- `kubectl config delete-context`: 삭제

### 인증서 지정 방식
- 파일 경로 방식: `certificate-authority: /path/to/ca.crt`
- Base64 인코딩 데이터 직접 포함: `certificate-authority-data: <BASE64_STRING>`