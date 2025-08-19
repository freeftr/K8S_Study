# Section 07 Cluster Roles

- Role => Namespace 기반 <br>
- Cluster Role => Cluster Scoped
  - 클러스터 전체에 적용되는 권한
  - 모든 네임스페이스에 걸친 자원 접근 제어
- Cluster Role Binding: ClusterRole과 사용자를 연결

### 주의할 점
- ClusterRole은 클러스터 자원뿐 아니라 네임스페이스 자원에도 사용이 가능하다.
  - 모든 네임스페이스에 대해 권한이 열림.