# Section 02 Namespaces

네임스페이스는 다음과 같은 기능을 가진다.
- 리소스 구분
- 접근 제어
- 리소스 제한: Quota를 통해 CPU, 메모리 제한

## 기본 Namespace
***
### default
사용자가 기본적으로 작업하는 공간

### kube-system
쿠버네티스 내부 리소스용 네임스페이스

### kube-public
모든 사용자에게 열려 있는 네임스페이스

## DNS
***
~~~
db-service.dev.svc.cluster.local
~~~
- cluster.local: 클러스터의 기본 도메인
- db-service: 서비스 이름
- dev: 네임스페이스
- svc: 서비스

## 명령어
***
- `kubectl get pods --namespace=kube-system`: kube-system의 pod 조회
- `--namespace` 통해 네임스페이스 지정 가능
- definition안에도 `namespace:` 필드로 지정 가능

### 생성
1. definition
~~~
apiVersion: v1
kind: Namespace
metadata:
    name: dev
spec:
//Resource Quota
    hard:
        pods: "10"
        requests.cpu: "4"
        requests.memory: 5Gi
        limits.cpu: "10"
        limits.memory: 10Gi
~~~

또는 `kubectl create namespace dev`

기본적으로는 default 네임스페이스를 바라본다. 네임스페이스를 바꾸고 싶다면 <br>
`kubectl config set-context $(kubectl config current-context) --namespace=원하는 네임스페이스`

`kubectl get pods --all-namespaces`모든 네임스페이스 pod 조회