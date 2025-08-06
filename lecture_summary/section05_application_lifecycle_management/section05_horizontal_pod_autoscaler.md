# Section 05 Horizontal Pod Autoscaler

HPA는 CPU, 메모리 또는 메트릭을 기준으로 Pod 개수를 자동으로 조절하는 리소스이다.

### Manual
1. `kubectl top pod`으로 모니터링
2. `kubectl scale`로 조절
=> 실시간 대응 불가

### HPA
- 메트릭 서버로부터 사용량 주기적 수집
- 설정된 threshold에 따라 조절

### HPA - Imperative
- `kubectl auto scale deployment my-app --cpu-percent=50 --min=1 --max=10`으로 설정
- `kubectl get hpa`로 확인

### HPA - Declarative
~~~apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
~~~

## Metrics
***

### Custom Metrics
- 내부 시스템 지표
- `custom-metrics-adapter`

### External Metrics
- 외부 지표
- Datadog, Prometheus
- `external-metrics-adapter`