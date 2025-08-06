# Section 05 Secrets

민감정보를 ConfigMap으로 옮기는 것은 보안상 위험하다. 그러면 어떻게 해야할까?

이럴 때 사용하는 것이 Secret이다.

## Secret
***
Secret은 비밀번호나 인증 키 같은 민감 정보를 저장하기 위한 리소스이다.
- base64로 인코딩 해서 저장

## Secret 생성 방법
***

### Imperative
- `kubectl create secret generic [시크릿이름] --from-literal=KEY=VALUE`
- 여러 개의 키-값을 넣고 싶으면 `--from-literal`을 여러 번 지정할 수 있음.

### 파일
- `kubectl create secret generic app-secret --from-file=./my-secret.txt

### Declarative  - definition
~~~
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_USER: YWRtaW4=
  DB_PASS: MTIzNDU=
~~~
- data에 들어가는 값은 반드시 base64로 인코딩된 값이어야 한다.


## Pod에 Secret 주입
***

### 환경 변수로 주입
~~~
envFrom:
- secretRef:
    name: app-secret
~~~

### 특정 key만 주입
~~~
env:
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: app-secret
      key: DB_USER
~~~

### 볼륨으로 주입
~~~
volumes:
- name: secret-volume
  secret:
    secretName: app-secret

containers:
- name: app
  ...
  volumeMounts:
  - name: secret-volume
    mountPath: /etc/secrets
~~~

## 주의사항
***
Secret은 인코됭된 것이지 암호화가 아니다.
- 깃헙에 올리지 마세요.

etcd에 저장되는 시크릿도 암호화되어 있지 않다. => at rest encryption 설정 필요

### at-rest-encryption 설정 방법
1. EncryptionConfiguration 파일을 생성.
2. kube-api server에 해당 설정을 인자로 넘김

또한 같은 네임스페이스에서 Pod나 Deployment 생성 권한을 가진 사용자는 Secret을 마음대로 참조할 수 있다.
- 권한 설정 필요.