# Section 08 Container Storage Interface

### CSI란?
- 스토리지를 컨테이너 오케스트레이터와 연결하는 인터페이스

### 동작 방식
- CSI는 RPC 기반의 명세를 정의.
- k8s는 Pod가 볼륨을 필요로 할 때 CSI RPC를 호출 -> 드라이버가 실제 동작을 처리
  - CreateVolume RPC => CSI 드라이버가 실제 스토리지에서 볼륨 생성
  - 이런식임.
  - CSI에는 파라미터, 출력 값, 에러 코드 등의 정의되어 있음.

### 이점
- 확장성: CSI 표준을 정함으로써 새 스토리지가 나와도 k8s에서는 변경이 필요없음.
