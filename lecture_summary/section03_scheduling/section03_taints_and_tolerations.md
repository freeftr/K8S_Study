# Section 03 Taints and Tolerations

- Taint: 노드의 거부 조건 => 특정 Pod를 받지 않겠다.
  - 노드에 설정
- Toleration: Pod의 허용 조건 => Taint를 무시하고 배치 가능하다.
  - Pod에 설정

## 설정 방법
***

### Taint
- `kubectl taint nodes 적용노드이름 key=value:taint-effect`
- taint-effect: 노드에 taint를 설정했을 때, 해당 taint를 tolerate하지 못하는 Pod를
어떻게 처리할지 설정
  - `NoSchedule`: 이 taint를 tolerate하지 않는 Pod는 절대 이 노드에 스케줄되지 않음
  - `PreferNoSchedule`: 다른 노드가 부족하거나, 조건에 맞지 않으면 이 노드에 배치될 수 있음.
  - `NoExecute`: 새 파드는 스케줄하지 않음 + 기존에 있던 Pod도 수급적용

### Toleration
~~~
apiVersion: v1
kind: pod
metadata:
    name: pod-d
spec:
    containers:
    - name: nginx
      image: nginx
    tolerations: => app=blue:NoSchedule taint가 있어도 배치 가능. 여기에 배치된다는 뜻이 아님.
    - key: app
      operator: Equal
      value: blue
      effect: NoSchedule
~~~

> **Master 노드에 파드가 배치안되는 이유**<br>
> 쿠버네티스 설치 시 마스터 노드에는 자동으로 Taint가 적용됨.