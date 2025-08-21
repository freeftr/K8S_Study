# Section 08 Persistent Volumes

---
## Persistent Volume
- 이전 강의의 volume은 Pod definition에 직접 설정을 했음.
  - 매번 하기 귀찮다.
- PV는 클러스터 관리자가 중앙에서 스토리지를 미리 Pool 형태로 구성해두고, 
사용자는 이를 요청(Claim)해서 사용.

### PV definition
~~~
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes:
      - ReadWriteOnce
  capacity:
    storage: 1Gi
  hostPath:
    path: /tmp/data
~~~
- `capacity`: 예약할 스토리지 크기
- `accessModes`
  - `ReadWriteOnce`: 단일 노드 read/write
  - `ReadOnlyMany`: 여러 노드에서 read-only
  - `ReadWriteMany`: 여러 노드에서 read/write
- `hostPath`는 외부 스토리지 사용 여부에 따라 수정 가능

### 동작 흐름
1. 관리자가 PV 생성하고 클러스터에 등록
2. 사용자는 PVC로 PV 요청
3. k8s가 PV와 PVC 바인딩