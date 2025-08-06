# Section 05 In Place Resize of Pods

In-place Pod Resizing => 실시간 리소스 재조정
- 기존에는 Deployment/StatefulSet의 리소스를 수정하면 기존 Pod는 제거되고 새로 생기는 방식
- 다운타임 발생

## In-place Pod Resizing
***
- Pod를 삭제하지 않고, 실행 중인 상태에서 리소스를 동적으로 조정하는 기능
- Kubernetes v1.7부터 alpha로 도입
- 현재는 기본적으로 비활성화 상태
- FeatureGate 활성화 필요
  - 클러스터 구성요소에 `--feature-gates=InPlacePodVerticalScaling=true` 추가
- CPU와 메모리에만 적용 가능
  - 메모리 줄이는 것은 안댐
- init/ephemeral 컨테이너는 지원 안댐

### Pod 내 container 리소스 정의
~~~
spec:
  containers:
    - name: app
      image: my-image
      resources:
        requests:
          cpu: "250m"
          memory: "512Mi"
        limits:
          cpu: "500m"
          memory: "1Gi"
      resizePolicy:
        - resourceName: cpu
          restartPolicy: NotRequired
        - resourceName: memory
          restartPolicy: RestartContainer
~~~
- `resizePolicy`: 리소스별 리사이징 시 재시작 여부
- `NotRequired`: 컨테이너 재시작 필요 x
- `RestartContainer`: 컨테이너 재시작 필요