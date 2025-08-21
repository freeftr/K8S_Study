# Section 08 Persistent Volume Claims

---
## PVC란?
- 사용자가 PV 요청 시 사용하는 객체
- k8s가 PVC와 PV를 자동으로 바인딩

### Binding 과정
1. 사용자가 PVC 생성
2. k8s가 PV 목록에서 확인
   - capacity
   - AccessMode
   - StorageClass
   - Label
3. 조건이 맞으면 1:1 바인딩
4. 안맞으면 Pending으로 있다가 PV 추가되면 바인딩.

### PVC definition
~~~
apiVersion: v1
kind PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
~~~

- Reclaim Policy: PVC 삭제 시 기존 PV 처리 옵션
  - Retain: 기본값으로 PV는 남고 다른 PVC 사용 불가
    - 관리자가 재설정 필요
  - Delete: 같이 삭제
    - 클라우드
