# Section 02 Pod with YAML

쿠버네티스는 yaml 파일을 이용해 객체를 생성한다. 그리고 yaml 파일은 항상 다음 최상위 4가지 필드로 시작한다.
~~~
apiVersion: 
// 쿠버네티스 API 버전
kind:
// 생성할 리소스의 종류
metadata:
// 리소스 이름, 라벨 등 메타데이터

spec:
// 해당 리소스의 실제 설정 내용, 컨테이너 정보
~~~

### apiVersion
- Pod는 항상 v1
- apps/v1

### kind
- Service, Pod, Deployment등

### metadata
- labes: 키-값 형태로 자유롭게 지정 가능

### spec
- containers: 배열로 정의
- image: 사용할 Docker 이미지

### 생성 명령어
~~~
kubectl create -f pod-definition.yaml
~~~
### 상세 정보 확인
~~~
kubectl describe pod my-app-pod
~~~

### 전체 흐름
~~~
nano test-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: testpod
spec:
  containers:
    - name: test
      image: redis
      
kubectl apply -f test-pod.yaml
kubectl get pods
kubectl describe pod testpod
~~~