# Section 05 Encrypting Secret Data at Rest

쿠버네티스는 기본적으로 Base64 인코딩만 되어 있습니다.<br>
=> 보안에 주의해야 함.

이는 etcd에 저장될 때도 평문으로 저장되어 etcd 접근 권한이 있으면 secret 내용을 볼 수 있다.

## 암호화 설정
***

### 1. 암호화 설정 확인
- kube-apiserver 프로세스에 `--encyption-provider-config` 옵션이 있는지 확인
~~~
ps aux | grep kube-apiserver
~~~

### 2. 암호화 설정 파일 생성 - enc.yaml
~~~
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <Base64로 인코딩된 32바이트 키>
      - identity: {}
~~~
- `aescbc`: CBC 방식의 AES 알고리즘
- `indentity`: 암호화 안함 (복호화용 백업)
- `head -c 32 /dev/urandom | base64`로 키 생성

### 3. 특정 디렉토리에 enc.yaml 보관

### 4. kube-apiserver에 옵션 추가
- `/etc/kubernetes/manifests/kube-apiserver.yaml`에 `- --encryption-provider-config=/etc/kubernetes/enc/enc.yaml
` 추가
- `volumeMounts`, `volumes`에 해당 디렉토리 mount 지정도 추가 필요

