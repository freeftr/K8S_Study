# Section 03 Multiple Schedulers

기본 스케줄러로도 요구사항을 만족하지 못할 때? -> 커스텀 스케줄러를 만들 수 있음.
- 직접 스케줄링 알고리즘을 구현해서 기본 스케줄러로 배치할 수도 있음.

### 스케줄러 배포
1. 스케줄러 이름 설정해서 생성
- 기본은 default-scheduler
2. Pod 또는 Deployment로 스케줄러 실행.
- `--config`: 커스텀 스케줄러의 설정파일로서 Pod-definition에 지정해야 함.
- `leaderElection`: 여러 노드에서 동일한 스케줄러를 실행할 경우 하나만 활성화되어야 한다.
- `--kubeconfig`: API 서버에 접근하기 위한 인증 정보

설정파일을 전달하는 방식은 두 가지가 있다.
- ConfigMap: 커스텀 스케줄러 설정 파일을 ConfigMap으로 생성
  - 이걸 Pod의 볼륨으로 마운트
- Volume: 볼륨으로 직접 마운트

### 명령어
- `kubectl get pods -n kube-system`: 커스텀 스케줄러 pod 확인
- `kubectl get events -o wide`: 어떤 스케줄러가 Pod를 스케줄링 했는지 확인
- `kubectl logs <scheduler-pod> -n kube-system`: 스케줄러 로그 확인