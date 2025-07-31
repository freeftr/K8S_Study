# Section 03 DaemonSets

DaemonSet은 모든 노드에 하나의 Pod 복사본을 자동으로 배포하고 유지하는 역할을 합니다.
- 모든 노드에 정확히 하나의 Pod를 보장
- 모니터링 에이전트, 로그 수집기, kube-proxy등을 배포하는데 유용

기본 스케줄러와 Node Affinity를 사용하여 동작한다.

### definition
~~~
apiVersion: apps/v1
kind: DaemonSet
metadata:
    name: monitoriing-daemon
spec:
    selector:
        matchLabels:
            name: monitoring-agent
    template:
        metadata:
            labels:
                name: monitoring-agent
        spec:
            containers:
            - name: agent
              image: monitoring-agent:latest
~~~