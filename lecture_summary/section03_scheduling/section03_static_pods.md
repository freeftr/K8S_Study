# Section 03 Static Pods

Static Pod는 Kubelet이 자체적으로 관리하는 Pod이다.
- Control Plane 없이 동작 가능
- 로컬 파일 시스템의 디렉토리에 있는 Pod defintion 파일을 읽어 Pod 생성.

### 설정 방법
- kubelet.service.file 의 `--pod-manifest-path=/etc/kubernetes/manifests
  `
- kubeconfig.file에 `staticPodPath: /etc/kubernetes/manifests`

### 특징
Kubelet이 static pod를 생성하면, mirror pod를 API 서버에 등록해 조회가 가능하다.
- 수정, 삭제 불가
- yaml 파일 삭제해야 삭제, 수정해야 수정

쿠버네티스는 static pod를 가지고 control plane을 구성한다. 
- API Server
- Controller Manager
- Scheduler
- etcd

*Static Pod, DaemonSet 모두 kube-scheduler의 영향을 받지 않음.