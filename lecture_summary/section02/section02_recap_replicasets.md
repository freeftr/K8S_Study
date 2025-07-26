# Section 02 Recap ReplicaSets

고가용성을 제공하기 위해 여러 인스턴스를 scale-out해야 하는데, Replication Controller가 이를 돕는다.
- 특정 수의 인스턴스를 항상 유지함.
- 그리고 이 작업은 단일 노드뿐만 아니라 여러 노드에 걸쳐 수행될 수 있음

## Replication Controller vs Replica Set
***
### Replication Controller
1. definition 파일 만들기
~~~
apiVersion: v1
kind: ReplicationController
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end
spec:
    template: //어떤 Pod를 만들지 지정
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end
        spec:
            containers:
            - name: nginx-container
              image: nginx
    replicas: 3 // 원하는 Pod 수
~~~
2. ReplicationController 생성
    ~~~
   kubectl create -f yml_파일_이름
   ~~~
3. 생성 확인
   ~~~
   kubectl get replicationcontroller
   ~~~
   
### ReplicaSet
1. definition 파일 생성
- apps/v1임을 주의하라.
- selectors를 통해 이미 생성된 Pod들을 관리할 수 있음.
~~~
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: myapp-replicaset
    labels:
        app: myapp
        type: front-end
spec:
    template:
        
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                tpye: front-end
        spec:
            containers:
            - name: nginx-container
              image: nginx
    replicas: 3
    selector:
        matchLabels:
            type: front-end
~~~
2. 생성
~~~
kubectl create -f yaml_파일_이름
~~~
3. 생성 확인
~~~
kubectl get replicaset
~~~

## Labels and Selectors
***
ReplicaSet은 label을 통해 Pod를 모니터하고 필요한 조치를 취한다. Label을 통해 조건에 맞는 Pod들을 감시한다.
Template이 필요한 이유는 새로운 Pod를 만들어야 할 상황이 존재하기 때문이다.

### Scale
1. `replicas` 필드 수정
2. 적용
~~~
kubectl replace -f 파일_이름
~~~

또는

명령어로 바로 적용
~~~
kubectl scale --replicas=6 -f 파일_이름
~~~
근데 이렇게 하면 파일 안에 설정은 3으로 작성된 상태가 유지된다. 6으로 적용되기는 함.

*`-f` 인풋 파일 뜻

## 명령어들
~~~
kubectl create -f 파일_이름
kubectl get replicaset
kubectl delete replicaset 레플리카셋_이름
kubectl replace -f 파일_이름
kubectl scale --replicas=6 -f 파일_이름
~~~