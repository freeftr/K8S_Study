# Section 05 Vertical Pod Autoscaling

VPA는 Pod의 리소스를 자동으로 조정하여 over/under provisioning을 방지하고,
워크로드 성능을 최적화한다.
- 기존 Pod 재생성 방식
- 리소스 사용량을 기본으로 리소스 요청/제한을 자동 조정
- 기본 쿠버네티스 빌트 인이 아니기 때문에 따로 배포해야 함.
  - `kubectl apply -f https://github.com/kubernetes/autoscaler/releases/download/vertical-pod-autoscaler-<VERSION>/vpa-up.yaml`
  - `kubectl get pods -n kube-system | grep vpa`

### VPA 컴포넌트
- Recommender: Pod의 리소스 사용량을 메트릭 서버에서 수집하여 추천 값 계산
- Updater: 리소스 설정이 비효율적인 Pod를 감지하고 삭제한다.
- Admission Controller: 새로 생성되는 Pod의 리소스 요청/제한을 추천값으로 변경하여 적용한다.

### VPA definition
~~~
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: my-app-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  updatePolicy:
    updateMode: "Auto"
  resourcePolicy:
    containerPolicies:
      - containerName: "*"
        minAllowed:
          cpu: "250m"
          memory: "512Mi"
        maxAllowed:
          cpu: "2"
          memory: "2Gi"
~~~
- `updatePolicy`
  - `Off`: 리소스 추천만 받음.
  - `Inital`: 신규 pod 생성 시에만 리소스 자동 적용
  - `Recreate`: 기존 Pod 재생성
  - `Auto`: 현재는 Recreate In-place resizing 활성화 시 지원

- `kubectl describe vpa my-app-vpa`: 추천값 확인하는 방법