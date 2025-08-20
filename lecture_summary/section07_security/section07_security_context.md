# Section 07 Security Context


### Docker Security
- 컨테이너는 호스트와 같은 커널을 공유하지만, 네임스페이스를 통해 격리됨.
- 기본적을 컨테이너는 root 사용자로 실행
  - `docker run --user <uid>`를 통해 사용자 아이디 지정 가능
- root와 호스트는 동일하지 않음.
  - root는 Docker가 Linux Capabilities를 통해 권한을 제한당함.

### Container Security
- Pod 또는 컨테이너 단위에서 보안 관련 설정을 정의하는 필드
- Pod와 Container 모두 정의되어 있으면 Container 설정이 우선이다.

- Pod
~~~
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000        # UID 1000으로 실행
    runAsGroup: 3000       # GID 3000으로 실행
    fsGroup: 2000          # 볼륨 마운트 시 권한 그룹
  containers:
  - name: ubuntu
    image: ubuntu
    command: ["sleep", "3600"]
~~~

- Container
~~~
apiVersion: v1
kind: Pod
metadata:
  name: security-context-container-demo
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command: ["sleep", "3600"]
    securityContext:
      runAsUser: 2000
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]   # 네트워크 및 시스템 시간 권한 추가
        drop: ["KILL", "MKNOD"]          # 불필요한 권한 제거
~~~

- `runAsUser`/`runAsGroup`: 특정 UID GID로 실행
- `runAsNonRoot: true`: 반드시 root로 실행
- `fsGroup`: Pod의 볼륨에 접근할 그룹 ID 지정
- `privileged: true`: 컨테이너에 모든 권한 부여