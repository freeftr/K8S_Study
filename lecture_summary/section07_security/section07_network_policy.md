# Section 07 Network Policy

- Ingress: 외부에서 들어오는 트래픽
- Egress: 외부로 나가는 트래픽

기본적으로 쿠버네티스에서는 모든 Pod 간 통신이 허용된 상태이다. 하지만 때에 따라서는 
제한을 해야하는 경우가 있고, 이때 **NetworkPolicy**를 사용할 수 있다.

~~~
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: api
    ports:
    - protocol: TCP
      port: 3306
~~~
- `policyTypes`: Ingress인지, 
  - 반드시 지정해야 차단효과가 생긴다.
- `podSelector`: 어떤 Pod 선택할 지

---

### Network Policy 작성 순서
1. PodSelector
   - `podSelector`로 같은 라벨을 지정
   - 모든 트래픽 차단
2. PolicyTypes
   - `Ingress`만 정의
   - 외부 요청을 받는 방향 제어
3. Ingress Rules
   - `from`: 라벨 지정
   - `ports`: 통신 방식, 포트 번호 지정


### Selector 종류
- PodSelector
~~~
from:
- podSelector:
    matchLabels:
      role: api
~~~
- NamespaceSelector
~~~
from:
- namespaceSelector:
  matchLabels:
  env: prod
~~~
- Pod + Namespace
~~~
from:
- podSelector:
    matchLabels:
      role: api
  namespaceSelector:
    matchLabels:
      env: prod
~~~
- ipBlock
~~~
from:
- ipBlock:
    cidr: 192.168.5.10/32
~~~