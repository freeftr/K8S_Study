# Section 03 Node Selectors

Node Selector는 Pod를 특정 label이 붙은 노드에 스케줄링한다.

1. 노드에 label 붙이기
   - `kubectl label nodes node1 size=large`
2. Pod 정의에 nodeSelector 추가하기
~~~
apiVersion: v1
kind: Pod
metadata:
    name: test-pod
spec:
    containers:
    - name: processor
      image: my-test-pod:latest
    nodeSelector:
      size: large
~~~

이때 Node Selector에 맞는 노드가 하나도 없다면 Pod는 Pending 상태가 된다.
- key-value가 정확히 일치해야함.