# Section 02 Kube Scheduler
Pod를 적절한 노드에 배치하는 역할을 한다.
- 실제로 Pod를 실행하지는 않음.

### Scheduler가 필요한 이유?
Pod가 많고 노드가 많다면 이를 적절하게 배치해야 효율적으로 사용이 가능하다.
- 컨테이너마다 요구하는 CPU, 메모리 자원이 다르다.
- 어떤 노드는 어떤 애플리케이션 전용일수도

### 과정
1. Filter
   - Pod의 요구 조건 충족 확인 (CPU, 메모리 등)
2. Rank
   - 남은 노드들에 점수를 매긴다. 0~10점
   - Pod를 배치했을 때 더 많은 자원인 남으면 높은 점수.
   - 가장 점수가 높은 노드에 Pod 배치
   
### 설치 방법
1. 바이너리 다운로드
~~~
wget https://storage.googleapis.com/kubernetes-release/release/v1.27.0/bin/linux/amd64/kube-scheduler
~~~

실행 시 `--config` 옵션을 통해 스케줄링 설정 파일을 지정 가능하다.