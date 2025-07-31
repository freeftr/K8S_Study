# Section 03 Resource Limits

- Request: 컨테이너가 최소한으로 보장받을 CPU 및 메모리 양
  - 스케줄러는 request를 기준으로 스케줄링함.
- Limit: 컨테이너가 최대로 사용할 수 있는 CPU 및 메모리 양
  - limit을 초과하게 되면 pod를 다운 시킨다.

### CPU, 메모리 표현 방식
- CPU: 1CPU = 1 vCPU
- Memory
  - MI(Mebibyte, 1024) GI(Gibibyte)
  - M(Megabyte, 1024) G(Gigabyte)

### definition 설정
~~~
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      resources:
        requests:
            memory: "1Gi"
            cpu:
        limits:
            memory: "2Gi"
            cpu: 2
~~~

### LimitRange
- Namespace 단위로 기본 request/limit을 설정
~~~
apiVersion: v1
kind: LimitRange
metadata:
    name: cpu-resource-constraint
spec:
    limits:
    - default:
        cpu: 500m
      defaultRequest:
        cpu:
      max:
        cpu: "1"
      min:
        cpu: 100m
      type: Container
~~~

Request는 리소스가 증가하게 되면 문제가 생기기 때문에 반드시 지정해야 한다. 하지만,
limits는 꼭 필요한 경우에만 지정해야 유연성 측면에서 추가적인 리소스가 필요시 컨테이너 내
잉여 리소스를 받을 수 있기 때문에 좋다.