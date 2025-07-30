# Section 03 Labels and Selectors

기본적으로 쿠버네티스 객체를 그룹화, 필터링하는데 사용한다. 

### Labels
각 객체에 key-value 형태로 부착된 메타데이터이다.

### Selector
Selector를 통해 객체에 붙은 Label을 보고 객체를 필터링하고 선택한다.
- `kubectl get pods --selector app=App1`
  - `--selector` 인자를 통해 라벨을 선택

~~~
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: simple-webapp
    labels:    => ReplicaSet의 Label
        app: App1
        function: Front-end
spec:
    replicas: 3
    selector:
        matchLabels:
            app: App1 => ReplicaSet이 선택할 Pod의 Label
    template:
        metadata:
            labels:
                app: App1 => Pod의 Label
                function: Front-end
        spec:
            containers:
            - name: simple-webapp
              image: simple-webapp
~~~

### Annotations
비즈니스 로직과 무관한 메타데이터이 저장 용도다. => 그냥 주석이라고 보면 될듯
- Selector에 사용 불가능하다.