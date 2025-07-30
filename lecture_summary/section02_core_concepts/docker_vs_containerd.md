# Docker vs Container D

쿠버네티스는 애초에 Docker의 컨테이너 오케스트레이션을 위해 만들어졌다. 쿠버네티스가 컨테이너 오케스트레이터로서 
각광받자 이제 다른 런타임 엔진의 필요성이 요구되었고, Container Runtime Interface(CRI)를 제공해 OCI 표준만
지키면 컨테이너 런타임을 만들 수 있게 해주었다.
- Open Container Initiative
  - imagespec: 이미지 빌드 방식의 기준
  - runtimespec

하지만 Docker는 CRI가 나오기 전에 만들어져 CRI를 지원하지 않았지만 주류였기 때문에, 쿠버네티스측은 dockershim을 통해
Docker가 CRI 없이도 동작하게 했다. 그런데 이 dockershim을 유지하기 위한 비용은 컸고, 지원을 그만하게 된다.

여기서 containerd는 Docker에서 분리된 경량 컨테이너 런타임으로서 CRI 호환 런타임이다.
- ctr: containerd의 CLI 도구이다. 순전히 디버깅용이기 때문에 굉장히 사용이 불편하다.
- nerdctl: containerd용 Docker와 유사한 CLI 도구이다. Docker가 지원하는 대부분의 기능을 제공한다. + containerd의 최신 기능 지원
- crictl: 쿠버측에서 CRI 호환 가능한 컨테이너 런타임과 상호작용하기 위해 만들어진 CLI다. 
  - 컨테이너 검사용으로 주로 사용 -> 디버깅 목적