# Section 03 Priority Classes

Pod에 우선순위를 지정할 수 있게 해주는 역할
- 우선순위 숫자 부여 -> 클수록 우선
- Non-namespace 리소스
- 사용자 정의: -2,000,000,000 ~ + 1,000,000,000
- 시스템: +2,000,000,000 근삿값

### definition
~~~
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
    name: high-priority
value: 1000
globalDefault: false
preemptionPolicy: PreemptLowerPriority
description: "This priority class is for critical workloads."
~~~

- Preemption Policy
  - `PreemptLowerPriority`: 기존 낮은 우선순위 Pod를 제거하고 자신이 스케줄
  - `Never`: 기다림, 다른 Pod 제거하지 않음