# Section 06 Backup and Restore

### 백업 대상
- Resource Configuration
  - definition 파일등
  - declarative -> 깃헙 등 자동 백업
  - imperative -> API 서버에 직접 질의하여 백업
- Persistent Volume
  - 영속 스토리지 데이터
- etcd

## 백업 방법
***
### Kube API를 통한 백업
- `kubectl get all -all-namespaces -o yaml > cluster-backup.yaml`
- 위 명령어로 모든 yaml 추출해서 저장
- 자동화 도구
  - Velero

### ETCD
- 내장 스냅샷 기능 사용
~~~
ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /path/to/snapshot.db
~~~
- 상태 확인
~~~
ETCDCTL_API=3 etcdctl snapshot status /path/to/snapshot.db
~~~

### 복구 방법
1. Kube API 서버 중지
~~~
systemctl stop kube-apiserver
~~~
2. 스냅샷 복원
~~~
ETCDCTL_API=3 etcdctl \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot restore /path/to/snapshot.db \
  --data-dir=/var/lib/etcd-from-backup
~~~
3. etcd 설정 파일 수정
- 새 데이터 디렉토리 지정
4. 데몬 재로드, etcd 재시작
~~~
systemctl daemon-reload
systemctl restart etcd
~~~
5. Kube API 서버 재시작
~~~
systemctl start kube-apiserver
~~~