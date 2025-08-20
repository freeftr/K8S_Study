# Section 07 Custom Resource Definition

- Resource: 쿠버네티스에 정의된 객체
  - ETCD에 저장
  - `kubectl get`/`kubectl create`으로 관리하는 객체들.
- Controller: 해당 리소스를 감시하고 클러스터를 조정하는 프로세스.

### Custom Resource
- 사용자가 정의한 리소스
~~~
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Mumbai
  to: London
  number: 2
~~~
- ETCD에 저장되지만, Controller가 이를 관리하지 못해 Custom Controller를 만들어야 함.

### Custom Controller
- Custom Resource 생성/삭제 이벤트 감시
- 외부 API 호출 등 실질적 작업 수행

### Custom Resource Definition
- 쿠버네티스에 Custom Resource의 존재를 알려주는 것.
~~~
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: flighttickets.flights.com
spec:
  group: flights.com
  names:
    kind: FlightTicket
    plural: flighttickets
    singular: flightticket
    shortNames:
    - ft
  scope: Namespaced   # 또는 Cluster
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              from:
                type: string
              to:
                type: string
              number:
                type: integer
                minimum: 1
                maximum: 10
~~~
- `group`: CR의 `apiVersion`에 포함된다.
- `names`: kind, plural, singular, shortNames 정의
- `scope`: Namespaced or Cluster
- `versions`

### 생성 흐름
1. CRD 생성
2. CR 생성
   - ETCD에 저장
3. Custom Controller 배포
4. Controller가 외부 API 호출 작업 실행

*CRD는 쿠버네티스 API를 확장, Controller는 해당 리소스에 행동을 부여
이러한 방식을 **Operator Pattern**이라고 한다.