# Volume

Pod에 사용하는 저장소로 컨테이나가 아닌 "Pod"에 종속된다.
- 컨테이너 간 데이터 공유
- 컨테이너 데이터 유지 (휘발성이기 때문)

## 볼륨 생성 방식
***
1. 파드에 정의된 volumes 필드에서 하나 이상의 볼륨을 정의
2. 각 컨테이너의 volumeMounts 필드에서 해당 볼륨을 마운트할

## 볼륨 종류
***
쿠버네티스에서는 Pod의 로컬 디스크뿐만 아니라 외장 디스크까지 다양한 볼륨을 제공한다.

### emptyDir
Pod와 생명 주기를 같이 하는 임시 볼륨이다. -> 휘발성<br>
파드 내 여러 컨테이너가 공유할 수 있다.
- 기본적으로는 노드에서 할당해주는 물리적 디스크에 저장이 되나 `emptyDir.medium` 필드에 `Memory`라고 지정해주면
메모리에 저장이 된다.
~~~
apiVersion: v1
kind: Pod
metadata:
  name: shared-volume-pod
spec:
  containers:
  - name: writer
    image: busybox
    command: ["/bin/sh", "-c"]
    args:
      - echo "Hello from writer!" > /shared-data/hello.txt && sleep 3600;
    volumeMounts:
    - name: shared-volume  //-> 같은 볼륨 사용
      mountPath: /shared-data

  - name: reader
    image: busybox
    command: ["/bin/sh", "-c"]
    args:
      - sleep 5 && cat /shared-data/hello.txt && sleep 3600;
    volumeMounts:
    - name: shared-volume  //-> 같은 볼륨 사용
      mountPath: /shared-data

  volumes:
  - name: shared-volume
    emptyDir: {}
~~~

### hostPath
노드의 로컬 디스크의 경로(실제 경로)를 Pod에서 마운트해서 사용한다.
- Pod끼리도 공유가 가능하다. 

노드의 경로를 사용하기 때문에, 노드가 재시작되어 다른 노드에서 기동하면 다른 노드의 hostPath를 사용하기 때문에, 기존의 hostPath는 접근이
불가능하다. 
~~~
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-example
spec:
  containers:
  - name: app
    image: busybox
    command: ["/bin/sh", "-c"]
    args:
      - echo "Hello from container" > /host-data/hello.txt && sleep 3600;
    volumeMounts:
    - mountPath: /host-data
      name: myvolume

  volumes:
  - name: myvolume
    hostPath:
      path: /data //노드의 /data 디렉토리
      type: DirectoryOrCreate //없으면 자동 생성
~~~

### PersistentVolume and PersistentVolumeClaim
PV는 클러스터 관리자가 물리 디스크를 쿠버네티스에 표현한 것으로 추상화된 저장소 객체이다. PVC는 개발자가 이를 PV와 연결시키기 위해
조건에 맞는 PV를 연결하게 된다.

### PV
  - 옵션
    - capacity: 스토리지 용량
    - accessModes: 접근 모드
      - ReadWriteOnce: 하나의 Pod에만 마운트되고 하나의 Pod만 읽고 쓰기 가능
      - ReadOnlyMany: 여러 개의 Pod에 마운트가 가능하며, 여러 개의 Pod에서 동시 읽기 가능
      - ReadWriteMany: 여러 개 Pod 마운트, 읽기/쓰기 가능
    - ReclaimPolicy: PV를 재사용할때 기존 디스크 내용을 어떻게 처리할지에 대한 정책
      - Retain: 유지
      - Recycle: 기존 데이터 `rm -rf`로 삭제한 후 다른 PVC 받음
      - Delete: 볼륨 삭제
      - 디스크의 특성에 따라 가능한 것이 있고, 안되는 것이 있다.
    - VolumeMode: 파일시스템 또는 Raw
  - 생명 주기
    - available: 생성 후
    - bound: PVC에 바인딩 후
    - released: PVC 삭제 후 -> 보관 상태 
~~~
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
~~~

### PVC
~~~
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
~~~
위의 예제에 맞는 조건의 PV를 찾아 자동으로 바인딩한다.

### PV 동적 생성
따로 디스크를 생성하고 PV를 생성할 필요 없이 PVC를 정의하면 이에 맞는 디스크 및 PV를 자동으로 생성해준다.
- PVC에 필요한 디스크 용량 기재