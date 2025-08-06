# Section 05 Configuring ConfigMaps in Applications

Pod가 많아지면 설정 파일 안의 환경 변수를 일일이 관리하기가 힘들어진다.<br>
이를 중앙에서 관리할 수 있도록 도와주는 것이 ConfigMap이다.

## ConfigMap?
***
- 쿠버네티스에서 키-값 형식의 설정 데이터를 전달할 수 있게 해주는 리소스.
- Pod가 생성될 때 Pod 내부로 주입하여 컨테이너 안에서 실행 중인 애플리케이션이 환경 변수 형태로 값을 사용할 수 있음.

## ConfigMap 생성 방법
***

### 명령어로 생성 Imperative
- `kubectl create configmap [이름] --from-literal=키=값`
- 여러 개의 키-값을 넣고 싶으면 `--from-literal`을 여러 번 지정할 수 있음.
~~~
kubectl create configmap app-config \
  --from-literal=APP_COLOR=blue \
  --from-literal=APP_MODE=production
~~~
- 근데 이제 이런 식으로 다 넣기에는 항목이 많아지면 힘들어진다.

### 파일로 생성
- `kubectl create configmap app-config --from-file=path/to/config.txt`

### definition 파일 방식
~~~
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_COLOR: blue
    APP_MODE: production
~~~
작성 후 `kubectl apply -f ~`

## ConfigMap을 Pod에 주입하기
***
- Pod definition 파일에 `envFrom`을 통해 환경 변수를 집어 넣을 수 있음.
~~~
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
spec:
  containers:
  - name: webapp
    image: simple-webapp:1.0
    envFrom:
    - configMapRef:
        name: app-config
~~~