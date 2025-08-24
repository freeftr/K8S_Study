# Section 08 Storage Class

---
### Static Provisioning
- PVC가 PV에 바인딩되려면, 스토리지를 먼저 만들어야 한다.
  - "노가다" => 자동화 필요

### Dynamic Provisioning
- StorageClass를 사용하면 k8s가 알아서 스토리지를 생성 및 연결
- 사용자가 PVC를 만들면,
1. PVC가 특정 StorageClass를 참조
2. StorageClass가 정의된 Provisioner를 호출
3. 클라우드 시스템에서 실제 디스크 자동 생성
4. PV를 PVC에 바인딩.

### StorageClass definition
~~~
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/gce-pd   # GCP용 프로비저너
parameters:
  type: pd-standard                 # 디스크 유형 (standard or ssd)
  replication-type: none            # 복제 옵션 (none, regional-pd)
~~~

### PVC 설정
~~~
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard   # StorageClass 지정
~~~