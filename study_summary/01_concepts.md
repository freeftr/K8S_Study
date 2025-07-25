# 개념

### Pod
쿠버네티스의 가장 작은 배포 단위이다. 하나 이상의 컨테이너를 포함한다.
- 특징
  - Pod 안의 모든 컨테이너는 같은 IP 주소와 포트 네임스페이스를 공유한다.
    - localhost로 통신 가능
  - Pod 내 컨테이너간 디스크 볼륨을 공유할 수 있다.
    - 다른 컨테이너의 파일을 읽을 수 있기 때문에 로그도 읽을 수 있다.
  - Sidecar 패턴: 주 컨테이너를 하나 두고 여기서 사용하는 보조 프로그램들을 보조 컨테이너로 배포하는 패턴.
~~~
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
    - name: app-container
      image: nginx
    - name: log-agent
      image: fluentd
~~~

### Volume
컨테이너가 배포될 때 각자의 로컬 디스크를 생성한다. 근데 이 로컬 디스크들은 영구적이지 못해 따로 영속화를 해줘야 한다.
그리고 이를 저장하는 공간이 볼륨이다. 예를 들어, 웹 서버를 배포하는 Pod가 있다고 가정할 때, 웹 서버 컨테이너, 로그 수집
컨테이너가 있다고 한다. 웹 서버에서 발생하는 로그를 공유하기 위해서는 볼륨을 이용한다(사이드카 패턴).
- 특징
  - Pod 내 컨테이너간 공유가 가능하다.
- 종류
  - `emptydir`
    - Pod와 생명주기 공유 (영속화 안댐).
    - 비어있는 디렉토리 생성.
    ~~~
    volumes:
        - name: cache-volume
        emptyDir: {}
    ~~~
  - `hostPath`
    - 노드의 파일 시스템 경로를 마운트
    ~~~
    volumes:
        - name: host-volume
        hostPath:
            path: /data
    ~~~
  - `configMap` / `secret`
    - 환경설정 파일이나 비밀번호, 인증키 등을 마운트
    - read-only로 주요 사용
    ~~~
    volumes:
        - name: config-volume
        configMap:
            name: my-config
    ~~~
  - `persistentVolumeClaim`
    - 클러스터 외부 저장소에 연결하여 Pod 재시작에도 데이터 유지
    ~~~
    volumes:
        - name: data-volume
        persistentVolumeClaim:
            claimName: my-pvc
    ~~~
    
### Service
Pod가 묶여 하나의 서비스로 제공된다. 여러 개의 Pod를 서비스하면서 로드밸런서를 통해 하나의 IP와 포트로 묶어준다.
Pod는 동적으로 생성되고 사라지기 때문에 IP가 자주 바뀐다. 그렇기 때문에 서비스를 통해 고정된 IP를 통해 받아주는 것이다.
- Label Selector: 어떤 Pod들과 연결할지 결정.
- Label: Pod를 생성할 때 메타데이터에 라벨을 지정하는데 특정 라벨을 가지고 있는 Pod만 Label Selector가 가져간다.
  - 나 좀 여기로 데려가쇼 같은 느낌.

### Name space
클러스터 내에서 리소스를 가상으로 분리하는 논리적 공간이다. 하나의 클러스터 내 사용자 권한을 분리할 수 있고(개발, 운영,
테스트), 여러 팀이 한 클러스터를 공유할 때 격리하는 데에도 쓰인다.

### Label
쿠버네티스에서 리소스를 선택하는데 사용이 되는 기능이다. 하나의 리소스에 여러 라벨을 적용할 수 있고, Label Selector로
조건을 통해 선택할 수 있다.
~~~
selector:
  matchLabels:
    app: nginx
~~~

### Controller
기본 오브젝트들을 생성하고 관리해주는 역할을 가진다. 다음의 단계를 반복하면서 작동한다.
1. Observe: API 서버에서 현재 리소스 상태를 가져온다.
2. Compare: 사용자가 정의한 원하는 상태와 실제 상태를 비교한다.
3. Act: 필요 시 변경 조치를 취한다.
이 과정을 반복하며 이를 Control loop라고 한다.
- 종류
  - Replication Controller
    - Pod를 관리함. 지정된 수만큼 Pod를 유지한다.
    - Replicas: 지정한 Pod의 수, 유지 목표 Pod의 수
    - Pod template: 새 Pod를 만들 때 사용할 템플릿 정의
    - Pod Selector: 라벨로 어떤 Pod를 관리할지 선택
  - ReplicaSet
    - Replication Controller의 발전 버전
    - matchExpressions도 지원한다.
  - Deployment
    - 사용자가 원하는 상태에 따라 ReplicaSet을 자동으로 생성/관리하는 역할.
    - ReplicaSet의 상위 추상화 개념으로 이걸 통해 많이 사용.
    - 롤링 업데이트, 롤백등 

### 고급 Controller
위의 컨트롤러들이 단순한 워크로드를 처리했다고 하면 고급 컨트롤러는 특수한 워크로드를 처리한다.
- 유상태 서비스
- 배치 처리
- 데몬 프로세스

- 종류
  - DaemonSet: 모든 노드마다 하나씩 Pod를 실행하도록 보장하는 컨트롤러
    - 주로 로그 수집같은 서비스에 사용되는데 운영 환경에서는 노드마다 꼭 실행해야 하는 데몬이 있기 때문이다.
  - Job: 한 번만 실행되고 종료되는 작업을 관리하는 컨트롤러
    - Job에 의해서 관리되는 Pod는 Job이 종료되면 Pod를 같이 종료한다. 
  - Cron Jobs: 정해진 시간에 작업을 자동으로 실행하는 컨트롤러
    - 정해진 시간에 Job 객체를 생성하여 실행
  - StatefulSet: 상태를 가지는 애플리케이션을 안정적으로 배포하고 관리하기 위한 컨트롤러
    - 고유한 네트워크 식별자, 순차적 생성/삭제/스케일링, PVC 등을 보장한다.