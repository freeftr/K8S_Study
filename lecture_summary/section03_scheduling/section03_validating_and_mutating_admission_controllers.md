# Section 03 Validating and Mutating Admission Controllers

Admission Controller는 크게 두 종류로 나눌 수 있다.
- Validating: 요청을 검증하고 허용/거절
- Mutating: 요청 내용을 변경하고 수정 후 처리
- Mutating -> Validating 순서

## 커스텀 Admission Controller 구성하기
***
쿠버네티스에서는 외부서버와 연동되는 웹훅 기반 Admission Controller를 지원한다.
- MutatingAdmissionWebhook
- ValidatingAdmissionWebhook

1. 웹훅 서버 준비
   - 요청과 응답은 AdmissionReview JSON 형식으로 한다.
2. 쿠버네티스에 웹훅 등록
   - MutatingAdmissionWebhook or ValidatingAdmissionWebhook 생성
~~~
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: example-validator
webhooks:
  - name: pod-policy.example.com
    clientConfig:
      service:
        name: webhook-service
        namespace: default
        path: "/validate"
      caBundle: <base64 encoded CA cert>
    rules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
    admissionReviewVersions: ["v1"]
    sideEffects: None
~~~   
   - `service`: 클러스터 내 서비스일 경우
   - `url`: 외부 URL
   - `caBundle`: API 서버와 웹훅 서버는 TLS로 통신하기 때문에 CA 인증서가 필요하다. 웹훅 서버는 서버 인증서를 TLS용으로 발급받아야 함.
