# Section 02 Kubectl Apply

### Kubectl apply에서 사용하는 3가지 구성
- Local Config: 로컬에 저장된 yaml
- Live Object: 현재 클러스터에 존재하는 객체의 강태
- Last Applied Config: 이전에 apply로 적용된 yaml의 JSON 형식
  - 로컬 파일에서 필드가 삭제되었을 때 last applied에 없으면 라이브에서 삭제함.
  - 쿠버네티스 객체의 annotation 필드에 저장.
  
*중요한 것은 Imperative하고 섞어쓰면 상태 불일치 상태가 발생할 수 있으니 주의!!!