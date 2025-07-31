# Section 03 Admission Controllers

## Kubectl 흐름
***
1. API Server로 요청 전송
2. 인증: 요청을 보낸 사용자를 Kubeconfig에 설정된 인증서로 인증
3. 권한 부여: 작업 수행 권한 체크
   - RBAC: Rule Based Access Control
     - 누가 무엇을 할 수 있는가?
4. Admission Controller가 요청 검증
5. etcd 저장 후 작업

## Admission Controller
***
Admission Controller는 인증 및 권한 부여 이후에 실행되는 플러그인 시스템으로서
요청을 허용, 수정, 거부하는 역할은 가진다.
- 요청의 내용이 괜찮은가?

### 종류
- NamespaceExists: 존재하지 않는 네임스페이스에 대한 요청 거절
- NamespaceAutoProvision: 네임스페이스가 없으면 자동 생성
- AlwaysPullImages: 항상 이미지 다시 풀
- DefaultStorageClass: PVC에 기본 스토리지클래스 자동 지정
- EventRateLimit: API 요청 속도 제한
- NamespaceLifecycle: 네임스페이스 생명 주기 제어

### 명령어
- `kube-apiserver -h | grep enable-admission-plugins`: 현재 활성화된 플러그인 확인
- `kubectl -n kube-system exec -it <kube-apiserver-pod> -- kube-apiserver -h | grep enable-admission-plugins`
  - kubeadm 버전

kubeadm 기반일 경우 `/etc/kubernetes/manifests/kube-apiserver.yaml` 파일을 수정하면 된다.