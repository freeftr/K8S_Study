# Section 05 Rolling Updates and Rollbacks

### Rollout & Revision
- Rollout: deployment 생성 시 발생.
- Revision: 버전

## Deployment 전략
***

### Recreate
- 모든 기존 파드를 먼저 제거하고 새로운 버전의 파드를 생성
  - 교체 때 서비스 접근 불가

### Rolling Update
- 쿠버네티스 기본값
- 하나씩 기존 파드를 제거하고 새로운 버전의 파드를 하나씩 생성
  - 무중단 배포

### Update 방법
1. deployment definition 파일 수정
2. `kubectl apply -f deployment.yaml`

`kubectl set image deployment/my-deploy my-container=my-image:v2`로 바로 변경 가능
- yaml은 수정 안되니 유의

## Upgrade
***
1. deployment 생성 시 replica set 생성
2. 업데이트 시 새로운 replica set 생성
3. 롤링 업데이트 방식으로 하나씩 죽이고 새로운 replica set에 생성

### Rollback
- `kubectl rollout undo deployment/my-deploy`

### 전체 흐름
1. `kubectl create -f ~`
   - 생성
2. `kubectl get deployments`
   - 조회
3. `kubectl apply -f`
   - yaml 파일 적용
   - 또는 `kubectl set image` => yaml 파일은 안바뀜
4. `kubectl rollout status`
   - 현재 rollout 상태 보기
5. `kubectl history`
   - 이전 revision 히스토리 보기
6. `kubectl rollout undo`
   - 롤백