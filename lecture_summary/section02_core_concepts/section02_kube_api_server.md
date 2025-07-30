# Kube API Server

Kube API Server는 Kubernetes의 주요 관리 컴포넌트다.
1. kubectl을 통해 명령어를 실행
2. Kube API Server가 요청을 검증하고 유효성 검사 수행
3. ETCD 클러스터에서 데이터를 가져와서 응답

*꼭 kubectl이 아니어도 된다.

Pod 생성 요청 예시
1. 사용자 인증
2. 요청 유효성 검사
3. 필요한 데이터 조회
4. Kube API Server가 노드에 할당하지 않은 상태로 Pod 객체 생성
5. 생성된 정보를 etcd에 저장하고 사용자에게 성공 응답 전달
6. 스케줄러가 Kube API Server를 감시하다가, 할당되지 않은 Pod 발견
7. 적절한 노드를 선택해 Kube API Server에 알려줌
8. Kube API Server가 etcd에 배정된 노드 정보를 갱신
9. Kube API Server가 해당 노드의 kubelet에게 전달
10. kubelet이 해당 노드에서 Pod를 실제로 생성
11. 컨테이너 런타임이 애플리케이션 이미지를 실행
12. 완료되면 kubelet이 상태 정보를 Kube API Server에 보고
13. Kube API Server는 이를 다시 etcd에 최종 반영

Kube API Server는 etcd에 유일하게 직접 접근할 수 있는 컴포넌트다.

## Kube API Server 설치
***
- kubeadm: Pod 형태로 마스터 노드의 `kube-system` 네임스페이스에 생성된다.
  - `/etc/kubernetes/manifests/kube-apiserver.yaml`
  - 위의 경로에서 옵션 목록 확인 가능
- scratch
  ~~~
    wget https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kube-apiserver
    chmod +x kube-apiserver
    sudo mv kube-apiserver /usr/local/bin/
  ~~~
- /etc/systemd/system/kube-apiserver.service
- `ps -ef | grep kube-apiserver`로 실행 중인 프로세스와 인자 확인 가능