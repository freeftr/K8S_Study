# Section 07 Certificates Api

### CA Server
- 클러스터 내에서 인증서를 서명할 수 있는 루트 인증서와 개인키를 보관
- `kubeadm` 설치 시 마스터 노드가 CA

### 수동 인증서 발급
1. CSR을 생성해 기존 관리자에게 전달.
2. 기존 관리자가 CA 서버에서 CSR -> 인증서 발급
3. 이걸 받아서 새 관리자에게 전달

=> 계속 늘어날 경우 비효율적이다.

### Kubernetes Certificates API
1. 사용자가 CSR, 키 생성
2. 관리자가 CertificateSigningRequest 객체 생성
   - spec.request 필드에 Base64 인코딩된 내용으로 CSR 입력
3. `kubectl get csr`로 목록 확인
4. `kubectl certificate approve <csr-name>`으로 승인
5. Kubernetes Controller Manager가 CA 키로 서명해 인증서 발급
6. `kubectl get csr <csr-name> -o yaml`로 Base64 인코딩된 인증서를 확인 후 디코딩해 사용자에게 전달.

