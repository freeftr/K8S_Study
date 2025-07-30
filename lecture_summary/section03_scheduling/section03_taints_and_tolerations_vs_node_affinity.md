# Section 03 Taints and Tolerations vs Node Affinity

Taint와 Tolerations과 Node Affinity를 각각 사용하면 Pod와 노드에 대한
양방향 제어가 되지 않는다. 
- 같이 사용하면 양방향 제어 가능.

예를 들어 우리의 노드에 taint를 설정한다.
- 우리 노드에 들어오는 Pod들 제어 가능
- 우리 Pod가 집을 나가는 것은 제어 불가능

반대로 node affinity만 설정한다.
- 우리 Pod는 지정된 노드에만 배치할 수 있다.
- 근데 다른 Pod가 우리 집에 올 수 있다.

그럼 다음과 같이 하면 내 노드에는 내 Pod만 올 수 있게 설정 가능하다.
- deployment 설정
~~~
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
            - key: color
              operator: In
              values:
              - blue
  tolerations:
  - key: "color"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
~~~

- 노드 설정
~~~
kubectl label nodes node-blue color=blue
kubectl taint nodes node-blue color=blue:NoSchedule
~~~

Taint, Toleration, Node Affinity를 같이 설정함으로서 내 Pod만 내 노드에 오게 보장할 수 있다.