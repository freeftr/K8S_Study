# ETCD
Etcd는 단순, 안전, 빠른 분산형 키-값 저장소이다.

## ETCD 설치
***

### 1단계 Etcd 바이너리 다운로드
~~~
curl -LO https://github.com/etcd-io/etcd/releases/download/v3.5.13/etcd-v3.5.13-darwin-amd64.zip
~~~

### 2단계 압축 해제
~~~
tar xzvf etcd-v3.3.11-linux-amd64.tar.gz
~~~

### 3단계 실행
~~~
./etcd
~~~

etcd를 실행하면 2379번 포트를 리스닝한다.
ETCDCTL은 etcd 기본 cli 클라이언트로서 키-값 저장, 조회가 가능하다. 

## ETCD in Kubernetes
***
ETCD는 클러스터에 관한 정보를 저장합니다.
- 노드
- pod
- configMap
- secret
- accounts
- roles
- role binding

### ETCD 배포
ETCD는 클러스터 설정에 따라 두 가지로 배포된다.
- scratch
- kubeadm

### Scratch
수동으로 구성할 경우 위와 같이 바이너리 파일을 받은 후 마스터 노드에 직접 설치해야 한다.
- 서버 IP주소, 2379 포트 사용
~~~
wget -q --https-only \
"https://github.com/etcd-io/etcd/releases/download/v3.5.11/etcd-v3.5.11-linux-amd64.tar.gz"
~~~
- 주의 옵션: `advertise-client-urls`

### Kubeadm
kubeadm을 사용해 클러스터를 구성했으면, ETCD는 자동으로 Pod 형태로 kube-system 네임스페이스에 생성된다.
etcdctl을 통해 Pod 내부에서 접근이 가능하다.

### HA 구성 시 주의점
클러스터에 HA 구성을 적용하면 ETCD도 여러 인스턴스로 분산된다. 이때 각 ETCD 인스턴스들이 서로를 인식할 수 있게
하는 것이 중요하다.
- etcd 서비스 설정에 `--inital-cluster` 설정 필요.