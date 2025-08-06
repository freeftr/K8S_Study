# Section 05 Multi container Pods Design Pattern

## Kubernetes Multi Container Pod 패턴
***

### Co-located Containers
- 여러 컨테이너를 동일한 Pod 안에 배치
- 컨테이너가 서로 의존할 때 사용

### Init Containers
- 메인 컨테이너가 시작될 초기화 작업이 필요할 때 사용
- Init 컨테이너가 초기화 작업을 수행함.
- Init은 작업 후 종료되고 메인 컨테이너가 실행된다. 

### Sidecar Container
- 메인 컨테이너와 함께 실행되며 이를 돕는 보조 컨테이너
- 로그 수집, 프록시, 모니터링등 목적을 가진다.
- Pod의 생명주기를 함께함 (Init과 다른점)

### Co-located vs Sidecar
- Co-located는 시작 순서 제어가 불가능하다.
- Co-located는 단순 병렬 실행이다.

