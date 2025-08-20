# Section 07 TLS

### TLS 인증서가 필요한 이유
- 사용자와 서버간 통신이 안전하다고 보장.
- 평문으로 민감정보를 보내면 해커가 패킷 스니핑으로 탈취 가능

=> 통신 암호화 + 서버 신원 보장

## 암호화 방식
***
### Symmetric Encryption
- 하나의 키로 암호화, 복호화 수행
- 네트워크로 전송되는 키 탈취 위험성 있음.

### Asymmetric Encryption
- 서로 다른 두 개의 키 사용
  - Private 키: 외부에 공개 안함. 복호화에 사용. 
  - Public 키: 누구나 접근 가능. 암호화에 사용.


1. `ssh-keygen` 으로 공개키, 개인 키 생성
2. `~/.ssh/authorized_keys`에 공개키 등록
3. 클라이언트는 개인키로만 인증 가능
4. 동일 개인키로 여러 서버 접속 가능, 다른 사용자는 본인 키 쌍으로 접근

여기서 문제는 해커가 동일한 웹사이트를 만들어서 본인 서버의 키로 TLS를 적용
<br> CA로 해결

### CA
- 인증서에 서버 정보와 공개키를 포함하고, CA가 서명.
- 인증서 발급 절차:
1. 서버에서 CSR 생성
2. CA에 제출, CA가 도메인 소유 여부 검증
3. CA가 개인키로 서명한 인증서를 발급
4. 서버에 설치, 브라우저는 CA의 공개키로 인증서 유효성 검증

- 브라우저는 신뢰할 수 있는 CA 목록의 공개키를 내장.
- 서버 인증서의 서명을 CA 공개키로 검증

### 양방향 TLS
- 일반 웹에서는 서버만 인증
- 때에 따라서는 클라이언트도 인증서를 가질 수 있다.
- 서버가 클라이언트의 인증서를 검증하여 사용자가 맞는지 검증.

### PKI
- 공개키 기반의 전체 관리 체계
- CA, 인증서 발급/폐기 절차, 키 관리 정책, 사용자/서버

***

## 쿠버네티스에서의 TLS
- 쿠버네티스 통신은 TLS를 사용하여 암호화되며, 3가지 인증서 유형이 있음.
  - Server Certificates
    - 서버가 클라이언트에 자신을 증명할 때 사용
  - Client Certificates
    - 클라이언트가 서버에 자신을 증명할 때 사용
  - Root Certificates
    - CA 자체 인증서
    - 모든 다른 인증서 서명에 사용

## 컴포넌트와 인증서 매핑
### 서버
- kube-api server
  - apiserver.crt
  - apiserver.key
- etcd
  - etcdserver.crt
  - etcdserver.key
- kubelet
  - kubelet.crt
  - kubelet.key

### 클라이언트
- admin
  - admin.crt
  - admin.key
- scheduler
  - scheduler.crt
  - scheduler.key
- controller-manager
  - controller-manager.crt
  - controller-manager.key
- kube-proxy
  - kube-proxy.crt
  - kube-proxy.key
  
### 컴포넌트간 관계
- Kube-api server는 etcd의 클라이언트 역할
  - 인증 시 동일 키 재사용 가능
- kubelet와도 마찬가지

### 인증서 발급 구조
- 각 서버 클라이언트에서 CSR 생성
- CA가 CSR 서명
- 서명된 인증서를 컴포넌트에 배포

***
## Certificate 생성

### 인증서 생성 툴
- OpenSSl
- Easy-RSA
- CFSSL

### CA 인증서 생성
1. CA 개인키 생성
~~~
openssl genrsa -out ca.key 2048
~~~
2. CSR 생성
~~~
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
~~~
3. 자체 서명 루트 인증서 생성

### 클라이언트 인증서 생성 - Admin
1. 개인키 생성
~~~
openssl genrsa -out admin.key 2048
~~~
2. CSR 생성
~~~
openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr
~~~
3. Sign Certificate 생성
~~~
openssl x509 -req -in admin.csr -CA ca.crt -C
~~~
- group detail을 CSR을 추가함으로써 admin을 구분할 수 있다.

### 클라이언트 인증 생성 - 시스템 컴포넌트
- 앞에 prefix "system:"이 붙어야 함.

## 서버 인증서 생성

### etcd 서버
- HA 구성 시 Peer 인증서 별도로 발급

### kube-apiserver
- CN: kube-apiserver
- Alt Name 필요:
  - DNS: kubernetes, kubernetes.default, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local
  - API 서버 IP 주소
- CSR 생성 시 OpenSSL 설정 파일 섹션에 위 이름과 IP 추가

### kubelet 서버
- 각 노드별 인증서

### CA 루트 인증서 배포
- 모든 서버와 클라이언트는 CA의 공개 인증서를 보유해야 상호 검증 가능하다.

***
# Certificate Details
- 인증서 관련 문제가 생기면 클러스터 인증서 헬스 체크가 필요하다.
  - 수동 구축: 직접 생성, 배포한 인증서를 OS 레벨 서비스 설정에서 확인
  - kubeadm: 인증서를 kubeadm이 자동 생성, 컴포넌트를 Pod로 배포

## 점검 절차

### 1. 인증서 목록화
- 각 컴포넌트에서 사용하는 인증서 파일 경로, 이름, CN, SAN, OU, 발급자, 만료일 등을 리스트업
- `/etc/kubernetes/manifests/kube-apiserver.yaml` 여기서 인증서 경로 확인

### 2. 인증서 상세 정보 확인
~~~
openssl x509 -in <cert-file> -text -noout
~~~
- Subject: CN
- Subject Alternative Names: DNS, IP 모든 별칭 포함 여부
- Validity: 유효 기간 및 만료일 확인
- Issuer: 올바른 CA로부터 발급되었는지 확인
- Organization: 올바른 그룹 소속 여부 확인

### 3. 유효성 체크CN과 SAN이 클러스터 요구사항과 일치하는지
- OU에 맞는 권한 그룹이 설정되어 있는지
- 발급자가 올바른 루트 CA인지
- 인증서가 만료되지 않았는지
- Kubernetes 문서에 정의된 인증서 요구사항 준수 여부

