# Section 03 Node Affinity

Node Affinity는 Pod가 어떤 노드에 배치될 수 있는지 정의한다.
- Node Selector는 동등 비교라 하면 Node Affinity는 범위 비교가 가능하다.

~~~
spec:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: size
                    operator: In
                    values:
                    - large
                    - medium
~~~
- operator: 연산자
  - In: 이중에 있으면
  - NotIn: 이중에 없으면
  - Exists: 키가 있으면
  - DoesNotExist: 키가 없으면
- `requiredDuringSchedulingIgnoredDuringExecution`: 조건 만족 못할 시 Pending
- `preferredDuringSchedulingIgnoredDuringExecution`: 조건 맞는 노드 없으면 그냥 다른 노드에 배치

> **실행 중 노드의 레이블이 바뀌면?** <br>
> - `IgnoredDuringExecution`: label이 변경되어도 Pod는 계속 실행
> - `RequiredDuringExecution`: label을 만족하지 않으면 퇴출