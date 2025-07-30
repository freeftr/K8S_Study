# Section 02 Imperative Vs Declarative

- Imperative: 무엇을 어떻게 할지 명령어로 직접 지시
- Declarative: 무엇이 되어야 하는지만 선언하고 시스템이 알아서 처리

### Imperative
- 상태 추적 어려움
- 빠르고 간편하지만 지속적 관리에 불리
- `kubectl run`

### Declarative
- yaml로 최종적 상태 선언
- `kubectl apply`
- Git에 저장 가능, 추적 용이


### 시험에서 팁
간단한 설정은 Imperative/복잡한 설정은 Declarative
- 빠른 수정은 `kubectl edit` (쿠버네티스 메모리에 있는 파일 수정하는 거임)
    - 로컬 파일 수정 후 replace하는게 협업에 좋다.